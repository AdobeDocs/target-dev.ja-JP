---
title: のイベントを購読する [!DNL Adobe Target] Java SDK
description: を使用して、Java SDK 内で発生する様々なイベントを購読する方法を説明します。 [!UICONTROL OnDeviceDecisioningHandler] オブジェクト。
feature: APIs/SDKs
exl-id: f2d56762-6bf7-4c6b-9c14-fb20e5cfd60d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '142'
ht-degree: 4%

---

# SDK イベント (Java)

## 説明

条件 [SDK の初期化](initialize-sdk.md)、オプション `OnDeviceDecisioningHandler` オブジェクトは、 `ClientConfig` オブジェクト。 SDK 内で発生する様々なイベントをサブスクライブする場合に使用できます。 例えば、 `onDeviceDecisioningReady` イベントは、SDK がメソッド呼び出しの準備ができたときに呼び出されるコールバック関数と共に使用することができます。

## Events

The `OnDeviceDecisioningHandler` オブジェクトには、次のコールバックが含まれます。これらは、特定のイベントに対して呼び出されます。

| 名前 | 引数 | 説明 |
| --- | --- | --- |
| onDeviceDecisioningReady | None | クライアントが次の準備ができた時点でのみ呼び出されます。 [!UICONTROL オンデバイス判定] |
| artifactDownloadSucceeded | バイト[] アーティファクトファイルの内容 | 呼び出しは、 [!UICONTROL オンデバイス判定] アーティファクトがダウンロードされました |
| artifactDownloadFailed | 例外 | ダウンロード失敗のたびに呼び出されます。 [!UICONTROL オンデバイス判定] アーティファクト |

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
