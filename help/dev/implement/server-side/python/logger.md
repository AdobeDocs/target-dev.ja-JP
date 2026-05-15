---
title: リクエストをログに記録するための [!DNL Adobe Target] Python SDKの初期化
description: ' [!DNL Adobe Target] Python SDKでリクエストをログに記録する方法を説明します。'
feature: APIs/SDKs
exl-id: 0b3792a5-a9a7-4768-a429-598b49f1fd93
TQID: https://experienceleague.adobe.com/9LSln8V3QIG9GTok2yTTnKvhlpQhaed3a-qJyA4jErg
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: a94ced60-8199-4549-b453-ede2acb4101e
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 97
ht-degree: 3%

---

# ロガー（Python）

## 説明

SDK[&#128279;](initialize-sdk.md)を初期化する場合、`options["logger"]` オブジェクトはオプションのオブジェクトです。 既定では、INFO レベル ロガーは`adobe.target`名前空間の下に作成されます。 ただし、ログレベルをカスタマイズしたり、問題が発生したときに効果的にデバッグしたりするために、SDKの初期化時に`logger` オブジェクトを指定できます。

`logger` オブジェクトには`debug()`と`error()` メソッドが必要です。 適切なロガーが指定されると、[!DNL Target]件のリクエストと応答がログに記録されます。

## 例

### Python

```python {line-numbers="true"}
logger = logging.getLogger("org.logger")
logger.setLevel(logging.DEBUG)

client_options = {
  "client": "acmeclient",
  "organization_id": "1234567890@AdobeOrg",
  "logger": logger
}
target_client = TargetClient.create(client_options)
```

リクエストと応答がコンソールに出力されます。
