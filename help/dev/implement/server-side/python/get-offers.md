---
title: Python SDKを使用する場合は、 [!DNL Adobe Target] でgetOffers （）を使用します
description: getOffers （）を使用して決定を実行し、 [!DNL Adobe Target]からエクスペリエンスを取得する方法を説明します。
feature: APIs/SDKs
exl-id: 9539b806-e070-430e-80cf-cf632ce3f207
TQID: https://experienceleague.adobe.com/b7t1NfE5Gcsj86w4u3Cfl5-Eb7a6HG1Hg8vi6-ViQFg
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 357
ht-degree: 12%

---

# オファーを取得（Python）

## 説明

`get_offers()`は、決定を実行し、[!DNL Adobe Target]からエクスペリエンスを取得するために使用されます。


## メソッド

### getOffers

```python {line-numbers="true"}
target_client_instance.get_offers(options)
```

## パラメーター

`options` ディクトの構造は次のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| request | DeliveryRequest | ○ | None | [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエストに準拠 |
| target_cookie | str | いいえ | None | [!DNL Target] Cookie |
| target_location_hint | str | いいえ | None | [!DNL Target]場所のヒント |
| consumer_id | str | いいえ | None | 複数の呼び出しをステッチする場合は、異なる消費者IDを指定する必要があります |
| customer_ids | リスト [顧客ID] | いいえ | None | VisitorIdと互換性のある形式の顧客IDのリスト |
| session_id | str | いいえ | None | 複数のリクエストをリンクするために使用 |
| callback | コール可能 | いいえ | None | リクエストを非同期的に処理する場合、応答の準備ができたときにコールバックが呼び出されます |
| err_callback | コール可能 | いいえ | None | リクエストを非同期的に処理する場合、例外が発生したときにエラーコールバックが呼び出されます |

## 返品

同期で呼び出された場合は`TargetDeliveryResponse`、コールバックで呼び出された場合は`AsyncResult`を返します。 `TargetDeliveryResponse`の構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| 応答 | DeliveryResponse | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)の応答に準拠しています |
| target_cookie | dict | [!DNL Target] Cookie |
| target_location_hint_cookie | dict | [!DNL Target]の場所ヒント Cookie |
| analytics_details | list[AnalyticsResponse] | Analytics ペイロード（クライアントサイドのAnalyticsの使用の場合） |
| trace | リスト [辞書] | すべてのリクエスト mbox/ビューのトレース データを集約しました |
| response_tokens | リスト [辞書] | &#x200B;[応答トークン &#x200B;](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)のリスト |
| meta | dict | オンデバイス判定で使用する追加の判定メタデータ |

ブラウザーにデータを渡すために使用される`target_cookie`および`target_location_hint_cookie` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| name | str | cookie 名 |
| value | any | Cookie値の場合、値は文字列に変換されます |
| max_age | int | `max_age option`は、現在の時間に対して秒単位で有効期限が切れる設定に便利です |

ターゲット応答のステータスを示すために使用される`meta` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| decisioning_method | str | 使用された決定方法：オンデバイスまたはサーバーサイド |
| remote_mboxes | リスト `[str]` | 決定方法が`on-device`の場合、オンデバイスで完全に決定できなかったmbox名の配列が与えられます。 つまり、[[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエストが必要です。 |
| remote_views | リスト `[str]` | 決定方法がオンデバイスの場合、オンデバイスで完全に決定できなかったビュー名の配列が与えられます。 つまり、[[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエストが必要です。 |

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
