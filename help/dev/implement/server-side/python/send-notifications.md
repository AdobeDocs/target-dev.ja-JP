---
title: Python SDK を使用して、にディスプレイまたはク  [!DNL Adobe Target]  ック通知を送信する
description: sendNotifications （）を使用して、測定およびレポート用に表示またはクリック通知を送信する方法  [!DNL Adobe Target]  説明します。
feature: APIs/SDKs
exl-id: 03827b18-a546-4ec8-8762-391fcb3ac435
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 8%

---

# 通知の送信（Python）

## 説明

`send_notifications()` は、測定およびレポート用の [!DNL Adobe Target] に表示またはクリック通知を送信するために使用されます。

>[!NOTE]
>
>必須のパラメーターを持つ `execute` オブジェクトがリクエスト自体の中にある場合、インプレッションは、条件を満たすアクティビティについて自動的に増分されます。

インプレッションを自動的に増分する SDK メソッドは次のとおりです。

* `get_offers()`
* `get_attributes()`

リクエスト内で `prefetch` オブジェクトが渡された場合、`prefetch` オブジェクト内の mbox を持つアクティビティに対して、インプレッションが自動的に増分されることはありません。 インプレッション `Send_notifications()` コンバージョンを増分するための、事前読み込みされたエクスペリエンスに使用する必要があります。

## メソッド

### send_notifications

```python {line-numbers="true"}
target_client.send_notifications(options)
```

## パラメーター

`options` の構造は以下のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| request | DeliveryRequest | ○ | None | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエストに準拠しています |
| target_cookie | str | いいえ | None | [!DNL Target] cookie |
| target_location_hint | str | いいえ | None | [!DNL Target] location hint |
| consumer_id | str | いいえ | None | 複数の呼び出しをステッチする場合、異なる消費者 ID を指定する必要があります |
| customer_ids | list[CustomerId] | いいえ | None | VisitorId 互換の形式の顧客 ID のリスト |
| session_id | str | いいえ | None | 複数のリクエストのリンクに使用 |
| callback | 呼び出し | いいえ | None | リクエストを非同期で処理する場合、応答の準備が整うとコールバックが呼び出されます |
| err_callback | 呼び出し | いいえ | None | リクエストを非同期で処理する場合、例外が発生するとエラーコールバックが呼び出されます |

## Returns

同期的に呼び出される場合は `TargetDeliveryResponse` （デフォルト）、コールバックで呼び出される場合は `AsyncResult` を `Returns` します。 `TargetDeliveryResponse` の構造は以下のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| response | DeliveryResponse | [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) 応答に準拠しています |
| target_cookie | dict | [!DNL Target] cookie |
| target_location_hint_cookie | dict | [!DNL Target] location hint cookie |
| analytics_details | list[AnalyticsResponse] | [!DNL Analytics] ペイロード（クライアントサイド [!DNL Analytics] を使用する場合） |
| trace |  | list[dict] | すべてのリクエスト mbox/ビューの集計トレースデータ |
| response_tokens | list[dict] | [ 応答トークン&#x200B;のリスト ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=ja) |
| meta | dict | オンデバイス判定で使用する追加の判定メタデータ |

## 例

まず、`home` および `product1` mbox のコンテンツをプリフェッチするための [!UICONTROL Target Delivery API] リクエストを作成します。

### Python

```python {line-numbers="true"}
mboxes = [MboxRequest(name="home"),
          MboxRequest(name="product1")]
prefetch = PrefetchRequest(mboxes=mboxes)
delivery_request = DeliveryRequest(prefetch=prefetch)

# Next, we fetch the offers via Target Python SDK getOffers() API
response = target_client.get_offers({ "request": delivery_request })
```

正常な応答には、リクエストされた mbox のプリフェッチされたコンテンツを含む [!UICONTROL Target Delivery API] 応答オブジェクトが含まれます。 サンプルの `target_response["response"]` オブジェクト（dict 形式）は、次のように表示されます。

### Python

```python {line-numbers="true"}
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

各 Target コンテンツオプションには、mbox の `name` フィールドと `state` フィールド、および `eventToken` フィールドがあります。 これらは、各コンテンツオプションが表示されるとすぐに、`send_notifications()` リクエストで指定する必要があります。 `product1` mbox がブラウザー以外のデバイスに表示されているとします。 通知リクエストは次のように表示されます。

### Python

```python {line-numbers="true"}
notification_mbox = NotificationMbox(name="product1",
                                     state="J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U")
notification = Notification(
    id="1",
    type=MetricType.DISPLAY,
    timestamp=1621530726000,  # Epoch time in milliseconds
    mbox=notification_mbox,
    tokens=["t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="]
)
notification_request = DeliveryRequest(notifications=[notification])
```

プリフェッチ応答で配信された [!DNL Target] オファーに対応する mbox 状態とイベントトークンの両方が含まれていることに注意してください。 通知リクエストを作成したら、`send_notifications()` の API メソッドを使用して [!DNL Target] に送信できます。

### Python

```python {line-numbers="true"}
response = target_client.send_notifications({ "request": notification_request })
```
