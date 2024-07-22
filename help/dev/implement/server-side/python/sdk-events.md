---
title: Python SDK でのイベント  [!DNL Adobe Target]  登録
description: '[!UICONTROL OnDeviceDecisioningHandler] オブジェクトを使用して Python SDK 内で発生する様々なイベントを登録する方法を説明します。'
feature: APIs/SDKs
exl-id: 4e32e3b5-6072-4703-b09d-abb467aa1304
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '165'
ht-degree: 3%

---

# SDK イベント （Python）

## 説明

[SDK を初期化 ](initialize-sdk.md) する際、`options["events"]` dict は、イベント名キーとコールバック関数値を持つオプションのオブジェクトです。 SDK 内で発生する様々なイベントをサブスクライブするために使用できます。 例えば、`client_ready` イベントは、SDK がメソッド呼び出しの準備ができたときに呼び出されるコールバック関数と共に使用できます。

`callback` 関数が呼び出されると、イベントオブジェクトが渡されます。 各イベントにはイベント名に対応する `type` があり、一部のイベントには関連情報を含む追加のプロパティが含まれています。

## Events

| イベント名（タイプ） | 説明 | その他のイベントプロパティ |
| --- | --- | --- |
| client_ready | アーティファクトがダウンロードされ、SDK が get_offers 呼び出しの準備が整うと発行されます。 を使用する場合に推奨 | オンデバイス判定方法。 | None |
| artifact_download_successful | 新しいアーティファクトがダウンロードされるたびに発行されます。 | artifact_payload, artifact_location |
| artifact_download_failed | アーティファクトのダウンロードに失敗するたびに生成されます。 | artifact_location, エラー |

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
