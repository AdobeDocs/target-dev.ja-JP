---
title: オンデバイス決定ルールアーティファクトを自動的にダウンロード、保存、更新します
description: ' [!DNL Adobe Target] SDKの初期化中に、オンデバイス判定ルール アーティファクトを使用する方法について説明します。'
feature: APIs/SDKs
exl-id: be41a723-616f-4aa3-9a38-8143438bd18a
TQID: https://experienceleague.adobe.com/o4oNaCtd3PS1cDndSJHkI10pDke1DTaEnBn8u9pIQk8
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 7a5aae2510a014c6efaeee63080cde3e7746f91c
workflow-type: tm+mt
source-wordcount: 352
ht-degree: 0%

---

# [!DNL Adobe Target] SDKを介したルールアーティファクトの自動ダウンロード、保存、更新

この方法は、Web サーバーの初期化と起動を同時に[!DNL Adobe Target] SDKを初期化できる場合に最適です。 ルールアーティファクトは、[!DNL Adobe Target] SDKによってダウンロードされ、web サーバーアプリケーションがリクエストの処理を開始する前にメモリにキャッシュされます。 Web アプリケーションを起動して実行すると、メモリ内ルール アーティファクトを使用してすべての[!DNL Adobe Target]決定が実行されます。 キャッシュされたルール アーティファクトは、SDKの初期化手順で指定した`pollingInterval`に基づいて更新されます。

## 手順の概要

1. SDKのインストール
1. SDKの初期化
1. ルールアーティファクトの保存と使用

## &#x200B;1. SDKのインストール

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

## &#x200B;2. SDKの初期化

1. まず、SDKを読み込みます。 サーバーの起動を制御できるファイルと同じファイルにインポートします。

   **Node.js**

   ```javascript {line-numbers="true"}
   const TargetClient = require("@adobe/target-nodejs-sdk");
   ```

   **Java**

   ```javascript {line-numbers="true"}
   import com.adobe.target.edge.client.ClientConfig;
   import com.adobe.target.edge.client.TargetClient;
   ```

1. SDKを設定するには、create メソッドを使用します。

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

1. 次に示すように、**[!UICONTROL 管理]** > **[!UICONTROL 実装]**&#x200B;に移動すると、クライアントと組織Idの両方を[!DNL Adobe Target]から取得できます。

   <!-- Insert image-client-code.png -->
   ![Targetの管理下にある実装ページ ](assets/asset-rule-artifact-3.png)

## &#x200B;3. ルールアーティファクトの保存と使用

ルールアーティファクトを自分で管理する必要はなく、SDK メソッドの呼び出しは簡単である必要があります。

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
>上記のコードサンプルでは、`TargetClient` オブジェクトはメモリ内ルールアーティファクトへの参照を保持しています。 このオブジェクトを使用して標準のSDK メソッドを呼び出す場合、決定にはメモリ内ルールアーティファクトが使用されます。 クライアントのリクエストを初期化およびリッスンするファイル以外のファイルでSDK メソッドを呼び出す必要がある構造がアプリケーションにある場合や、それらのファイルがTargetClient オブジェクトにアクセスできない場合は、JSON ペイロードをダウンロードして、他のファイルで使用するローカル JSON ファイルに格納し、SDKを初期化する必要があります。 これは、次の節で、[JSON ペイロードを使用したルールアーティファクトのダウンロード ](rule-artifact-json.md)について説明します。

[!DNL Adobe Target] SDKを初期化した後にweb アプリケーションを開始する例を次に示します。

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
