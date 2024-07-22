---
title: ' [!DNL Adobe Target] .NET SDK でのイベントのサブスクライブ'
description: '[!UICONTROL OnDeviceDecisioningHandler] オブジェクトを使用して.NET SDK 内で発生する様々なイベントを購読する方法について説明します。'
feature: APIs/SDKs
exl-id: 7578033f-3de5-4d13-9739-46ad1269ec5f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '120'
ht-degree: 5%

---

# SDK イベント （.NET）

## 説明

[SDK を初期化 ](initialize-sdk.md) する際に、オプションの `OnDeviceDecisioningReady` デリゲートを `TargetClientConfig` オブジェクトに指定できます。このオブジェクトは、SDK がオンデバイスメソッド呼び出しの準備ができたときに呼び出されます。 [!UICONTROL on-device decisioning] アーティファクトのダウンロードを処理するために使用できるその他のデリゲートもいくつかあります。

## Events

特定のイベントに対して、次のデリゲートを設定できます。

| 名前 | 引数 | 説明 |
| --- | --- | --- |
| OnDeviceDecisioningReady | None | クライアントの [!UICONTROL on-device decisioning] ークフロー準備が初めて完了したときにのみ呼び出されます。 |
| ArtifactDownloadSuccessful | アーティファクトファイルの文字列コンテンツ | [!UICONTROL on-device decisioning] アーティファクトがダウンロードされるたびに呼び出されます。 |
| ArtifactDownloadFailed | 例外 | [!UICONTROL on-device decisioning] アーティファクトのダウンロードに失敗するたびに呼び出されます |

## 例

### \.NET

```dotnet {line-numbers="true"}
var clientConfig = new TargetClientConfig.Builder("acmeclient", "1234567890@AdobeOrg")
    .SetDecisioningMethod(DecisioningMethod.OnDevice)
    .SetOnDeviceDecisioningReady(DecisioningReady)
    .SetArtifactDownloadSucceeded(artifact => Console.WriteLine("The artifact was successfully downloaded. Contents: " + artifact))
    .SetArtifactDownloadFailed(exception => Console.WriteLine("The artifact failed to download. Exception: " + exception.Message))
    .Build();

var targetClient = TargetClient.Create(clientConfig);

// ...

static void DecisioningReady()
{
    var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

    var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
        .SetExecute(new ExecuteRequest(mboxes: mboxRequests))
        .Build();

    var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
}
```
