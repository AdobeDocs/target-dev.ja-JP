---
title: Experience CloudのAAM セグメントとの統合
description: Experience Cloudとの統合、Audience Managerの統合
keywords: 配信 api, サーバーサイド，サーバーサイド，統合，audience manager, aam
exl-id: c21e0200-23ba-4a0b-adf4-38e03c087f00
feature: Implement Server-side
source-git-commit: e3f14e97fa48ffb1f07b29aca5711d16e75faa80
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 4%

---

# AAM セグメント

[!DNL Adobe Audience Manager] セグメントは、[!DNL Adobe Target] SDK を介して利用できます。 AAM セグメントを活用するには、次のフィールドを指定する必要があります。

>[!NOTE]
>
>AAM セグメントは、オンデバイス判定アクティビティではサポートされていません。

| フィールド名 | 必須 | 説明 |
| --- | --- | --- |
| `locationHint` | ○ | DCS の場所のヒントは、プロファイルを取得するためにどのAAM DCS エンドポイントにヒットするかを判断するために使用されます。 1 以上にする必要があります。 |
| `marketingCloudVisitorId` | ○ | Marketing Cloud 訪問者 ID |
| `blob` | ○ | AAM Blob は、追加データをAAMに送信するために使用されます。 サイズ &lt; 1024 の値を指定し、空白にすることはできません。 |

`getOffers` メソッドを呼び出すと SDK によってこれらのフィールドが自動的に入力されますが、有効な訪問者 cookie が指定されていることを確認する必要があります。 この cookie を取得するには、ブラウザーに VisitorAPI.js を実装する必要があります。

## 実装ガイド

### Cookie の使用

Cookie は、リクエストと [!DNL Adobe Target] リクエスト [!DNL Adobe Audience Manager] 関連付けるために使用されます。 この実装で使用する Cookie は次のとおりです。

| Cookie | 名前 | 説明 |
| --- | --- | --- |
| 訪問者 cookie | `AMCVS_XXXXXXXXXXXXXXXXXXXXXXXX%40AdobeOrg` | この cookie は、ターゲット `getOffers` 応答から `visitorState` を使用して初期化される場合に `VisitorAPI.js` によって設定されます。 |
| target の cookie | `mbox` | Web サーバーは、target `getOffers` 応答の `targetCookie` の名前と値を使用して、この cookie を設定する必要があります。 |

### 手順の概要

ユーザーがブラウザーに URL を入力し、そのブラウザーが web サーバーにリクエストを送信するとします。 そのリクエストを処理する場合：

1. サーバーは、リクエストから訪問者と Target の Cookie を読み取ります。
1. サーバーは [!DNL Target] SDK の `getOffers` メソッドを呼び出し、訪問者およびターゲット Cookie を指定します（使用可能な場合）。
1. `getOffers` 呼び出しが実行されると、応答の `targetCookie` と `visitorState` の値が使用されます。
   1. Cookie は、`targetCookie` から取得した値で応答に設定されます。 これは、target cookie を保持するようにブラウザーに指示する `Set-Cookie` 応答ヘッダーを使用して行われます。
   1. `VisitorAPI.js` を初期化し、ターゲットレスポンスから `visitorState` を渡すHTMLレスポンスが準備されます。
1. HTMLの応答がブラウザーに読み込まれます。
   1. `VisitorAPI.js` はドキュメントのヘッダーに含まれます。
   1. VisitorAPI は、`getOffers` SDK 応答からの `visitorState` で初期化されます。 これにより、訪問者 Cookie がブラウザーに設定され、後続のリクエストでサーバーに送信されます。

### サンプルコード

次のコードサンプルは、上記の各手順を実装しています。 各ステップは、コード内で、実装の横にインラインコメントとして表示されます。

#### Node.js

このサンプルは、[express、Node.js web フレームワーク ](https://expressjs.com/) に基づいています。

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

このサンプルでは、[spring、Java web フレームワーク ](https://spring.io/) を使用します。

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

`TargetRequestUtils.java` の詳細については、[ ユーティリティメソッド（Java） ](https://experienceleague.adobe.com/docs/target-dev/developer/server-side/java/utility-methods.html?lang=ja){target=_blank} を参照してください
