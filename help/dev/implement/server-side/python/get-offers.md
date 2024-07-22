---
title: Python SDK を使用する場合は、getOffers （）  [!DNL Adobe Target]  使用してください。
description: getOffers （）を使用して決定を実行し、 [!DNL Adobe Target] からエクスペリエンスを取得する方法を説明します。
feature: APIs/SDKs
exl-id: 9539b806-e070-430e-80cf-cf632ce3f207
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '348'
ht-degree: 12%

---

# オファーを取得（Python）

## 説明

`get_offers()` を使用して、決定を実行し、[!DNL Adobe Target] からエクスペリエンスを取得します。


## メソッド

### getOffers

```python {line-numbers="true"}
target_client_instance.get_offers(options)
```

## パラメーター

`options` dict の構造は次のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| request | DeliveryRequest | ○ | None | [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエストに準拠しています |
| target_cookie | str | いいえ | None | [!DNL Target] cookie |
| target_location_hint | str | いいえ | None | [!DNL Target] location hint |
| consumer_id | str | いいえ | None | 複数の呼び出しをステッチする場合、異なる消費者 ID を指定する必要があります |
| customer_ids | list[CustomerId] | いいえ | None | VisitorId 互換の形式の顧客 ID のリスト |
| session_id | str | いいえ | None | 複数のリクエストのリンクに使用 |
| callback | 呼び出し | いいえ | None | リクエストを非同期で処理する場合、応答の準備が整うとコールバックが呼び出されます |
| err_callback | 呼び出し | いいえ | None | リクエストを非同期で処理する場合、例外が発生するとエラーコールバックが呼び出されます |

## Returns

同期的に呼び出される場合は `TargetDeliveryResponse` を返し（デフォルト）、コールバックで呼び出される場合は `AsyncResult` を返します。 `TargetDeliveryResponse` の構造は以下のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| response | DeliveryResponse | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) 応答に準拠しています |
| target_cookie | dict | [!DNL Target] cookie |
| target_location_hint_cookie | dict | [!DNL Target] location hint cookie |
| analytics_details | list[AnalyticsResponse] | Analytics ペイロード（クライアントサイド Analytics を使用する場合） |
| trace | list[dict] | すべてのリクエスト mbox/ビューの集計トレースデータ |
| response_tokens | list[dict] | &#x200B;応答トークン [ リスト ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) |
| meta | dict | オンデバイス判定で使用する追加の判定メタデータ |

データをブラウザーに戻すために使用される `target_cookie` および `target_location_hint_cookie` オブジェクトは、次の構造を持っています。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| name | str | cookie 名 |
| value | any | Cookie の値。値は文字列に変換されます |
| max_age | int | `max_age option` は、現在の時刻（秒）を基準に有効期限を設定する場合に便利です |

ターゲット応答のステータスを示すために使用される `meta` オブジェクトは、次の構造を持ちます。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| decisioning_method | str | 使用された判定方法：オンデバイスまたはサーバーサイド |
| remote_mbox | リスト `[str]` | Decisioning メソッドが `on-device` の場合、オンデバイスで完全に決定できなかった mbox 名の配列が提供されます。 つまり、[[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエストが必要です。 |
| remote_views | リスト `[str]` | 判定方法がオンデバイスの場合、オンデバイスで完全に決定できなかったビュー名の配列が与えられます。 つまり、[[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエストが必要です。 |

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
