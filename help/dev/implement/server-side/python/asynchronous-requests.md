---
title: ' [!DNL Adobe Target] Python SDK で非同期要求を使用する方法'
description: Python SDK が非同期リクエストをサポートする方法  [!DNL Target]  説明します。これにより、有効なターゲット時間をゼロに減らすことができます。
feature: APIs/SDKs
exl-id: 44ab74e5-3c1a-49cf-9fff-fe523b0c2592
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '133'
ht-degree: 4%

---

# 非同期要求（Python）

## 説明

サーバーサイド統合の利点の 1 つは、並列処理を使用することで、サーバーサイドで利用できる膨大な帯域幅とコンピューティングリソースを活用できる点です。 Python SDK[!DNL Target]、非同期要求をサポートしているので、有効なターゲット時間をゼロに減らすことができます。

## サポートされるメソッド

### Python

```python {line-numbers="true"}
get_offers(options)
send_notifications(options)
get_attributes(mbox_names, options)
```

## 例

Python 3.9+で `asyncio` モジュールの async/await を使用するサンプルアプリケーションは、次のようになります。

### Python

```python {line-numbers="true"}
async def execute_mboxes(self, mboxes):
    context = Context(channel=ChannelType.WEB)
    execute = ExecuteRequest(mboxes=mboxes)
    delivery_request = DeliveryRequest(context=context, execute=execute)

    get_offers_options = {
      "request": delivery_request
    }
    return await asyncio.to_thread(target_client.get_offers, get_offers_options)

async def get_target_delivery_response(mboxes):
    target_delivery_response = await execute_mboxes(mboxes)
    response = Response(target_delivery_response.get("response").to_str(), status=200, mimetype='application/json')
    return response

mboxes = [MboxRequest(name="a1-serverside-ab", index=1)]
return asyncio.run(get_target_delivery_response(mboxes)
```

この例では、Python 3.9 以降を使用していることを前提としています。 古いバージョンの Python を使用している場合でも、`get_offers` に `options.callback` を渡すことで非同期リクエストを送信できます。 コールバックまたは async/await を使用した非同期実行について詳しくは、サンプル Flask アプリを参照してください [ こちら ](https://github.com/adobe/target-python-sdk/blob/main/samples/app.py)。
