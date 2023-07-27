---
title: で非同期リクエストを使用する方法 [!DNL Adobe Target] Python SDK
description: 方法を学ぶ [!DNL Target] Python SDK は非同期リクエストをサポートしているので、有効なターゲット時間をゼロに抑えることができます。
feature: APIs/SDKs
exl-id: 44ab74e5-3c1a-49cf-9fff-fe523b0c2592
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 4%

---

# 非同期リクエスト (Python)

## 説明

サーバー側の統合の利点の 1 つは、並列処理を使用することで、サーバー側で利用可能な大量の帯域幅とコンピューティングリソースを活用できる点です。 [!DNL Target] Python SDK は非同期リクエストをサポートしているので、有効なターゲット時間をゼロに抑えることができます。

## サポートされるメソッド

### Python

```python {line-numbers="true"}
get_offers(options)
send_notifications(options)
get_attributes(mbox_names, options)
```

## 例

を使用するサンプルアプリケーション `asyncio` Python 3.9 以降でのモジュールの async/await は次のようになります。

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

この例では、Python 3.9 以降を使用していることを前提としています。 古いバージョンの Python を使用している場合でも、を渡すことで、非同期リクエストを送信できます `options.callback` から `get_offers`. コールバックまたは async/await を使用した非同期実行について詳しくは、サンプルの Flask アプリを参照してください。 [ここ](https://github.com/adobe/target-python-sdk/blob/main/samples/app.py).
