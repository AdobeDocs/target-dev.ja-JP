---
title: のイベントを購読する [!DNL Adobe Target] Python SDK
description: Python SDK 内で発生する様々なイベントを、 [!UICONTROL OnDeviceDecisioningHandler] オブジェクト。
feature: APIs/SDKs
exl-id: 4e32e3b5-6072-4703-b09d-abb467aa1304
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 3%

---

# SDK イベント (Python)

## 説明

条件 [SDK の初期化](initialize-sdk.md)、 `options["events"]` dict は、イベント名キーとコールバック関数値を持つオプションのオブジェクトです。 SDK 内で発生する様々なイベントをサブスクライブする場合に使用できます。 例えば、 `client_ready` イベントは、SDK がメソッド呼び出しの準備ができたときに呼び出されるコールバック関数と共に使用することができます。

次の場合に `callback` 関数が呼び出されると、イベントオブジェクトが渡されます。 各イベントには `type` イベント名に対応し、一部のイベントには、関連情報を含む追加のプロパティが含まれます。

## Events

| イベント名（タイプ） | 説明 | 追加のイベントプロパティ |
| --- | --- | --- |
| client_ready | アーティファクトがダウンロードされ、SDK が get_offers 呼び出しの準備ができたときに発生します。 を使用する際に推奨 | オンデバイス判定方法。 | None |
| artifact_download_succeeded | 新しいアーティファクトがダウンロードされるたびに発生します。 | artifact_payload, artifact_location |
| artifact_download_failed | アーティファクトのダウンロードが失敗するたびに発生します。 | artifact_location, error |

## 例

### Python

```python {line-numbers="true"}
def client_ready_callback():
    # make get_offers requests

def artifact_download_succeeded(event):
    print("The artifact was successfully downloaded from {}".format(event.artifact_location))
    # optionally do something with event.artifact_payload, like persist it

def artifact_download_failed(event):
    print("The artifact failed to download from {} with the following error: {}"
          .format(event.artifact_location, str(event.error)))

client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback,
        "artifact_download_succeeded": artifact_download_succeeded,
        "artifact_download_failed": artifact_download_failed
    }
}
target_client = target_client.create(client_options)
```
