---
title: 次の宛先にディスプレイまたはクリック通知を送信： [!DNL Adobe Target] Python SDK の使用
description: sendNotifications() を使用して、通知を送信またはクリックする方法を説明します。 [!DNL Adobe Target] 測定およびレポート用。
feature: APIs/SDKs
exl-id: 03827b18-a546-4ec8-8762-391fcb3ac435
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 9%

---

# 通知の送信 (Python)

## 説明

`send_notifications()` は、ディスプレイまたはクリック通知の送信先に使用されます。 [!DNL Adobe Target] 測定およびレポート用。

>[!NOTE]
>
>When in an `execute` 必要なパラメーターを含むオブジェクトがリクエスト自体内にある場合、インプレッションは、条件を満たすアクティビティに対して自動的に増分されます。

インプレッションを自動的に増分する SDK メソッドは次のとおりです。

* `get_offers()`
* `get_attributes()`

When a `prefetch` オブジェクトがリクエスト内で渡された場合、インプレッションは、 `prefetch` オブジェクト。 `Send_notifications()` インプレッション数およびコンバージョン数を増分するために、はプリフェッチされたエクスペリエンスに使用する必要があります。

## メソッド

### send_notifications

```python {line-numbers="true"}
target_client.send_notifications(options)
```

## パラメーター

`options` は次の構造を持ちます。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| request | DeliveryRequest | ○ | None | に準拠 [[!UICONTROL Target 配信 API]](/help/dev/implement/delivery-api/overview.md) リクエスト |
| target_cookie | str | いいえ | None | [!DNL Target] cookie |
| target_location_hint | str | いいえ | None | [!DNL Target] ロケーションヒント |
| consumer_id | str | いいえ | None | 複数の呼び出しを結び付ける場合は、異なる消費者 ID を提供する必要があります |
| customer_ids | リスト[CustomerId] | いいえ | None | VisitorId 互換形式の顧客 ID のリスト |
| session_id | str | いいえ | None | 複数のリクエストをリンクするために使用します |
| callback | 呼び出し可能な | いいえ | None | リクエストを非同期で処理する場合、応答の準備ができたときにコールバックが呼び出されます。 |
| err_callback | 呼び出し可能な | いいえ | None | リクエストを非同期で処理する場合、例外が発生したときにエラーコールバックが呼び出されます。 |

## Returns

`Returns` a `TargetDeliveryResponse` 同期的に（デフォルト）呼び出された場合、または `AsyncResult` コールバックで呼び出された場合。 `TargetDeliveryResponse` は次の構造を持ちます。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| 応答 | DeliveryResponse | に準拠 [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) 応答 |
| target_cookie | 判決 | [!DNL Target] cookie |
| target_location_hint_cookie | 判決 | [!DNL Target] ロケーションヒント Cookie |
| analytics_details | リスト[AnalyticsResponse] | [!DNL Analytics] ペイロード（クライアント側の場合） [!DNL Analytics] 使用状況 |
| trace |  | リスト[判決] | すべてのリクエスト mbox/ビューの集計トレースデータ |
| response_tokens | リスト[判決] | リスト [レスポ&#x200B;ンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) |
| meta | 判決 | オンデバイス判定で使用する追加の判定メタデータ |

## 例

まず、 [!UICONTROL Target 配信 API] のコンテンツをプリフェッチするためのリクエスト `home` および `product1` mbox.

### Python

```python {line-numbers="true"}
mboxes = [MboxRequest(name="home"),
          MboxRequest(name="product1")]
prefetch = PrefetchRequest(mboxes=mboxes)
delivery_request = DeliveryRequest(prefetch=prefetch)

# Next, we fetch the offers via Target Python SDK getOffers() API
response = target_client.get_offers({ "request": delivery_request })
```

成功した応答には、 [!UICONTROL Target 配信 API] リクエストされた mbox のプリフェッチされたコンテンツを含む応答オブジェクト。 サンプル `target_response["response"]` オブジェクト（dict 形式）は次のようになります。

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

mbox に注意 `name` および `state` フィールド、および `eventToken` 「 」フィールドに入力します。 これらは、 `send_notifications()` リクエストを送信します。 例えば、 `product1` mbox がブラウザー以外のデバイスに表示されている。 通知リクエストは次のように表示されます。

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

mbox の状態と、 [!DNL Target] プリフェッチ応答で配信されたオファー。 通知リクエストを作成したら、に送信できます。 [!DNL Target] 経由 `send_notifications()` API メソッド：

### Python

```python {line-numbers="true"}
response = target_client.send_notifications({ "request": notification_request })
```
