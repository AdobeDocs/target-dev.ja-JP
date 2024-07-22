---
title: ' [!DNL Adobe Target] Python SDK で非同期要求を使用する方法'
description: Python SDK が非同期リクエストをサポートする方法  [!DNL Target]  説明します。これにより、有効なターゲット時間をゼロに減らすことができます。
feature: APIs/SDKs
exl-id: fafb9e28-5ac5-41c1-8e7f-f40550b6749f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '123'
ht-degree: 16%

---

# 属性を取得（Python）

## 説明

`get_attributes()` を使用して、[!DNL Target] から実験とパーソナライズされたエクスペリエンスを取得し、属性値を抽出します。


## メソッド

### getAttributes

```python {line-numbers="true"}
target_client_instance.get_attributes(mbox_names, options)
```

## パラメーター

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| mbox_names | list[str] | ○ | None | mbox 名のリスト |
| options | dict | × | None | [ オファーを取得 ](get-offers.md) に使用するオプションと同じ |

## AttributesProvider

`target_client.get_attributes()` から返される `AttributesProvider` には、次のメソッドがあります。

| メソッド | 戻り値の型 | 説明 |
| --- | --- | --- |
| get_value （mbox_name, key） | any | 指定された mbox 名と属性キーの値を返します |
| as_object （mbox_name） | dict | キーと値のペアを持つ単純な json オブジェクトを返します |
| get_response （） | [TargetDeliveryResponse](https://github.com/adobe/target-python-sdk/blob/main/target_python_sdk/types/target_delivery_response.py) | 通常 `get_offers` によって返される応答オブジェクトを返します |

## 例

### Python

```python {line-numbers="true"}
def client_ready_callback():
    context = Context(channel=ChannelType.WEB)
    mboxes = [MboxRequest(name="a1-serverside-ab", index=1)]
    execute = ExecuteRequest(mboxes=mboxes)
    delivery_request = DeliveryRequest(context=context, execute=execute)

    get_attributes_options = {
      "request": delivery_request
    }

    attributes_provider = target_client.get_attributes(["demo-engineering-flags"], get_attributes_options)
    # returns just the value of searchProviderId from the demo-engineering-flags mbox offer
    search_provider_id = attributes_provider.get_value("demo-engineering-flags", "searchProviderId")

    # returns a simple dict object representing the demo-engineering-flags mbox offer
    engineering_flags = attributes_provider.as_object("demo-engineering-flags")
    """  the value of engineeringFlags looks like this
        {
            "cdnHostname": "cdn.cloud.corp.net",
            "searchProviderId": 143,
            "hasLegacyAccess": false
        }
    """

    asset_url = "http://{}/path/to/asset".format(engineering_flags.get("cdnHostname"))


client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback
    }
}
target_client = TargetClient.create(client_options)
```
