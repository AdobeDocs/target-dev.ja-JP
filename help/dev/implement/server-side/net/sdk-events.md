---
title: '[!DNL Adobe Target] .NET SDKのイベントの購読'
description: '[!UICONTROL OnDeviceDecisioningHandler] オブジェクトを使用して、.NET SDK内で発生するさまざまなイベントを購読する方法を説明します。'
feature: APIs/SDKs
exl-id: 7578033f-3de5-4d13-9739-46ad1269ec5f
TQID: 'https://experienceleague.adobe.com/oeGknU-pW1-XjVrxn8JNEPoFBF8Gntt-vaVnqjdyTC8'
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
    internal-label: Target
feature_v2:
  - id: a19e8738-9679-599a-b83b-5f2f15f8e4d6
    internal-label: APIs/SDKs
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: 5d119ccf18b09b3ba864a69642458597f65c754f
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 5%
---
# SDK Events （.NET）

## 説明

SDK](initialize-sdk.md)を[初期化する際に、オプションの`OnDeviceDecisioningReady` デリゲートを`TargetClientConfig` オブジェクトで指定できます。これは、SDKがオンデバイス メソッド呼び出しの準備ができたときに呼び出されます。 [!UICONTROL  オンデバイス決定] アーティファクトのダウンロードを処理するために使用できる他のデリゲートもいくつかあります。

## Events

特定のイベントに対して、次のデリゲートを設定できます。

| 名前 | 引数 | 説明 |
| --- | --- | --- |
| OnDeviceDecisioningReady | None | クライアントが初めて[!UICONTROL  オンデバイス決定]の準備が整ったときに一度だけ呼び出されます |
| ArtifactDownloadSucceeded | アーティファクトファイルの文字列コンテンツ | [!UICONTROL  オンデバイス決定] アーティファクトがダウンロードされるたびに呼び出されます |
| ArtifactDownloadFailed | 例外 | [!UICONTROL  オンデバイス決定] アーティファクトのダウンロードに失敗するたびに呼び出されます |

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
