---
title: ' [!DNL Adobe Target] .NET SDK での非同期要求の使用方法'
description: Java SDK が  [!DNL Target]  非同期リクエストをサポートする方法を説明します。これにより、有効なターゲット時間をゼロに減らすことができます。
feature: APIs/SDKs
exl-id: fd36cc7b-a884-4e57-93c2-8aff8256109a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '91'
ht-degree: 4%

---

# 非同期要求（.NET）

## 説明

サーバーサイド統合の利点の 1 つは、並列処理を使用することで、サーバーサイドで利用できる膨大な帯域幅とコンピューティングリソースを活用できることです。 .NET SDK[!DNL Target]、非同期リクエストをサポートしており、アプリの既存の非同期ワークフローに [!DNL Target] れを簡単に統合できます。

## サポートされるメソッド

### \.NET

```dotnet {line-numbers="true"}
Task<TargetDeliveryResponse> GetOffersAsync(TargetDeliveryRequest request);
Task<TargetDeliveryResponse> SendNotificationsAsync(TargetDeliveryRequest request);
Task<TargetAttributes> GetAttributesAsync(TargetDeliveryRequest request, params string[] mboxes);
```

## 例

非同期 SDK API の使用例は、次のようになります。

### \.NET

```dotnet {line-numbers="true"}
var deliveryRequest = new TargetDeliveryRequest.Builder()
    .SetExecute(new ExecuteRequest(mboxes: new List<MboxRequest> { new MboxRequest(index: 1, name: "a1-serverside-ab") }))
    .Build();

var response = await this.targetClient.GetOffersAsync(deliveryRequest);

var notificationRequest = new TargetDeliveryRequest.Builder()
    .SetSessionId(response.Request.SessionId)
    .SetTntId(response.Response?.Id?.TntId)
    .SetNotifications(new List<Notification>
        {
            new (id: "1", type: MetricType.Display, timestamp: DateTimeOffset.UtcNow.ToUnixTimeMilliseconds(),
                mbox: new NotificationMbox("product1", "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"),
                tokens: new List<string> { "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==" })
        })
    .Build();

var notificationResponse = await this.targetClient.SendNotificationsAsync(notificationRequest);
```

この例では、[SDK を初期化した ](initialize-sdk.md) ことを前提としています。
