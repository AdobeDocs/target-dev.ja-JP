---
title: Node [!DNL Adobe Target] js SDK でのイベントへのサブスクライブ
description: '[!UICONTROL OnDeviceDecisioningHandler] オブジェクトを使用して Node.js SDK 内で発生する様々なイベントを登録する方法について説明します。'
feature: APIs/SDKs
exl-id: 40c53840-a560-4819-ae04-f527c36b22fe
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '163'
ht-degree: 2%

---

# SDK イベント（Node.js）

## 説明

[SDK を初期化 ](initialize-sdk.md) する際、`options.events` オブジェクトは、イベント名キーとコールバック関数値を持つオプションのオブジェクトです。 SDK 内で発生する様々なイベントをサブスクライブするために使用できます。 例えば、`clientReady` イベントは、SDK がメソッド呼び出しの準備ができたときに呼び出されるコールバック関数と共に使用できます。

コールバック関数が呼び出されると、イベントオブジェクトが渡されます。 各イベントには、イベント名に対応する `type` があります。 一部のイベントには、関連情報を含む追加のプロパティが含まれています。

## Events

| イベント名（タイプ） | 説明 | その他のイベントプロパティ |
| --- | --- | --- |
| clientReady | アーティファクトがダウンロードされ、SDK が `getOffers` 呼び出し用に準備されたときに発行されます。 オンデバイス判定方法を使用する場合に推奨されます。 |
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
