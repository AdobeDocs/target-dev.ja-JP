---
title: ' [!DNL Adobe Target] Node.js SDKでのイベントへのサブスクライブ'
description: '[!UICONTROL OnDeviceDecisioningHandler] オブジェクトを使用して Node.js SDK内で発生する様々なイベントを登録する方法について説明します。'
feature: APIs/SDKs
exl-id: 40c53840-a560-4819-ae04-f527c36b22fe
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 2%

---

# SDK イベント（Node.js）

## 説明

[SDKの初期化 &#x200B;](initialize-sdk.md) 時、`options.events` オブジェクトは、イベント名キーとコールバック関数値を持つオプションのオブジェクトです。 これを使用すると、SDK内で発生する様々なイベントを登録できます。 例えば、`clientReady` イベントは、SDKがメソッド呼び出しの準備ができたときに呼び出されるコールバック関数と共に使用できます。

コールバック関数が呼び出されると、イベントオブジェクトが渡されます。 各イベントには、イベント名に対応する `type` があります。 一部のイベントには、関連情報を含む追加のプロパティが含まれています。

## Events

| イベント名（タイプ） | 説明 | その他のイベントプロパティ |
| --- | --- | --- |
| clientReady | アーティファクトがダウンロードされ、SDKが `getOffers` 呼び出しに対応している場合に発行されます。 オンデバイス判定方法を使用する場合に推奨されます。 |  |
| artifactDownloadSuccessful | 新しいアーティファクトがダウンロードされるたびに発行されます。 | artifactPayload, artifactLocation |
| artifactDownloadFailed | アーティファクトのダウンロードに失敗するたびに生成されます。 | artifactLocation, エラー |

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
