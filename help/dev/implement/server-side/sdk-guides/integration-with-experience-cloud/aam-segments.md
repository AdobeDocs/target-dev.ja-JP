---
title: Adobe Experience Cloud AAMのセグメントとの統合
description: Adobe Experience CloudやAudience Managerと連携し
keywords: 配信api, サーバーサイド，サーバーサイド，統合，audience manager, aam
exl-id: c21e0200-23ba-4a0b-adf4-38e03c087f00
feature: Implement Server-side
TQID: https://experienceleague.adobe.com/mc55SxaUU8BJ81hKLji9xi0-OHCux3W4R0syuVoGrIo
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 431
ht-degree: 4%

---

# AAM セグメント

[!DNL Adobe Audience Manager]個のセグメントは[!DNL Adobe Target]個のSDKを使用して活用できます。 AAM セグメントを活用するには、次のフィールドを指定する必要があります。

>[!NOTE]
>
>AAM セグメントは、オンデバイス判定アクティビティではサポートされていません。

| フィールド名 | 必須 | 説明 |
| --- | --- | --- |
| `locationHint` | ○ | DCS Location Hintは、プロファイルを取得するためにヒットするAAM DCS エンドポイントを決定するために使用されます。 >= 1にする必要があります。 |
| `marketingCloudVisitorId` | ○ | Marketing Cloud 訪問者 ID |
| `blob` | ○ | AAM Blobは、AAMにデータを送信するために使用されます。 空白にせず、サイズ &lt;= 1024を指定してください。 |

SDKでは、`getOffers` メソッド呼び出しを行う際に、これらのフィールドが自動的に入力されますが、有効な訪問者Cookieが提供されていることを確認する必要があります。 このCookieを取得するには、ブラウザーにVisitorAPI.jsを実装する必要があります。

## 実装ガイド

### Cookieの使用

Cookieは、[!DNL Adobe Audience Manager]件のリクエストと[!DNL Adobe Target]件のリクエストを関連付けるために使用されます。 この実装で使用されるCookieです。

| Cookie | 名前 | 説明 |
| --- | --- | --- |
| 訪問者Cookie | `AMCVS_XXXXXXXXXXXXXXXXXXXXXXXX%40AdobeOrg` | このCookieは、ターゲット `getOffers`応答から`visitorState`で初期化されたときに`VisitorAPI.js`によって設定されます。 |
| ターゲット Cookie | `mbox` | Web サーバーは、ターゲット `getOffers`応答の`targetCookie`の名前と値を使用してこのCookieを設定する必要があります。 |

### 手順の概要

ユーザーがWeb サーバーにリクエストを送信するブラウザーにURLを入力するとします。 その要求を満たす場合：

1. サーバーは、リクエストから訪問者とターゲット Cookieを読み取ります。
1. サーバーは[!DNL Target] SDKの`getOffers` メソッドを呼び出し、訪問者とターゲット Cookieが使用可能な場合は指定します。
1. `getOffers`呼び出しが満たされると、応答の`targetCookie`と`visitorState`の値が使用されます。
   1. `targetCookie`から取得した値を使用して、応答にCookieが設定されます。 これは、`Set-Cookie`応答ヘッダーを使用して実行されます。このヘッダーは、ターゲット Cookieを保持するようにブラウザーに指示します。
   1. `VisitorAPI.js`を初期化し、ターゲット応答から`visitorState`に渡すHTML応答が用意されています。
1. HTMLの応答がブラウザーに読み込まれます…
   1. `VisitorAPI.js`が文書ヘッダーに含まれています。
   1. VisitorAPIは、`getOffers` SDK応答から`visitorState`で初期化されます。 これにより、訪問者のCookieがブラウザーで設定され、その後のリクエストでサーバーに送信されます。

### サンプルコード

次のコードサンプルは、上記の各手順を実装しています。 各ステップは、実装の横にインラインコメントとしてコードに表示されます。

#### Node.js

このサンプルは、Node.js web フレームワーク ](https://expressjs.com/)である[expressに依存しています。

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

このサンプルでは、Java web フレームワーク ](https://spring.io/)である[springを使用しています。

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

`TargetRequestUtils.java`について詳しくは、[ ユーティリティ メソッド （Java） ](https://experienceleague.adobe.com/docs/target-dev/developer/server-side/java/utility-methods.html){target=_blank}を参照してください
