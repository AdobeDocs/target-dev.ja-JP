---
title: create メソッドを使用してPython SDKを初期化する
description: create メソッドを使用してPython SDKを初期化し、[!UICONTROL TargetClient]をインスタンス化して、実験とパーソナライズされたエクスペリエンスのために [!DNL Adobe Target] を呼び出す方法を説明します。
feature: APIs/SDKs
exl-id: 3e231e8e-696d-45c7-b733-79bf99da5bec
TQID: https://experienceleague.adobe.com/la4hiAeSKSTgV7-WPLuW-MudsVJAm3qbq1vT7rnzymQ
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 287
ht-degree: 16%

---

# Python SDKの初期化

説明
`create` メソッドを使用して、Python SDKを初期化し、[!UICONTROL Target Client]をインスタンス化して、実験およびパーソナライズされたエクスペリエンスのために[!DNL Adobe Target]を呼び出します。

## メソッド

### 作成

```python {line-numbers="true"}
TargetClient.create(options)
```

## パラメーター

`options`の構造は次のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| クライアント | str | ○ | None | [!UICONTROL Adobe Target クライアント ID] |
| organization_id | str | ○ | None | [!UICONTROL Experience Cloud組織ID] |
| timeout | int | × | 3000 | タイムアウト （ミリ秒単位） |
| server_domain | str | × | `client.tt.omtrdc.net` | デフォルトのホスト名を上書き |
| 安全 | ブール | × | true | HTTP スキームを適用する設定を解除 |
| ロガー | object | × | INFO ロガー | デフォルトのINFO ロガーを置き換えます |
| target_location_hint | str | × | None | [!DNL Target]場所のヒント |
| property_token | str | × | None | [!DNL Target] プロパティ トークン。 ここで指定した場合、すべてのget_offers呼び出しがこの値を使用します。 |
| decisioning_method | str | × | サーバーサイド | 使用する決定方法を決定します（[ オンデバイス ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、サーバーサイド、ハイブリッド） |
| polling_interval | int | × | 300000 （5分） | [ オンデバイス決定ルール アーティファクト ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)のポーリング間隔（ミリ秒） |
| artifact_location | str | × | None | [ オンデバイス決定ルール アーティファクト ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)への完全修飾URL。 内部で決定された場所を上書きします。 |
| artifact_payload | object | × | None | [ デバイス上の決定ルール アーティファクト ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)のJSON ペイロード。 指定した場合は、URLからリクエストする代わりに使用されます。 |
| [events](sdk-events.md) | dict &lt;str, callable> | × | None | イベント名キーとコールバック関数値を持つオプションのオブジェクト |
| environment_id | int | × | 本番 | [!DNL Target]環境ID |
| 環境 | str | × | 本番 | [!DNL Target]環境名 |
| cdn_environment | str | × | 本番 | CDN環境名 |
| telemetry_enabled | ブール | × | True | Falseに設定すると、テレメトリ データは[!DNL Adobe]に送信されません |
| version | str | × | None | このSDKのバージョン番号 |

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
