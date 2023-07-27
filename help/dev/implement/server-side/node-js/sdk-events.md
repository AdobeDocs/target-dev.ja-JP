---
title: のイベントを購読する [!DNL Adobe Target] Node.js SDK
description: を使用して、Node.js SDK 内で発生する様々なイベントを購読する方法について説明します。 [!UICONTROL OnDeviceDecisioningHandler] オブジェクト。
feature: APIs/SDKs
exl-id: 40c53840-a560-4819-ae04-f527c36b22fe
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '164'
ht-degree: 2%

---

# SDK イベント (Node.js)

## 説明

条件 [SDK の初期化](initialize-sdk.md)、 `options.events` オブジェクトは、イベント名キーとコールバック関数値を持つオプションのオブジェクトです。 SDK 内で発生する様々なイベントをサブスクライブする場合に使用できます。 例えば、 `clientReady` イベントは、SDK がメソッド呼び出しの準備ができたときに呼び出されるコールバック関数と共に使用することができます。

コールバック関数が呼び出されると、イベントオブジェクトが渡されます。 各イベントには `type` イベント名に対応します。 一部のイベントには、関連情報を含む追加のプロパティが含まれます。

## Events

| イベント名（タイプ） | 説明 | 追加のイベントプロパティ |
| --- | --- | --- |
| clientReady | アーティファクトがダウンロードされ、SDK が次の準備ができたときに発生します。 `getOffers` 呼び出し。 オンデバイス判定方法を使用する場合に推奨されます。 |
| artifactDownloadSucceeded | 新しいアーティファクトがダウンロードされるたびに発生します。 | artifactPayload, artifactLocation |
| artifactDownloadFailed | アーティファクトのダウンロードが失敗するたびに発生します。 | artifactLocation, error |

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
