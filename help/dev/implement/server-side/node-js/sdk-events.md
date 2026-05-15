---
title: ' [!DNL Adobe Target] Node.js SDKでのイベントの購読'
description: '[!UICONTROL OnDeviceDecisioningHandler] オブジェクトを使用してNode.js SDK内で発生するさまざまなイベントを購読する方法を説明します。'
feature: APIs/SDKs
exl-id: 40c53840-a560-4819-ae04-f527c36b22fe
TQID: https://experienceleague.adobe.com/KWuJT-p-Er-1mx766Y-itlFn7REZnqkUksdHKCy-2-U
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 163
ht-degree: 2%

---

# SDK Events （Node.js）

## 説明

SDK](initialize-sdk.md)を[初期化する場合、`options.events` オブジェクトは、イベント名キーとコールバック関数値を持つオプションのオブジェクトです。 SDK内で発生するさまざまなイベントの購読に使用できます。 例えば、`clientReady` イベントは、SDKがメソッド呼び出しの準備ができたときに呼び出されるコールバック関数と共に使用できます。

コールバック関数が呼び出されると、イベントオブジェクトが渡されます。 各イベントには、イベント名に対応する`type`があります。 一部のイベントには、関連情報を含む追加のプロパティが含まれます。

## Events

| イベント名（タイプ） | 説明 | その他のイベントプロパティ |
| --- | --- | --- |
| clientReady | アーティファクトがダウンロードされ、SDKが`getOffers`呼び出しの準備ができたときに発行されます。 オンデバイス判定メソッドを使用する場合に推奨されます。 |  |
| artifactDownloadSucceeded | 新しいアーティファクトがダウンロードされるたびに実行されます。 | artifactPayload, artifactLocation |
| artifactDownloadFailed | アーティファクトのダウンロードに失敗するたびに実行されます。 | artifactLocation、エラー |

## 例

### Node.js

```js {line-numbers="true"}
const targetClient = TargetClient.create({
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device",
    events: {
        clientReady: onTargetClientReady,
        artifactDownloadSucceeded: onArtifactDownloadSucceeded,
        artifactDownloadFailed: onArtifactDownloadFailed
    }
});

function onTargetClientReady() {
    // make getOffers requests
    targetClient.getOffers({...})            
}

function onArtifactDownloadSucceeded(event) {
    console.log(`The artifact was successfully downloaded from '${event.artifactLocation}'`);
    // optionally do something with event.artifactPayload, like persist it
}

function onArtifactDownloadFailed(event) {
    console.log(`The artifact failed to download from '${event.artifactLocation}' with the following error message: ${event.error.message}`);
}
```
