---
title: ' [!DNL Adobe Target] Python SDKでの非同期リクエストの使用方法'
description: ' [!DNL Target] Python SDKが非同期リクエストをサポートする方法を説明します。これにより、効果的な目標時間を0に短縮できます。'
feature: APIs/SDKs
exl-id: 44ab74e5-3c1a-49cf-9fff-fe523b0c2592
TQID: https://experienceleague.adobe.com/ZWRw2OlSbuEHorY0MXPOaBw3uePIW5dzpsuqho0Jtqk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 142
ht-degree: 4%

---

# 非同期リクエスト （Python）

## 説明

サーバーサイド統合の利点の1つは、並列処理を使用して、サーバーサイドで利用可能な巨大な帯域幅とコンピューティングリソースを活用できることです。[!DNL Target] Python SDKは非同期リクエストをサポートしており、これにより効果的な目標時間をゼロにすることができます。

## サポートされるメソッド

### Python

```python {line-numbers="true"}
get_offers(options)
send_notifications(options)
get_attributes(mbox_names, options)
```

## 例

Python 3.9以降で`asyncio` モジュールのasync/awaitを使用するサンプルアプリケーションは次のようになります。

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

この例では、Python 3.9以降を使用していることを前提としています。 古いバージョンのPythonを使用している場合でも、`options.callback`を`get_offers`に渡すことで非同期リクエストを送信できます。 コールバックまたは非同期/待機を使用した非同期実行の詳細については、サンプル Flask アプリを参照してください。こちら[こちら](https://github.com/adobe/target-python-sdk/blob/main/samples/app.py)。
