---
title: 次の宛先にディスプレイまたはクリック通知を送信： [!DNL Adobe Target] .NET SDK の使用
description: sendNotifications() を使用して、通知を送信またはクリックする方法を説明します。 [!DNL Adobe Target] 測定およびレポート用。
feature: APIs/SDKs
exl-id: 724e787c-af53-4152-8b20-136f7b5452e1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 1%

---

# 通知の送信 (.NET)

## 説明

`SendNotifications()` は、ディスプレイまたはクリック通知の送信先に使用されます。 [!DNL Adobe Target] 測定およびレポート用。

>[!NOTE]
>
>When in an `Execute` 必要なパラメーターを含むオブジェクトがリクエスト自体内にある場合、インプレッションは、条件を満たすアクティビティに対して自動的に増分されます。

インプレッションを自動的に増分する SDK メソッドは次のとおりです。

* `GetOffers()`
* `GetAttributes()`

When a `Prefetch` オブジェクトがリクエスト内で渡された場合、インプレッションは、 `Prefetch` オブジェクト。 `SendNotifications()` インプレッション数およびコンバージョン数を増分するために、はプリフェッチされたエクスペリエンスに使用する必要があります。

## メソッド

### 作成

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.SendNotifications(TargetDeliveryRequest request)
```

## 例

まず、 [!UICONTROL Target 配信 API] のコンテンツをプリフェッチするためのリクエスト `home` および `product1` mbox.

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

成功した応答には、 [!DNL Target Delivery API] リクエストされた mbox のプリフェッチされたコンテンツを含む応答オブジェクト。 サンプル `targetResponse.Response` オブジェクトは次のように表示されます。

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

次の点に注意してください。 `mbox` 名前と `state` フィールド、および `eventToken` フィールドの [!DNL Target] コンテンツオプション。 これらは、 `SendNotifications()` リクエストを送信します。 例えば、 `product1` mbox がブラウザー以外のデバイスに表示されている。 通知リクエストは次のように表示されます。

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

mbox の状態と、 [!DNL Target] プリフェッチ応答で配信されたオファー。 通知リクエストを作成したら、に送信できます。 [!DNL Target] 経由 `SendNotifications()` API メソッド：

### \.NET

```dotnet {line-numbers="true"}
var notificationResponse = targetClient.SendNotifications(mboxNotificationRequest);
```
