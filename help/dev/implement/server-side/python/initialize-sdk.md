---
title: create メソッドを使用して Python SDKを初期化します
description: create メソッドを使用して Python SDKを初期化し、[!UICONTROL TargetClient] ールをインスタンス化して、実験やパーソナライズされたエクスペリエンスを呼び出す方法に  [!DNL Adobe Target]  いて説明します。
feature: APIs/SDKs
exl-id: 3e231e8e-696d-45c7-b733-79bf99da5bec
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '271'
ht-degree: 17%

---

# Python SDKの初期化

説明
`create` メソッドを使用して Python SDKを初期化し、[!UICONTROL Target Client] をインスタンス化して、実験とパーソナライズされたエクスペリエンスのために [!DNL Adobe Target] を呼び出します。

## メソッド

### 作成

```python {line-numbers="true"}
TargetClient.create(options)
```

## パラメーター

`options` の構造は以下のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| クライアント | str | ○ | None | [!UICONTROL Adobe Target client ID] |
| organization_id | str | ○ | None | [!UICONTROL Experience Cloud Organization ID] |
| timeout | int | × | 3000 | タイムアウト （ミリ秒） |
| server_domain | str | × | `client.tt.omtrdc.net` | デフォルトのホスト名を上書き |
| セキュア | ブール | × | true | HTTP スキームを適用するための設定を解除 |
| ロガー | object | × | 情報ロガー | デフォルトの INFO ロガーを置き換えます |
| target_location_hint | str | × | None | [!DNL Target] location hint |
| property_token | str | × | None | [!DNL Target] プロパティトークン。 ここで指定した場合、すべての get_offers 呼び出しでこの値が使用されます。 |
| decisioning_method | str | × | server-side | 使用する判定方法を決定します（[ オンデバイス ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、サーバーサイド、ハイブリッド） |
| polling_interval | int | × | 300000 （5 分） | [ オンデバイス判定ルールアーティファクト ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) のポーリング間隔（ミリ秒） |
| artifact_location | str | × | None | [ オンデバイス判定ルールアーティファクト ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) への完全修飾 URL。 内部的に決定された場所を上書きします。 |
| artifact_payload | object | × | None | [ オンデバイス判定ルールアーティファクト ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) の JSON ペイロード。 指定した場合、URL からリクエストする代わりに使用されます。 |
| [events](sdk-events.md) | dict &lt;str, callable> | × | None | イベント名のキーとコールバック関数の値を含むオプションのオブジェクト |
| environment_id | int | × | 実稼動 | [!DNL Target] 環境 ID |
| 環境 | str | × | 実稼動 | [!DNL Target] 環境名 |
| cdn_environment | str | × | 実稼動 | CDN 環境名 |
| telemetry_enabled | ブール | × | True | False に設定すると、テレメトリ データは [!DNL Adobe] に送信されません |
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
