---
title: .NET SDK を使用して  [!DNL Adobe Target]  にディスプレイまたはクリック通知を送信
description: sendNotifications （）を使用して、測定およびレポート用に表示またはクリック通知を送信する方法  [!DNL Adobe Target]  説明します。
feature: APIs/SDKs
exl-id: 724e787c-af53-4152-8b20-136f7b5452e1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '231'
ht-degree: 1%

---

# 通知の送信（.NET）

## 説明

`SendNotifications()` は、測定およびレポート用の [!DNL Adobe Target] に表示またはクリック通知を送信するために使用されます。

>[!NOTE]
>
>必須のパラメーターを持つ `Execute` オブジェクトがリクエスト自体の中にある場合、インプレッションは、条件を満たすアクティビティについて自動的に増分されます。

インプレッションを自動的に増分する SDK メソッドは次のとおりです。

* `GetOffers()`
* `GetAttributes()`

リクエスト内で `Prefetch` オブジェクトが渡された場合、`Prefetch` オブジェクト内の mbox を持つアクティビティに対して、インプレッションが自動的に増分されることはありません。 インプレッション `SendNotifications()` コンバージョンを増分するための、事前読み込みされたエクスペリエンスに使用する必要があります。

## メソッド

### 作成

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.SendNotifications(TargetDeliveryRequest request)
```

## 例

まず、`home` および `product1` mbox のコンテンツをプリフェッチするための [!UICONTROL Target Delivery API] リクエストを作成します。

### \.NET

```dotnet {line-numbers="true"}
var mboxRequests = new List<MboxRequest>
    {
        new (index: 1, name: "home"),
        new (index: 2, name: "product1"),
    };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .SetPrefetch(new PrefetchRequest(mboxes: mboxRequests))
    .Build();

// Next, we fetch the offers via Target .NET SDK GetOffers() API
var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
```

正常な応答には、リクエストされた mbox のプリフェッチされたコンテンツを含む [!DNL Target Delivery API] 応答オブジェクトが含まれます。 `targetResponse.Response` オブジェクトの例は次のようになります。

### \.NET

```dotnet {line-numbers="true"}
{
  "status": 200,
  "requestId": "e8ac2dbf5f7d4a9f9280f6071f24a01e",
  "id": {
    "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "client": "adobetargetmobile",
  "edgeHost": "mboxedge21.tt.omtrdc.net",
  "prefetch": {
    "mboxes": [
      {
        "index": 0,
        "name": "home",
        "options": [
          {
            "type": "html",
            "content": "HOME OFFER",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      },
      {
        "index": 1,
        "name": "product1",
        "options": [
          {
            "type": "html",
            "content": "TEST OFFER 1",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      }
    ]
  }
}
```

[!DNL Target] の各コンテンツオプションには、`mbox` 名フィールドと `state` フィールド、および `eventToken` フィールドがあります。 これらは、各コンテンツオプションが表示されるとすぐに、`SendNotifications()` リクエストで指定する必要があります。 `product1` mbox がブラウザー以外のデバイスに表示されているとします。 通知リクエストは次のように表示されます。

### \.NET

```dotnet {line-numbers="true"}
var mboxNotifications = new List<Notification>
{
    new (id: "1", type: MetricType.Display, timestamp: DateTimeOffset.UtcNow.ToUnixTimeMilliseconds(),
        mbox: new NotificationMbox("product1", "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"),
        tokens: new List<string> { "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==" })
}; 

var mboxNotificationRequest = new TargetDeliveryRequest.Builder()
    .SetNotifications(mboxNotifications)
    .Build();
```

プリフェッチ応答で配信された [!DNL Target] オファーに対応する mbox 状態とイベントトークンの両方が含まれていることに注意してください。 通知リクエストを作成したら、`SendNotifications()` の API メソッドを使用して [!DNL Target] に送信できます。

### \.NET

```dotnet {line-numbers="true"}
var notificationResponse = targetClient.SendNotifications(mboxNotificationRequest);
```
