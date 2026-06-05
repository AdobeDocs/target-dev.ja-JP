---
title: ' [!DNL Adobe Target] Python SDKでのイベントの購読'
description: '[!UICONTROL OnDeviceDecisioningHandler] オブジェクトを使用して、Python SDK内で発生するさまざまなイベントを購読する方法を説明します。'
feature: APIs/SDKs
exl-id: 4e32e3b5-6072-4703-b09d-abb467aa1304
TQID: https://experienceleague.adobe.com/iFtlxw8Wlc9EMtDTndtXD7a2gu1TzGM6ijXire9gHPk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 166
ht-degree: 3%

---

# SDK Events （Python）

## 説明

[SDK](initialize-sdk.md)を初期化する場合、`options["events"]` ディクトは、イベント名キーとコールバック関数値を持つオプションのオブジェクトです。 SDK内で発生するさまざまなイベントの購読に使用できます。 例えば、`client_ready` イベントは、SDKがメソッド呼び出しの準備ができたときに呼び出されるコールバック関数と共に使用できます。

`callback`関数が呼び出されると、イベントオブジェクトが渡されます。 各イベントには、イベント名に対応する`type`があり、一部のイベントには、関連情報を含む追加のプロパティが含まれています。

## Events

| イベント名（タイプ） | 説明 | その他のイベントプロパティ |
| --- | --- | --- |
| client_ready | アーティファクトがダウンロードされ、SDKでget_offers呼び出しの準備が整ったときに発行されます。 オンデバイス判定メソッドを使用する場合に推奨されます。 | None |
| artifact_download_succeeded | 新しいアーティファクトがダウンロードされるたびに実行されます。 | artifact_payload, artifact_location |
| artifact_download_failed | アーティファクトのダウンロードに失敗するたびに実行されます。 | artifact_location, error |

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
