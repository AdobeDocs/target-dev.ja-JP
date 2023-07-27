---
title: 次の宛先にディスプレイまたはクリック通知を送信： [!DNL Adobe Target] Java SDK の使用
description: sendNotifications() を使用して、通知を送信またはクリックする方法を説明します。 [!DNL Adobe Target] 測定およびレポート用。
feature: APIs/SDKs
exl-id: 9231b480-f50f-40d1-ab06-0b9f2a2d79e3
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 2%

---

# 通知の送信 (Java)

## 説明

`sendNotifications()` は、ディスプレイまたはクリック通知の送信先に使用されます。 [!DNL Adobe Target] 測定およびレポート用。

>[!NOTE]
>
>When in an `execute` 必要なパラメーターを含むオブジェクトがリクエスト自体内にある場合、インプレッションは、条件を満たすアクティビティに対して自動的に増分されます。

インプレッションを自動的に増分する SDK メソッドは次のとおりです。

* `getOffers()`
* `getAttributes()`

When a `prefetch` オブジェクトがリクエスト内で渡された場合、インプレッションは、 `prefetch` オブジェクト。 `sendNotifications()` インプレッション数およびコンバージョン数を増分するために、はプリフェッチされたエクスペリエンスに使用する必要があります。

## メソッド

### 作成

```javascript {line-numbers="true"}
ResponseStatus TargetClient.sendNotifications(TargetDeliveryRequest request)
```

## 例

まず、 [!DNL Target Delivery API] のコンテンツをプリフェッチするためのリクエスト `home` および `product1` mbox.

### プリフェッチ

```javascript {line-numbers="true"}
List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("home").index(1));
mboxRequests.add((MboxRequest) new MboxRequest().name("product1").index(2));
PrefetchRequest prefetchMboxesRequest = new PrefetchRequest().setMboxes(mboxRequests)

// Next, we fetch the offers via Target Java SDK getOffers() API
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```

成功した応答には、 [!UICONTROL Target 配信 API] リクエストされた mbox のプリフェッチされたコンテンツを含む応答オブジェクト。 サンプル `targetResponse.response` オブジェクトは次のようになります。

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

mbox に注意 `name` および `state` フィールド、および `eventToken` フィールドの [!DNL Target] コンテンツオプション。 これらは、 `sendNotifications()` リクエストを送信します。 例えば、 `product1` mbox がブラウザー以外のデバイスに表示されている。 通知リクエストは次のようになります。

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

mbox の状態と、 [!DNL Target] プリフェッチ応答で配信されたオファー。 通知リクエストを作成したら、に送信できます。 [!DNL Target] 経由 `sendNotifications()` API メソッド：

### 応答

```javascript {line-numbers="true"}
ResponseStatus notificationResponse = targetJavaClient.sendNotifications(mboxNotificationRequest);
```
