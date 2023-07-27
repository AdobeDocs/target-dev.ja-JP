---
title: create メソッドを使用して、Node.js SDK を初期化します。
description: create メソッドを使用して Node.js SDK を初期化し、 [!DNL Target] ～に電話をかけるクライアント [!DNL Adobe Target] 実験とパーソナライズされたエクスペリエンスの場合。
feature: APIs/SDKs
exl-id: 71516e44-508a-4d8d-9f2b-7c54243e9c60
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 18%

---

# Node.js SDK を初期化します。

## 説明

以下を使用します。 `create` メソッドを使用して、Node.js SDK を初期化し、 [!UICONTROL Target] ～に電話をかけるクライアント [!DNL Adobe Target] 実験とパーソナライズされたエクスペリエンスの場合。

## メソッド

**作成**

```js {line-numbers="true"}
TargetClient.create(options: Object): TargetClient
```

## パラメーター

`options` は次の構造を持ちます。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| client | 文字列 | ○ | None | [!UICONTROL Adobe Target Client ID] |
| organizationId | 文字列 | ○ | None | [!UICONTROL Experience Cloud組織 ID] |
| 環境 | 文字列 | × | 実稼動 | ターゲット環境名。 Adobe Analytics の [!DNL Target] UI, [!UICONTROL 管理] > [!UICONTROL 環境]. |
| timeout | 数値 | × | 3000 | タイムアウト（ミリ秒） |
| serverDomain | 文字列 | × | `*client*.tt.omtrdc.net` | デフォルトのホスト名を上書き |
| secure | ブール値 | × | true | HTTP スキームを強制するには設定を解除します |
| logger | オブジェクト | × | NOOP ロガー | デフォルトの NOOP ロガーを置き換える |
| targetLocationHint | 文字列 | × | None | Target のロケーションのヒント |
| fetchApi | 関数 | × | global.fetch または window.fetch | [取得](https://fetch.spec.whatwg.org/) は、SDK が http リクエストに使用します。 デフォルトでは、ノードフェッチまたはフェッチのブラウザ実装が使用されます。 ただし、 `fetchApi` |
| propertyToken | 文字列 | × | None | **Target プロパティトークン**. ここで指定した場合、 `getOffers` 呼び出しではこの値が使用されます。 **オンデバイス判定の場合**&#x200B;に設定されている場合、SDK は、 `propertyToken` |
| decisioningMethod | 文字列 | × | サーバーサイド | 使用する判定方法を決定します ([オンデバイス](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、サーバー側、ハイブリッド ) |
| pollingInterval | 数値 | × | 300000（5 分） | のポーリング間隔 [オンデバイス判定ルールアーティファクト](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) （ミリ秒単位） |
| artifactLocation | 文字列 | × | None | の完全修飾 URL [オンデバイス判定ルールアーティファクト](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). 内部的に決定された場所を上書きします。 |
| artifactPayload | オブジェクト | × | None | の JSON ペイロード [オンデバイス判定ルールアーティファクト](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md). 指定した場合、URL からリクエストする代わりに使用されます。 |
| [events](sdk-events.md) | オブジェクト&lt;string function=&quot;&quot;> | × | None | イベント名キーとコールバック関数値を持つオプションのオブジェクトです。 |
| telemetryEnabled | ブール値 | × | true | 有効にすると、Adobeは SDK 機能の使用状況とパフォーマンスのテレメトリデータを収集します。 個人データは収集されません。  |

## 例

### Node.js

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    events: {clientReady: targetClientReady }
};

const targetClient = TargetClient.create(CONFIG);

function targetClientReady() {
    // make calls to Adobe Target
}
```
