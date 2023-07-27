---
title: のイベントを購読する [!DNL Adobe Target] .NET SDK
description: .NET SDK 内で発生する様々なイベントを、 [!UICONTROL OnDeviceDecisioningHandler] オブジェクト。
feature: APIs/SDKs
exl-id: 7578033f-3de5-4d13-9739-46ad1269ec5f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '129'
ht-degree: 5%

---

# SDK イベント (.NET)

## 説明

条件 [SDK の初期化](initialize-sdk.md)、オプション `OnDeviceDecisioningReady` デリゲートは、 `TargetClientConfig` オブジェクト。SDK がオンデバイスメソッドの呼び出しの準備ができたときに呼び出されます。 他にも、 [!UICONTROL オンデバイス判定] アーティファクトのダウンロード。

## Events

特定のイベントに対して、次のデリゲートを設定できます。

| 名前 | 引数 | 説明 |
| --- | --- | --- |
| OnDeviceDecisioningReady | None | クライアントが次の準備ができた時点でのみ呼び出されます。 [!UICONTROL オンデバイス判定] |
| ArtifactDownloadSucceeded | 文字列アーティファクトファイルの内容 | 次の条件を満たすたびに呼び出されます。 [!UICONTROL オンデバイス判定] アーティファクトがダウンロードされました |
| ArtifactDownloadFailed | 例外 | ダウンロード失敗のたびに呼び出されます。 [!UICONTROL オンデバイス判定] アーティファクト |

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
