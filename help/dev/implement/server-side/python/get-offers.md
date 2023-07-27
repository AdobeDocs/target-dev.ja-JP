---
title: で getOffers() を使用します。 [!DNL Adobe Target] Python SDK を使用する場合
description: getOffers() を使用して決定を実行し、次の場所からエクスペリエンスを取得する方法を説明します。 [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 9539b806-e070-430e-80cf-cf632ce3f207
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '379'
ht-degree: 13%

---

# オファーの取得 (Python)

## 説明

`get_offers()` は、決定を実行し、次の場所からエクスペリエンスを取得するために使用されます。 [!DNL Adobe Target].


## メソッド

### getOffers

```python {line-numbers="true"}
target_client_instance.get_offers(options)
```

## パラメーター

The `options` dict の構造は次のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| request | DeliveryRequest | ○ | None | に準拠 [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエスト |
| target_cookie | str | いいえ | None | [!DNL Target] cookie |
| target_location_hint | str | いいえ | None | [!DNL Target] ロケーションヒント |
| consumer_id | str | いいえ | None | 複数の呼び出しを結び付ける場合は、異なる消費者 ID を提供する必要があります |
| customer_ids | リスト[CustomerId] | いいえ | None | VisitorId 互換形式の顧客 ID のリスト |
| session_id | str | いいえ | None | 複数のリクエストをリンクするために使用します |
| callback | 呼び出し可能な | いいえ | None | リクエストを非同期で処理する場合、応答の準備ができたときにコールバックが呼び出されます。 |
| err_callback | 呼び出し可能な | いいえ | None | リクエストを非同期で処理する場合、例外が発生したときにエラーコールバックが呼び出されます。 |

## Returns

を返します。 `TargetDeliveryResponse` 同期的に（デフォルト）呼び出された場合、または `AsyncResult` コールバックで呼び出された場合。 `TargetDeliveryResponse` は次の構造を持ちます。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| 応答 | DeliveryResponse | に準拠 [[!UICONTROL Target 配信 API]](/help/dev/implement/delivery-api/overview.md) 応答 |
| target_cookie | 判決 | [!DNL Target] cookie |
| target_location_hint_cookie | 判決 | [!DNL Target] ロケーションヒント Cookie |
| analytics_details | リスト[AnalyticsResponse] | Analytics ペイロード（クライアント側 Analytics を使用する場合） |
| trace | リスト[判決] | すべてのリクエスト mbox/ビューの集計トレースデータ |
| response_tokens | リスト[判決] | 次のリス&#x200B;ト：[レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) |
| meta | 判決 | オンデバイス判定で使用する追加の判定メタデータ |

`target_cookie` および `target_location_hint_cookie` データをブラウザーに渡すために使用されるオブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| name | str | cookie 名 |
| value | any | cookie の値の場合、値は文字列に変換されます |
| max_age | int | The `max_age option` は、現在の時刻（秒）を基準に有効期限が切れる便利な設定です。 |

The `meta` ターゲット応答のステータスを示すために使用されるオブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| decisioning_method | str | どの判定方法が使用されたか：オンデバイス、またはサーバーサイド |
| remote_mboxes | list`[str]` | 判定方法が `on-device`に値を指定する場合、完全に決定できなかった mbox 名の配列がデバイス上で提供されます。 つまり、 [[!UICONTROL Target 配信 API]](/help/dev/implement/delivery-api/overview.md) リクエストが必要です。 |
| remote_views | list`[str]` | 判定方法がオンデバイスの場合、完全に決定できなかったビュー名の配列がデバイス上に表示されます。 つまり、 [[!UICONTROL Target 配信 API]](/help/dev/implement/delivery-api/overview.md) リクエストが必要です。 |

## 例

### Python

```python {line-numbers="true"}
def client_ready_callback():
    context = Context(channel=ChannelType.WEB)
    mboxes = [MboxRequest(name="a1-serverside-ab", index=1)]
    execute = ExecuteRequest(mboxes=mboxes)
    delivery_request = DeliveryRequest(context=context, execute=execute)

    get_offers_options = {
      "request": delivery_request
    }

    target_delivery_response = target_client.get_offers(get_offers_options)


client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback
    }
}
target_client = TargetClient.create(client_options)
```
