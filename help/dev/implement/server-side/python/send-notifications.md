---
title: Python SDKを使用してディスプレイまたはクリック通知を [!DNL Adobe Target] に送信する
description: sendNotifications （）を使用して、測定とレポート用にディスプレイ通知またはクリック通知を [!DNL Adobe Target] に送信する方法を説明します。
feature: APIs/SDKs
exl-id: 03827b18-a546-4ec8-8762-391fcb3ac435
TQID: https://experienceleague.adobe.com/r7j2MaCmcZBEsx7TmTlKL9R-IKlncZJw5DhSfcKmVNU
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: c2be0313-b3ae-45e0-b454-d20bf54b23f2
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 420
ht-degree: 8%

---

# 通知の送信（Python）

## 説明

`send_notifications()`は、測定とレポート用にディスプレイまたはクリック通知を[!DNL Adobe Target]に送信するために使用されます。

>[!NOTE]
>
>必須パラメーターを持つ`execute` オブジェクトがリクエスト自体の中にある場合、適格なアクティビティに対してインプレッションが自動的に増分されます。

インプレッションを自動的に増分するSDK メソッドは次のとおりです。

* `get_offers()`
* `get_attributes()`

`prefetch` オブジェクトがリクエスト内で渡されると、`prefetch` オブジェクト内のmboxを持つアクティビティに対して、インプレッションが自動的に増分されません。 インプレッションとコンバージョンを増やすには、プリフェッチされたエクスペリエンスに`Send_notifications()`を使用する必要があります。

## メソッド

### send_notifications

```python {line-numbers="true"}
target_client.send_notifications(options)
```

## パラメーター

`options`の構造は次のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| request | DeliveryRequest | ○ | None | [[!UICONTROL Target配信API]](/help/dev/implement/delivery-api/overview.md)要求に準拠 |
| target_cookie | str | いいえ | None | [!DNL Target] Cookie |
| target_location_hint | str | いいえ | None | [!DNL Target]場所のヒント |
| consumer_id | str | いいえ | None | 複数の呼び出しをステッチする場合は、異なる消費者IDを指定する必要があります |
| customer_ids | リスト [顧客ID] | いいえ | None | VisitorIdと互換性のある形式の顧客IDのリスト |
| session_id | str | いいえ | None | 複数のリクエストをリンクするために使用 |
| callback | コール可能 | いいえ | None | リクエストを非同期的に処理する場合、応答の準備ができたときにコールバックが呼び出されます |
| err_callback | コール可能 | いいえ | None | リクエストを非同期的に処理する場合、例外が発生したときにエラーコールバックが呼び出されます |

## 返品

`Returns`は、同期で呼び出された場合は`TargetDeliveryResponse`、コールバックで呼び出された場合は`AsyncResult`です。 `TargetDeliveryResponse`の構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| 応答 | DeliveryResponse | [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)の応答に準拠しています |
| target_cookie | dict | [!DNL Target] Cookie |
| target_location_hint_cookie | dict | [!DNL Target]の場所ヒント Cookie |
| analytics_details | list[AnalyticsResponse] | [!DNL Analytics] ペイロード （クライアント側[!DNL Analytics]使用時） |
| trace | リスト [辞書] | すべてのリクエスト mbox/ビューのトレース データを集約しました |
| response_tokens | リスト [辞書] | [応答トークン&#x200B;リスト &#x200B;](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=ja) |
| meta | dict | オンデバイス判定で使用する追加の判定メタデータ |

## 例

まず、`home`と`product1`のmboxのコンテンツをプリフェッチするための[!UICONTROL Target Delivery API] リクエストを作成します。

### Python

```python {line-numbers="true"}
mboxes = [MboxRequest(name="home"),
          MboxRequest(name="product1")]
prefetch = PrefetchRequest(mboxes=mboxes)
delivery_request = DeliveryRequest(prefetch=prefetch)

# Next, we fetch the offers via Target Python SDK getOffers() API
response = target_client.get_offers({ "request": delivery_request })
```

正常な応答には、[!UICONTROL Target Delivery API]応答オブジェクトが含まれ、要求されたmboxのプリフェッチされたコンテンツが含まれます。 サンプル `target_response["response"]` オブジェクト （文書形式）は、次のように表示されます。

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

各Target コンテンツオプションのmbox `name` フィールドと`state` フィールド、および`eventToken` フィールドに注意してください。 これらは、各コンテンツオプションが表示されるとすぐに、`send_notifications()` リクエストで提供する必要があります。 `product1` mboxがブラウザー以外のデバイスに表示されているとします。 通知リクエストは次のように表示されます。

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

プリフェッチ応答で配信された[!DNL Target] オファーに対応するmbox状態とイベントトークンの両方が含まれていることに注意してください。 通知リクエストを作成したら、`send_notifications()` API メソッドを介して[!DNL Target]に送信できます。

### Python

```python {line-numbers="true"}
response = target_client.send_notifications({ "request": notification_request })
```
