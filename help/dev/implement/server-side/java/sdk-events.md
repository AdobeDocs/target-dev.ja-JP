---
title: Java SDK でのイベント  [!DNL Adobe Target]  登録
description: '[!UICONTROL OnDeviceDecisioningHandler] オブジェクトを使用して Java SDK 内で発生する様々なイベントを登録する方法について説明します。'
feature: APIs/SDKs
exl-id: f2d56762-6bf7-4c6b-9c14-fb20e5cfd60d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '135'
ht-degree: 5%

---

# SDK イベント （Java）

## 説明

[SDK を初期化 ](initialize-sdk.md) する際に、`ClientConfig` オブジェクトにオプションの `OnDeviceDecisioningHandler` オブジェクトを指定できます。 SDK 内で発生する様々なイベントをサブスクライブするために使用できます。 例えば、`onDeviceDecisioningReady` イベントは、SDK がメソッド呼び出しの準備ができたときに呼び出されるコールバック関数と共に使用できます。

## Events

`OnDeviceDecisioningHandler` オブジェクトには、特定のイベントに対して呼び出される次のコールバックが含まれています。

| 名前 | 引数 | 説明 |
| --- | --- | --- |
| onDeviceDecisioningReady | None | クライアントの [!UICONTROL on-device decisioning] ークフロー準備が初めて完了したときにのみ呼び出されます。 |
| artifactDownloadSuccessful | byte[] アーティファクトファイルの内容 | [!UICONTROL on-device decisioning] アーティファクトがダウンロードされるたびに呼び出されます。 |
| artifactDownloadFailed | 例外 | [!UICONTROL on-device decisioning] アーティファクトのダウンロードに失敗するたびに呼び出されます |

## 例

### SDK イベント

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
