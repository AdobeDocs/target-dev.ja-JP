---
title: .NET SDKを使用してディスプレイまたはクリック通知を [!DNL Adobe Target] に送信する
description: sendNotifications （）を使用して、測定とレポート用にディスプレイ通知またはクリック通知を [!DNL Adobe Target] に送信する方法を説明します。
feature: APIs/SDKs
exl-id: 724e787c-af53-4152-8b20-136f7b5452e1
TQID: https://experienceleague.adobe.com/4lJvfqWv6vDehZ-CmO7xj61-ZFS9-3nOAcA7vlbg-3c
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: c2be0313-b3ae-45e0-b454-d20bf54b23f2
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 230
ht-degree: 1%

---

# 送信通知（.NET）

## 説明

`SendNotifications()`は、測定とレポート用にディスプレイまたはクリック通知を[!DNL Adobe Target]に送信するために使用されます。

>[!NOTE]
>
>必須パラメーターを持つ`Execute` オブジェクトがリクエスト自体の中にある場合、適格なアクティビティに対してインプレッションが自動的に増分されます。

インプレッションを自動的に増分するSDK メソッドは次のとおりです。

* `GetOffers()`
* `GetAttributes()`

`Prefetch` オブジェクトがリクエスト内で渡されると、`Prefetch` オブジェクト内のmboxを持つアクティビティに対して、インプレッションが自動的に増分されません。 インプレッションとコンバージョンを増やすには、プリフェッチされたエクスペリエンスに`SendNotifications()`を使用する必要があります。

## メソッド

### 作成

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.SendNotifications(TargetDeliveryRequest request)
```

## 例

まず、`home`と`product1`のmboxのコンテンツをプリフェッチするための[!UICONTROL Target Delivery API] リクエストを作成します。

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

正常な応答には、要求されたmboxのプリフェッチされたコンテンツを含む[!DNL Target Delivery API]応答オブジェクトが含まれます。 サンプル `targetResponse.Response` オブジェクトは次のように表示されます。

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

[!DNL Target]の各コンテンツオプションに、`mbox`の名前と`state`のフィールド、および`eventToken`のフィールドを記録します。 これらは、各コンテンツオプションが表示されるとすぐに、`SendNotifications()` リクエストで提供する必要があります。 `product1` mboxがブラウザー以外のデバイスに表示されているとします。 通知リクエストは次のように表示されます。

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

プリフェッチ応答で配信された[!DNL Target] オファーに対応するmbox状態とイベントトークンの両方が含まれていることに注意してください。 通知リクエストを作成したら、`SendNotifications()` API メソッドを介して[!DNL Target]に送信できます。

### \.NET

```dotnet {line-numbers="true"}
var notificationResponse = targetClient.SendNotifications(mboxNotificationRequest);
```
