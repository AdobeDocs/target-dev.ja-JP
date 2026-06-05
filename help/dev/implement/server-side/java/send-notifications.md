---
title: Java SDKを使用してディスプレイまたはクリック通知を [!DNL Adobe Target] に送信する
description: sendNotifications （）を使用して、測定とレポート用にディスプレイ通知またはクリック通知を [!DNL Adobe Target] に送信する方法を説明します。
feature: APIs/SDKs
exl-id: 9231b480-f50f-40d1-ab06-0b9f2a2d79e3
TQID: https://experienceleague.adobe.com/aoa6x9BkuaC-6XaqU03mvWXqWucNSF9eJAk41Bk0-nE
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: c2be0313-b3ae-45e0-b454-d20bf54b23f2
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 232
ht-degree: 2%

---

# 通知の送信（Java）

## 説明

`sendNotifications()`は、測定とレポート用にディスプレイまたはクリック通知を[!DNL Adobe Target]に送信するために使用されます。

>[!NOTE]
>
>必須パラメーターを持つ`execute` オブジェクトがリクエスト自体の中にある場合、適格なアクティビティに対してインプレッションが自動的に増分されます。

インプレッションを自動的に増分するSDK メソッドは次のとおりです。

* `getOffers()`
* `getAttributes()`

`prefetch` オブジェクトがリクエスト内で渡されると、`prefetch` オブジェクト内のmboxを持つアクティビティに対して、インプレッションが自動的に増分されません。 インプレッションとコンバージョンを増やすには、プリフェッチされたエクスペリエンスに`sendNotifications()`を使用する必要があります。

## メソッド

### 作成

```javascript {line-numbers="true"}
ResponseStatus TargetClient.sendNotifications(TargetDeliveryRequest request)
```

## 例

まず、`home`と`product1`のmboxのコンテンツをプリフェッチするための[!DNL Target Delivery API] リクエストを作成します。

### 先行取得

```javascript {line-numbers="true"}
List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("home").index(1));
mboxRequests.add((MboxRequest) new MboxRequest().name("product1").index(2));
PrefetchRequest prefetchMboxesRequest = new PrefetchRequest().setMboxes(mboxRequests)

// Next, we fetch the offers via Target Java SDK getOffers() API
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```

正常な応答には、[!UICONTROL Target Delivery API]応答オブジェクトが含まれ、要求されたmboxのプリフェッチされたコンテンツが含まれます。 サンプル `targetResponse.response` オブジェクトは次のようになります。

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

[!DNL Target] コンテンツの各オプションで、mbox `name`および`state` フィールドと`eventToken` フィールドに注意してください。 これらは、各コンテンツオプションが表示されるとすぐに、`sendNotifications()` リクエストで提供する必要があります。 `product1` mboxがブラウザー以外のデバイスに表示されているとします。 通知リクエストは次のようになります。

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

プリフェッチ応答で配信された[!DNL Target] オファーに対応するmbox状態とイベントトークンの両方が含まれていることに注意してください。 通知リクエストを作成したら、`sendNotifications()` API メソッドを介して[!DNL Target]に送信できます。

### 応答

```javascript {line-numbers="true"}
ResponseStatus notificationResponse = targetJavaClient.sendNotifications(mboxNotificationRequest);
```
