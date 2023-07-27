---
title: JSON ペイロードを使用して、オンデバイス判定ルールアーティファクトをダウンロード、保存、更新します。
description: この方法は、SDK メソッドを使用する各ファイルで SDK を初期化する必要がある方法でアプリケーションが構造化されている場合に最適です。
feature: APIs/SDKs
exl-id: 4ccfb455-f813-4bdb-a9c1-d576a110a9bb
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '291'
ht-degree: 1%

---

# JSON ペイロードを使用したルールアーティファクトのダウンロード、保存、更新

この方法は、SDK メソッドを使用する各ファイルで SDK を初期化する必要がある方法でアプリケーションが構造化されている場合に最適です。 SDK の初期化中に Web アプリケーションがルールアーティファクトの JSON ペイロードを使用できるようにする前に、JSON ペイロードがダウンロードされ、アプリケーションで使用できることを確認する必要があります。

## 手順の概要

1. SDK のインストール
1. SDK の初期化
1. JSON ペイロードの保存と使用

## 1. SDK をインストールします。

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

## 2. SDK を初期化します

1. まず、SDK を読み込みます。 サーバーの起動を制御できる同じファイルにインポートします。

   **Node.js**

   ```javascript {line-numbers="true"}
   const TargetClient = require("@adobe/target-nodejs-sdk");
   ```

   **Java**

   ```javascript {line-numbers="true"}
   import com.adobe.target.edge.client.ClientConfig;
   import com.adobe.target.edge.client.TargetClient;
   ```

1. SDK を設定するには、 create メソッドを使用します。

   **Node.js**

   ```javascript {line-numbers="true"}
   const CONFIG = {
       client: "<your target client code>",
       organizationId: "your EC org id",
       decisioningMethod: "on-device",
       pollingInterval : 300000,
       events: {
           artifactDownloadSucceeded: onArtifactDownloadSucceeded,
           artifactDownloadFailed: onArtifactDownloadFailed
       }
   };
   
   const TargetClient = TargetClient.create(CONFIG);
   
   function onArtifactDownloadSucceeded(event) {
       //Adobe Target SDK has now downloaded the JSON Artifact/Payload
       console.log(event.artifactLocation) // Location from where the Artifact is downloaded.
       console.log(event.artifactPayload) // JSON Payload which we can store locally.
   }
   
   function onArtifactDownloadFailed(event) {
       //Adobe Target SDK has failed to download the JSON Artifact/Payload.
       console.log(event.artifactLocation) // Location from where the Artifact is downloaded.
       console.log(event.error.message) // Error message
   }
   ```

   **Java**

   ```javascript {line-numbers="true"}
   package com.adobe.target.edge.client.model.ondevice.OnDeviceDecisioningHandler;
   
   ClientConfig config = ClientConfig.builder()
       .client("<you target client code>")
       .organizationId("<your EC org id>")
       .onDeviceDecisioningHandler(
         new OnDeviceDecisioningHandler() {
           void onDeviceDecisioningReady() {
             // On-Device Decision is ready.
           }
           void artifactDownloadSucceeded(byte[] artifactData) {
             // Store artifactData to local disk.        
             // ...
           }
         }
       )
       .build();
   TargetClient targetClient = TargetClient.create(config);
   ```

1. クライアントと `organizationId` ～から取り出すことができる [!DNL Adobe Target]移動して **[!UICONTROL 管理]** > **[!UICONTROL 実装]**、ここに示すように。

   &lt;!— image-client-code.png を挿入 —>
   ![代替画像](assets/asset-rule-artifact-3.png)

## 3. JSON ペイロードを保存して削除する

JSON ペイロードを保存するために使用するメカニズムは、システムアーキテクチャによって異なります。 ローカルファイル、データベース、または Memcached などのメモリオブジェクトキャッシュシステムを使用できます。 使用するには、アプリケーションからこの JSON を読み取れる必要があります。 このガイドでは、ローカルファイルをストレージとして使用します。

>[!BEGINTABS]

>[!TAB Node.js]

```javascript {line-numbers="true"}
//... Code removed for brevity

function onArtifactDownloadSucceeded(event) {
    const jsonPath = 'src/config/target-rules.json'
    fs.writeFile(jsonPath, JSON.stringify(event.artifactPayload), 'utf8', (err) => {
        if (err) {
            throw err;
        };
        console.log(`The artifact from '${event.artifactLocation}' is now saved to '${jsonPath}'`);
    });
}


function onArtifactDownloadFailed(event) {
  console.log(`The local decisioning artifact failed to download from '${event.artifactLocation}' with the following error message: ${event.error.message}`);
}

//... Code removed for brevity
```

>[!TAB Java]

```javascript {line-numbers="true"}
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
>を初期化することで、 [!DNL Adobe Target]JSON ペイロードを介した SDK では、サーバーは、 [!DNL Adobe Target]SDK は、ルールアーティファクトがダウンロードされるのを待つ必要はありません。

JSON ペイロード初期化機能の例を以下に示します。

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
        clientReady : startWebServer,
        artifactDownloadSucceeded : onArtifactDownloadSucceeded,
        artifactDownloadFailed : onArtifactDownloadFailed
    },
};

function onArtifactDownloadSucceeded(event) {
    const jsonPath = 'src/config/target-rules.json'
    fs.writeFile(jsonPath, JSON.stringify(event.artifactPayload), 'utf8', (err) => {
        if (err) {
            throw err;
        };
        console.log(`The artifact from '${event.artifactLocation}' is now saved to '${jsonPath}'`);
    });
}

function onArtifactDownloadFailed(event) {
  console.log(`The on-device decisioning artifact failed to download from '${event.artifactLocation}' with the following error message: ${event.error.message}`);
}

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

```javascript {line-numbers="true"}
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
        .onDeviceDecisioningHandler(
          new OnDeviceDecisioningHandler() {
            void onDeviceDecisioningReady() {
              // On-Device Decision is ready.
            }
            void artifactDownloadSucceeded(byte[] artifactData) {
              // Store artifactData to local disk.        
              // ...
            }
          }
        )
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
