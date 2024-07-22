---
title: リクエストをログに記録する  [!DNL Adobe Target] Python SDK の初期化
description: ' [!DNL Adobe Target] Python SDK でリクエストをログに記録する方法を説明します。'
feature: APIs/SDKs
exl-id: 0b3792a5-a9a7-4768-a429-598b49f1fd93
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 3%

---

# ロガー（Python）

## 説明

[SDK を初期化 ](initialize-sdk.md) する場合、`options["logger"]` オブジェクトはオプションのオブジェクトです。 デフォルトでは、INFO レベルのロガーが `adobe.target` 名前空間の下に作成されます。 ただし、問題が発生した場合にログレベルやデバッグを効果的にカスタマイズするために、SDK の初期化時に `logger` オブジェクトを指定できます。

`logger` オブジェクトには `debug()` および `error()` メソッドが必要です。 適切なロガーを指定すると、[!DNL Target] のリクエストと応答がログに記録されます。

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

リクエストと応答がコンソールに印刷されているのが確認できます。
