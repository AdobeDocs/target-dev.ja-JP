---
title: Node.js SDK を使用して  [!DNL Adobe Target]  表示またはクリック通知をに送信します。
description: sendNotifications （）を使用して、測定およびレポート用に表示またはクリック通知を送信する方法  [!DNL Adobe Target]  説明します。
feature: APIs/SDKs
exl-id: 84bb6a28-423c-457f-8772-8e3f70e06a6c
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 4%

---

# 通知の送信（Node.js）

## 説明

`[!UICONTROL sendNotifications()]` は、測定およびレポート用の [!DNL Adobe Target] に表示またはクリック通知を送信するために使用されます。

>[!NOTE]
>
>必須のパラメーターを持つ `execute` オブジェクトがリクエスト自体の中にある場合、インプレッションは、条件を満たすアクティビティについて自動的に増分されます。

インプレッションを自動的に増分する SDK メソッドは次のとおりです。

* `getOffers()`
* `getAttributes()`

リクエスト内で `prefetch` オブジェクトが渡された場合、`prefetch` オブジェクト内の mbox を持つアクティビティに対して、インプレッションが自動的に増分されることはありません。 インプレッション `sendNotifications()` コンバージョンを増分するための、事前読み込みされたエクスペリエンスに使用する必要があります。

## メソッド

### 作成

```js {line-numbers="true"}
TargetClient.sendNotifications(options: Object): Promise
```

### パラメーター

`options` の構造は以下のとおりです。

| 名前 | タイプ | 必須 | デフォルト |
| --- | --- | --- | --- |
| request | オブジェクト | ○ | None |

## 例

まず、`home` および `product1` mbox のコンテンツをプリフェッチするための Target Delivery API リクエストを作成します。

### Node.js

```js {line-numbers="true"}
const prefetchMboxesRequest = {
  prefetch: {
    mboxes: [
      { name: "home" },
      { name: "product1" }
    ]
  }
};
// Next, we fetch the offers via Target Node.js SDK getOffers() API
const targetResponse = await targetClient.getOffers({ request: prefetchMboxesRequest });
```

正常な応答には、リクエストされた mbox のプリフェッチされたコンテンツを含む [!UICONTROL Target Delivery API] 応答オブジェクトが含まれます。 `targetResponse.response` オブジェクトの例は次のようになります。

### Node.js

```js {line-numbers="true"}
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

[!DNL Target] の各コンテンツオプションには、mbox の `name` フィールドと `state` フィールド、および `eventToken` フィールドがあります。 これらは、各コンテンツオプションが表示されるとすぐに、`sendNotifications()` リクエストで指定する必要があります。 `product1` mbox がブラウザー以外のデバイスに表示されているとします。 通知リクエストは次のように表示されます。

### Node.js

```js {line-numbers="true"}
const mboxNotificationRequest = {
  notifications: [{
    id: "1",
    timestamp: Date.now(),
    type: "display",
    mbox: {
      name: "product1",
      state: "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
    },
    tokens: [ "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==" ]
  }]
};
```

プリフェッチ応答で配信された [!DNL Target] オファーに対応する mbox 状態とイベントトークンの両方が含まれていることに注意してください。 通知リクエストを作成したら、`sendNotifications()` の API メソッドを使用して [!DNL Target] に送信できます。

### Node.js

```js {line-numbers="true"}
const notificationResponse = await targetClient.sendNotifications({ request: mboxNotificationRequest });
```
