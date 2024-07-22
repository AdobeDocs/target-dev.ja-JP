---
title: オンデバイス判定ルールアーティファクトの自動的なダウンロード、保存、更新
description: ' [!DNL Adobe Target] SDK の初期化中にオンデバイス判定ルールアーティファクトを操作する方法を説明します。'
feature: APIs/SDKs
exl-id: be41a723-616f-4aa3-9a38-8143438bd18a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '347'
ht-degree: 0%

---

# [!DNL Adobe Target] SDK を使用したルールアーティファクトの自動ダウンロード、保存および更新

Web サーバーの初期化と開始を同時に行いながら [!DNL Adobe Target] SDK を初期化できる場合は、この方法が最適です。 ルールアーティファクトは、web サーバーアプリケーションがリクエストの処理を開始する前に、[!DNL Adobe Target] SDK によってダウンロードされ、メモリにキャッシュされます。 Web アプリケーションが起動して実行されると、すべて [!DNL Adobe Target] 決定がメモリ内ルールアーティファクトを使用して実行されます。 キャッシュされたルールアーティファクトは、SDK 初期化手順で指定した `pollingInterval` に基づいて更新されます。

## 手順の概要

1. SDK のインストール
1. SDK の初期化
1. ルールアーティファクトの保存と使用

## 1. SDK をインストールする

>[!BEGINTABS]

>[!TAB NPM]

```javascript {line-numbers="true"}
npm i @adobe/target-nodejs-sdk -P
```

>[!TAB MVN]

```javascript {line-numbers="true"}
<dependency>
    <groupId>com.adobe.target</groupId>
    <artifactId>java-sdk</artifactId>
    <version>1.0</version>
</dependency>
```

>[!ENDTABS]

## 2. SDK の初期化

1. まず、SDK を読み込みます。 サーバーの起動を制御できるのと同じファイルにを読み込みます。

   **Node.js**

   ```javascript {line-numbers="true"}
   const TargetClient = require("@adobe/target-nodejs-sdk");
   ```

   **Java**

   ```javascript {line-numbers="true"}
   import com.adobe.target.edge.client.ClientConfig;
   import com.adobe.target.edge.client.TargetClient;
   ```

1. SDK を設定するには、create メソッドを使用します。

   **Node.js**

   ```javascript {line-numbers="true"}
   const CONFIG = {
       client: "<your target client code>",
       organizationId: "your EC org id",
       decisioningMethod: "on-device",
       pollingInterval : 300000,
       events: {
           clientReady: startWebServer
       }
   };
   
   const TargetClient = TargetClient.create(CONFIG);
   
   function startWebServer() {
       //Adobe Target SDK has now downloaded the JSON Artifacts and is available in the memory.
       //You can start your web server now to serve requests now.
   }
   ```

   **Java**

   ```javascript {line-numbers="true"}
   ClientConfig config = ClientConfig.builder()
       .client("<you target client code>")
       .organizationId("<your EC org id>")
       .build();
   TargetClient targetClient = TargetClient.create(config);
   ```

1. ここに示すように、**[!UICONTROL Administration]** / **[!UICONTROL Implementation]** に移動すると、client と organizationId の両方を [!DNL Adobe Target] から取得できます。

   &lt;!— image-client-code.png を挿入 – >
   ![Target 管理の実装ページ ](assets/asset-rule-artifact-3.png)

## 3. ルールアーティファクトの保存と使用

ルールアーティファクトを自分で管理する必要はなく、SDK メソッドの呼び出しは簡単です。

>[!BEGINTABS]

>[!TAB Node.js]

```javascript {line-numbers="true"}
//req is the request object from the server request listener method
const targetCookie = req.cookies[TargetClient.TargetCookieName];
const request = {
    context: {
        channel: "web"
    },
    execute: {
        mboxes: [{
            address: { url: req.headers.host + req.originalUrl },
            name: "on-device-homepage-banner"
        }],
    },
};

TargetClient.getOffers({
    request,
    targetCookie
}).then(function(response) {
    //This Target response is coming from the In-memory Target artifact.
    console.log("Target response", response);
}).catch(function(err) {
    console.error("Target:", err);
})
```

>[!TAB Java]

```java {line-numbers="true"}
MboxRequest mbox = new MboxRequest().name("homepage").index(0);
TargetDeliveryRequest request = TargetDeliveryRequest.builder()
    .context(new Context().channel(ChannelType.WEB))
    .execute(new ExecuteRequest().mboxes(Arrays.asList(mbox)))
    .build();
TargetDeliveryResponse response = targetClient.getOffers(request);
```

>[!ENDTABS]

>[!NOTE]
>
>上記のコードサンプルでは、`TargetClient` オブジェクトは、メモリ内ルールアーティファクトへの参照を保持します。 このオブジェクトを標準 SDK メソッドの呼び出しに使用する場合、決定にはメモリ内ルールアーティファクトが使用されます。 クライアントリクエストを初期化してリッスンするファイル以外のファイルで SDK メソッドを呼び出す必要がある構造のアプリケーションで、それらのファイルが TargetClient オブジェクトにアクセスできない場合、JSON ペイロードをダウンロードしてローカル JSON ファイルに保存し、他のファイルで使用して SDK を初期化する必要があるファイルに保存できます。 これについては、次の節 [JSON ペイロードを使用したルールアーティファクトのダウンロード ](rule-artifact-json.md) で説明します。

[!DNL Adobe Target] SDK を初期化した後に web アプリケーションを開始する例を次に示します。

>[!BEGINTABS]

>[!TAB Node.js]

```javascript {line-numbers="true"}
const express = require("express");
const cookieParser = require("cookie-parser");
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
    client: "<your target client code>",
    organizationId: "your EC org id",
    decisioningMethod: "on-device",
    pollingInterval : 300000,
    events: {
        clientReady: startWebServer
    }
};

const app = express();
const targetClient = TargetClient.create(CONFIG);

app.use(cookieParser());

function saveCookie(res, cookie) {
  if (!cookie) {
    return;
  }

  res.cookie(cookie.name, cookie.value, {maxAge: cookie.maxAge * 1000});
}

const getResponseHeaders = () => ({
  "Content-Type": "text/html",
  "Expires": new Date().toUTCString()
});

function sendSuccessResponse(res, response) {
  res.set(getResponseHeaders());
  saveCookie(res, response.targetCookie);
  res.status(200).send(response);
}

function sendErrorResponse(res, error) {
  res.set(getResponseHeaders());
  res.status(500).send(error);
}

function startWebServer() {
    app.get('/*', async (req, res) => {
    const targetCookie = req.cookies[TargetClient.TargetCookieName];
    const request = {
        execute: {
        mboxes: [{
            address: { url: req.headers.host + req.originalUrl },
            name: "on-device-homepage-banner" // Ensure that you have a LIVE Activity running on this location
        }]
        }};

    try {
        const response = await targetClient.getOffers({ request, targetCookie });
        sendSuccessResponse(res, response);
    } catch (error) {
        console.error("Target:", error);
        sendErrorResponse(res, error);
    }
    });

    app.listen(3000, function () {
    console.log("Listening on port 3000 and watching!");
    });
}
```

>[!TAB Java]

```java {line-numbers="true"}
import com.adobe.target.edge.client.ClientConfig;
import com.adobe.target.edge.client.TargetClient;
import com.adobe.target.delivery.v1.model.ChannelType;
import com.adobe.target.delivery.v1.model.Context;
import com.adobe.target.delivery.v1.model.ExecuteRequest;
import com.adobe.target.delivery.v1.model.MboxRequest;
import com.adobe.target.edge.client.entities.TargetDeliveryRequest;
import com.adobe.target.edge.client.model.TargetDeliveryResponse;

@Controller
public class TargetController {

  private TargetClient targetClient;

  TargetController() {
    // You should instantiate TargetClient in a Bean and inject the instance into this class 
    // but we show the code here for demonstration purpose.
    ClientConfig config = ClientConfig.builder()
        .client("<you target client code>")
        .organizationId("<your EC org id>")
        .build();
    targetClient = TargetClient.create(config);
  }

  @GetMapping("/")
  public String homePage() {
    MboxRequest mbox = new MboxRequest().name("homepage").index(0);
    TargetDeliveryRequest request = TargetDeliveryRequest.builder()
        .context(new Context().channel(ChannelType.WEB))
        .execute(new ExecuteRequest().mboxes(Arrays.asList(mbox)))
        .build();
    TargetDeliveryResponse response = targetClient.getOffers(request);
    // ...
  }
}
```

>[!ENDTABS]
