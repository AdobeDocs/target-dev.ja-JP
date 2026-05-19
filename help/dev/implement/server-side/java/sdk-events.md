---
title: ' [!DNL Adobe Target] Java SDKでのイベントの購読'
description: '[!UICONTROL OnDeviceDecisioningHandler] オブジェクトを使用してJava SDK内で発生するさまざまなイベントを購読する方法を説明します。'
feature: APIs/SDKs
exl-id: f2d56762-6bf7-4c6b-9c14-fb20e5cfd60d
TQID: https://experienceleague.adobe.com/x3aig-jM-GXzmLNcUNclZUK9Y49tuSF9-sdkxzJFtiM
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 134
ht-degree: 5%

---

# SDK Events （Java）

## 説明

SDK[&#128279;](initialize-sdk.md)を初期化中に、`ClientConfig` オブジェクトにオプションの`OnDeviceDecisioningHandler` オブジェクトを指定できます。 SDK内で発生するさまざまなイベントの購読に使用できます。 例えば、`onDeviceDecisioningReady` イベントは、SDKがメソッド呼び出しの準備ができたときに呼び出されるコールバック関数と共に使用できます。

## Events

`OnDeviceDecisioningHandler` オブジェクトには、特定のイベントに対して呼び出される次のコールバックが含まれています。

| 名前 | 引数 | 説明 |
| --- | --- | --- |
| onDeviceDecisioningReady | None | クライアントが初めて[!UICONTROL on-device decisioning]の準備が整ったときに一度だけ呼び出されます |
| artifactDownloadSucceeded | アーティファクト ファイルのバイト [] コンテンツ | [!UICONTROL on-device decisioning] アーティファクトがダウンロードされるたびに呼び出されます |
| artifactDownloadFailed | 例外 | [!UICONTROL on-device decisioning] アーティファクトのダウンロードに失敗するたびに呼び出されます |

## 例

### SDK Events

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .defaultDecisioningMethod(DecisioningMethod.ON_DEVICE)
        .onDeviceDecisioningHandler(new OnDeviceDecisioningHandler() {
            @Override
            public void onDeviceDecisioningReady() {
                // make getOffers requests
                makeTargetRequests();
            }

            @Override
            public void artifactDownloadSucceeded(byte[] artifactData) {
                System.out.println("The artifact was successfully downloaded.");
            }

            @Override
            public void artifactDownloadFailed(TargetClientException e) {
                System.out.println("The artifact failed to download.");
            }
        }).build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);


void makeTargetRequests() {
    List<MboxRequest> mboxRequests = new ArrayList<>();
    mboxRequests.add((MboxRequest) new MboxRequest().name("a1-serverside-ab").index(1));

    TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
            .context(new Context().channel(ChannelType.WEB))
            .execute(new ExecuteRequest().setMboxes(mboxRequests))
            .build();

    TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
}
```
