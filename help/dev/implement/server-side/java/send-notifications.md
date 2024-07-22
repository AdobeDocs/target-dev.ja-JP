---
title: Java SDK を使用した、への表示またはク  [!DNL Adobe Target]  ック通知の送信
description: sendNotifications （）を使用して、測定およびレポート用に表示またはクリック通知を送信する方法  [!DNL Adobe Target]  説明します。
feature: APIs/SDKs
exl-id: 9231b480-f50f-40d1-ab06-0b9f2a2d79e3
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 2%

---

# 通知の送信（Java）

## 説明

`sendNotifications()` は、測定およびレポート用の [!DNL Adobe Target] に表示またはクリック通知を送信するために使用されます。

>[!NOTE]
>
>必須のパラメーターを持つ `execute` オブジェクトがリクエスト自体の中にある場合、インプレッションは、条件を満たすアクティビティについて自動的に増分されます。

インプレッションを自動的に増分する SDK メソッドは次のとおりです。

* `getOffers()`
* `getAttributes()`

リクエスト内で `prefetch` オブジェクトが渡された場合、`prefetch` オブジェクト内の mbox を持つアクティビティに対して、インプレッションが自動的に増分されることはありません。 インプレッション `sendNotifications()` コンバージョンを増分するための、事前読み込みされたエクスペリエンスに使用する必要があります。

## メソッド

### 作成

```javascript {line-numbers="true"}
ResponseStatus TargetClient.sendNotifications(TargetDeliveryRequest request)
```

## 例

まず、`home` および `product1` mbox のコンテンツをプリフェッチするための [!DNL Target Delivery API] リクエストを作成します。

### プリフェッチ

```javascript {line-numbers="true"}
List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("home").index(1));
mboxRequests.add((MboxRequest) new MboxRequest().name("product1").index(2));
PrefetchRequest prefetchMboxesRequest = new PrefetchRequest().setMboxes(mboxRequests)

// Next, we fetch the offers via Target Java SDK getOffers() API
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```

正常な応答には、リクエストされた mbox のプリフェッチされたコンテンツを含む [!UICONTROL Target Delivery API] 応答オブジェクトが含まれます。 `targetResponse.response` オブジェクトの例は次のとおりです。

### 応答

```javascript {line-numbers="true"}
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

[!DNL Target] の各コンテンツオプションには、mbox の `name` フィールドと `state` フィールド、および `eventToken` フィールドがあります。 これらは、各コンテンツオプションが表示されるとすぐに、`sendNotifications()` リクエストで指定する必要があります。 `product1` mbox がブラウザー以外のデバイスに表示されているとします。 通知リクエストは次のようになります。

### リクエスト

```javascript {line-numbers="true"}
TargetDeliveryRequest mboxNotificationRequest = TargetDeliveryRequest.builder().notifications(new ArrayList() {{
    add(new Notification()
            .id("1")
            .timestamp(System.currentTimeMillis())
            .type(MetricType.DISPLAY)
            .mbox(new NotificationMbox()
                    .name("product1")
                    .state("J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U")
            )
            .tokens(Arrays.asList(new String[]{"t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="}))
    );
}}).build();
```

プリフェッチ応答で配信された [!DNL Target] オファーに対応する mbox 状態とイベントトークンの両方が含まれていることに注意してください。 通知リクエストを作成したら、API メソッドを使用して [!DNL Target] に送信でき `sendNotifications()` す。

### 応答

```javascript {line-numbers="true"}
ResponseStatus notificationResponse = targetJavaClient.sendNotifications(mboxNotificationRequest);
```
