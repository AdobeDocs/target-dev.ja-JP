---
title: create メソッドを使用して Python SDK を初期化します。
description: create メソッドを使用して Python SDK を初期化し、 [!UICONTROL TargetClient] 電話をかける [!DNL Adobe Target] 実験とパーソナライズされたエクスペリエンスの場合。
feature: APIs/SDKs
exl-id: 3e231e8e-696d-45c7-b733-79bf99da5bec
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 19%

---

# Python SDK の初期化

説明： `create` メソッドを使用して、Python SDK を初期化し、 [!UICONTROL Target クライアント] 電話をかける [!DNL Adobe Target] 実験とパーソナライズされたエクスペリエンスの場合。

## メソッド

### 作成

```python {line-numbers="true"}
TargetClient.create(options)
```

## パラメーター

`options` は次の構造を持ちます。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| client | str | ○ | None | [!UICONTROL Adobe Target client ID] |
| organization_id | str | ○ | None | [!UICONTROL Experience Cloud組織 ID] |
| timeout | int | × | 3000 | タイムアウト（ミリ秒） |
| server_domain | str | × | `client.tt.omtrdc.net` |  | デフォルトのホスト名を上書き |
| secure | bool | × | true | HTTP スキームを強制するには設定を解除します |
| logger | object | × | INFO ロガー |  | デフォルトの INFO ロガーを置き換えます。 |
| target_location_hint | str | × | None | [!DNL Target] ロケーションヒント |
| property_token | str | × | None | [!DNL Target] プロパティトークン。 ここで指定した場合、すべての get_offers 呼び出しでこの値が使用されます。 |
| decisioning_method | str | × | サーバーサイド | 使用する判定方法を決定します ([オンデバイス](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、サーバー側、ハイブリッド ) |
| polling_interval | int | × | 300000（5 分） | のポーリング間隔 [オンデバイス判定ルールアーティファクト](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) （ミリ秒） |
| artifact_location | str | × | None | の完全修飾 URL [オンデバイス判定ルールアーティファクト](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). 内部的に決定された場所を上書きします。 |
| artifact_payload | object | × | None | の JSON ペイロード [オンデバイス判定ルールアーティファクト](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). 指定した場合、URL からリクエストする代わりに使用されます。 |
| [events](sdk-events.md) | 判決 &lt;str callable=&quot;&quot;> | × | None | イベント名キーとコールバック関数値を持つオプションのオブジェクトです。 |
| environment_id | int | × | 実稼動 | The [!DNL Target] 環境 ID |
| 環境 | str | × | 実稼動 | The [!DNL Target] 環境名 |
| cdn_environment | str | × | 実稼動 | CDN 環境名 |
| telemetry_enabled | bool | × | True | False に設定した場合、テレメトリデータは次の場所に送信されません： [!DNL Adobe] |
| version | str | × | None | この SDK のバージョン番号 |

## 例

### Python

```python {line-numbers="true"}
from target_python_sdk import TargetClient

def client_ready_callback(ready_event):
    # make calls to Adobe Target

client_options = {
    "client": "acmeclient",
    "organization_id": "1234567890@AdobeOrg",
    "events": {
        "client_ready": client_ready_callback
    }
}
target_client = TargetClient.create(client_options)
```
