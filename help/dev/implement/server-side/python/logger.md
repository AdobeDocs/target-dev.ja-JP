---
title: を初期化します。 [!DNL Adobe Target] リクエストをログに記録する Python SDK
description: リクエストを [!DNL Adobe Target] Python SDK。
feature: APIs/SDKs
exl-id: 0b3792a5-a9a7-4768-a429-598b49f1fd93
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '99'
ht-degree: 3%

---

# ロガー (Python)

## 説明

条件 [SDK の初期化](initialize-sdk.md)、 `options["logger"]` オブジェクトはオプションのオブジェクトです。 デフォルトでは、INFO レベルのロガーが `adobe.target` 名前空間。 ただし、問題が発生した場合にログレベルやデバッグを効果的にカスタマイズするには、 `logger` オブジェクトは、SDK の初期化時に指定できます。

The `logger` オブジェクトには `debug()` および `error()` メソッド。 適切なロガーが提供された場合、 [!DNL Target] リクエストと応答がログに記録されます。

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

コンソールに要求と応答が印刷されています。
