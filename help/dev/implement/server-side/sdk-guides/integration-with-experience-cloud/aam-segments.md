---
title: Experience CloudAAMセグメントとの統合
description: Experience Cloudとの統合、Audience Manager統合
keywords: 配信 api，サーバー側，サーバー側，統合， audience manager, aam
exl-id: c21e0200-23ba-4a0b-adf4-38e03c087f00
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '425'
ht-degree: 4%

---

# AAM セグメント

[!DNL Adobe Audience Manager] セグメントは、 [!DNL Adobe Target] SDK. AAMセグメントを活用するには、次のフィールドを指定する必要があります。

>[!NOTE]
>
>AAMセグメントは、オンデバイス判定アクティビティではサポートされていません。

| フィールド名 | 必須 | 説明 |
| --- | --- | --- |
| `locationHint` | ○ | DCS ロケーションヒントは、プロファイルを取得するために、どのAAM DCS エンドポイントをヒットするかを決定するために使用されます。 >= 1 である必要があります。 |
| `marketingCloudVisitorId` | ○ | Marketing Cloud 訪問者 ID |
| `blob` | ○ | AAM Blob は、追加データをAAMに送信するために使用されます。 空白およびサイズ &lt;= 1024 は指定できません。 |

SDK は、 `getOffers` メソッド呼び出しをおこなう必要がありますが、有効な訪問者 cookie が提供されていることを確認する必要があります。 この cookie を取得するには、ブラウザーに VisitorAPI.js を実装する必要があります。

## 実装ガイド

### Cookie の使用

cookie は相互に関連付けるために使用されます。 [!DNL Adobe Audience Manager] リクエストと [!DNL Adobe Target] リクエスト。 これらは、この実装で使用される cookie です。

| Cookie | 名前 | 説明 |
| --- | --- | --- |
| 訪問者の cookie | `AMCVS_XXXXXXXXXXXXXXXXXXXXXXXX%40AdobeOrg` | この cookie は、 `VisitorAPI.js` を使用して初期化された場合 `visitorState` ターゲットから `getOffers` 応答。 |
| ターゲット Cookie | `mbox` | Web サーバーでは、 `targetCookie` ターゲットから `getOffers` 応答。 |

### 手順の概要

ユーザーがブラウザーに URL を入力し、Web サーバーにリクエストを送信するとします。 そのリクエストを満たす場合：

1. サーバーがリクエストから訪問者 Cookie を読み取り、ターゲット Cookie を設定します。
1. サーバーが `getOffers` メソッド [!DNL Target] SDK( 訪問者とターゲットの cookie を指定する（可能な場合）。
1. 次の場合に `getOffers` 呼び出しが満たされます。値は `targetCookie` および `visitorState` を返します。
   1. Cookie は、 `targetCookie`. これは、 `Set-Cookie` 応答ヘッダー。ブラウザーに対して、ターゲットの cookie を保持するよう指示します。
   1. 初期化するHTML応答が準備されます。 `VisitorAPI.js` そして、 `visitorState` を target 応答から取得します。
1. HTML応答がブラウザーに読み込まれました…
   1. `VisitorAPI.js` はドキュメントヘッダーに含まれます。
   1. を使用して VisitorAPI が初期化されました。 `visitorState` から `getOffers` SDK の応答。 これにより、訪問者 Cookie がブラウザーに設定され、以降のリクエストでサーバーに送信されます。

### サンプルコード

次のコードサンプルは、上記の各手順を実装します。 各手順は、コード内で実装の横にインラインコメントとして表示されます。

#### Node.js

このサンプルは、 [式， Node.js Web フレームワーク](https://expressjs.com/).

>[!BEGINTABS]

>[!TAB server.js]

```js {line-numbers="true"}
const fs = require("fs");
const express = require("express");
const cookieParser = require("cookie-parser");
const Handlebars = require("handlebars");
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg",
  timeout: 10000,
  logger: console,
};
const targetClient = TargetClient.create(CONFIG);
const TEMPLATE = fs.readFileSync(`${__dirname}/index.handlebars`).toString();
const handlebarsTemplate = Handlebars.compile(TEMPLATE);

Handlebars.registerHelper("toJSON", function (object) {
  return new Handlebars.SafeString(JSON.stringify(object, null, 4));
});

const app = express();
app.use(cookieParser());
app.use(express.static(__dirname + "/public"));

app.get("/", async (req, res) => {
  // The server reads the visitor and target cookies from the request.
  const visitorCookie =
    req.cookies[
      encodeURIComponent(
        TargetClient.getVisitorCookieName(CONFIG.organizationId)
      )
    ];
  const targetCookie = req.cookies[TargetClient.TargetCookieName];
  const address = { url: req.headers.host + req.originalUrl };

  const targetRequest = {
    execute: {
      mboxes: [
        { name: "homepage", index: 1, address },
        { name: "SummerShoesOffer", index: 2, address },
        { name: "SummerDressOffer", index: 3, address }
      ],
    },
  };

  res.set({
    "Content-Type": "text/html",
    Expires: new Date().toUTCString(),
  });

  try {
    // The server makes a call to the `getOffers` method of the Target SDK specifying the visitor and target cookies if available.
    const targetResponse = await targetClient.getOffers({
      request: targetRequest,
      visitorCookie,
      targetCookie,
    });

    // When the `getOffers` call is fulfilled, values for `targetCookie` and `visitorState` from the response are used.
    // A cookie is set on the response with values taken from `targetCookie`.  This is done using the `Set-Cookie` response header which tells the browser to persist the target cookie.
    res.cookie(
      targetResponse.targetCookie.name,
      targetResponse.targetCookie.value,
      { maxAge: targetResponse.targetCookie.maxAge * 1000 }
    );

    // An HTML response is prepared that initializes VisitorAPI.js and passes in `visitorState` from the target response.
    const html = handlebarsTemplate({
      organizationId: CONFIG.organizationId,
      targetResponse,
    });

    res.status(200).send(html);
  } catch (error) {
    console.error("Target:", error);
    res.status(500).send(error);
  }
});

app.listen(3000, function () {
  console.log("Listening on port 3000 and watching!");
});
```

>[!TAB index.handlebars]

```html {line-numbers="true"}
<!doctype html>
<html>
<head>
  <meta charset="UTF-8">
  <title>ECID (Visitor API) Integration Sample</title>

  <!-- VisitorAPI.js is included in the document header. -->
  <script src="VisitorAPI.js"></script>
  <script>
    // VisitorAPI is initialized with visitorState from the `getOffers` SDK response. This will cause the visitor cookie to be set in the browser so it will be sent to the server on subsequent requests.
    Visitor.getInstance("{{organizationId}}", {serverState: {{toJSON targetResponse.visitorState}} });
  </script>
</head>
<body>
  <h1>response</h1>
  <pre>{{toJSON targetResponse}}</pre>
</body>
</html>
```

>[!ENDTABS]

#### Java

このサンプルでは、 [spring, Java web framework](https://spring.io/).

>[!BEGINTABS]

>[!TAB ClientSampleApplication.java]

```java {line-numbers="true"}
@SpringBootApplication
public class ClientSampleApplication {

    public static void main(String[] args) {
        System.setProperty(SimpleLogger.DEFAULT_LOG_LEVEL_KEY, "DEBUG");
        SpringApplication.run(ClientSampleApplication.class, args);
    }

    @Bean
    TargetClient marketingCloudClient() {
        ClientConfig clientConfig = ClientConfig.builder()
                .client("acmeclient")
                .organizationId("1234567890@AdobeOrg")
                .defaultDecisioningMethod(DecisioningMethod.SERVER_SIDE)
                .build();

        return TargetClient.create(clientConfig);
    }
}
```

>[!TAB TargetController.java]

```java {line-numbers="true"}
@Controller
@RequestMapping("/")
public class TargetController {

    @Autowired
    private TargetClientService targetClientService;

    @GetMapping
    public String index(Model model, HttpServletRequest request, HttpServletResponse response) {
        // The server reads the visitor and target cookies from the request.
        List<TargetCookie> targetCookies = getTargetCookies(request.getCookies());

        Address address = getAddress(request);

        List<MboxRequest> mboxRequests = new ArrayList<>();
        mboxRequests.add((MboxRequest) new MboxRequest().name("homepage").index(1).address(address));
        mboxRequests.add((MboxRequest) new MboxRequest().name("SummerShoesOffer").index(2).address(address));
        mboxRequests.add((MboxRequest) new MboxRequest().name("SummerDressOffer").index(3).address(address));

        TargetDeliveryResponse targetDeliveryResponse = targetClientService.getOffers(mboxRequests, targetCookies, request,
                response);

        // An HTML response is prepared that initializes VisitorAPI.js and passes in `visitorState` from the target response.
        model.addAttribute("visitorState", targetDeliveryResponse.getVisitorState());
        model.addAttribute("targetResponse", targetDeliveryResponse);
        model.addAttribute("organizationId", "1234567890@AdobeOrg");

        return "index";
    }
}
```

>[!TAB TargetClientService.java]

```java {line-numbers="true"}
@Service
public class TargetClientService {

    private final TargetClient targetJavaClient;

    public TargetClientService(TargetClient targetJavaClient) {
        this.targetJavaClient = targetJavaClient;
    }

    public TargetDeliveryResponse getOffers(List<MboxRequest> executeMboxes, List<TargetCookie> cookies, HttpServletRequest request, HttpServletResponse response) {

        Context context = getContext(request);
        ExecuteRequest executeRequest = new ExecuteRequest();
        executeRequest.setMboxes(executeMboxes);

        TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
                .context(context)
                .execute(executeRequest)
                .cookies(cookies)
                .build();

        // The server makes a call to the `getOffers` method of the Target SDK specifying the visitor and target cookies if available.
        TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);

        // When the `getOffers` call is fulfilled, values for `targetCookie` and `visitorState` from the response are used.
        // A cookie is set on the response with values taken from `targetCookie`.  This is done using the `Set-Cookie` response header which tells the browser to persist the target cookie.
        setCookies(targetResponse.getCookies(), response);
        return targetResponse;
    }
}
```

>[!TAB TargetRequestUtils.java]

```java {line-numbers="true"}
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Target Only : GetOffer</title>

    <!-- VisitorAPI.js is included in the document header. -->
    <script src="../../js/VisitorAPI.js"></script>
    <script th:inline="javascript">
        // VisitorAPI is initialized with visitorState from the `getOffers` SDK response. This will cause the visitor cookie to be set in the browser so it will be sent to the server on subsequent requests.
        Visitor.getInstance(/*[[${organizationId}]]*/ "", {serverState: /*[[${visitorState}]]*/ {}});
    </script>
</head>
<body>
    <h1>response</h1>
    <pre>[[${targetResponse}]]</pre>
</body>
</html>
```

>[!ENDTABS]

TargetRequestUtils.java について詳しくは、 [ユーティリティメソッド (Java)](https://experienceleague.corp.adobe.com/docs/target-dev/developer/server-side/java/utility-methods.html){target=_blank}
