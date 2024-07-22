---
title: create メソッドを使用して Node.js SDK を初期化します。
description: create メソッドを使用して Node.js SDK を初期化し、クライアントをインスタンス化して、実験とパーソナライ  [!DNL Target]  されたエクスペリエンスを  [!DNL Adobe Target]  行する呼び出しを行う方法を説明します。
feature: APIs/SDKs
exl-id: 71516e44-508a-4d8d-9f2b-7c54243e9c60
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 19%

---

# Node.js SDK の初期化

## 説明

`create` メソッドを使用して Node.js SDK を初期化し、[!UICONTROL Target] クライアントをインスタンス化して、実験およびパーソナライズされたエクスペリエンスのために [!DNL Adobe Target] を呼び出します。

## メソッド

**作成**

```js {line-numbers="true"}
TargetClient.create(options: Object): TargetClient
```

## パラメーター

`options` の構造は以下のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| クライアント | 文字列 | ○ | None | [!UICONTROL Adobe Target Client ID] |
| organizationId | 文字列 | ○ | None | [!UICONTROL Experience Cloud Organization ID] |
| 環境 | 文字列 | × | 実稼動 | ターゲット環境名。 [!DNL Target] UI で、[!UICONTROL Administration]/[!UICONTROL Environments] を選択します。 |
| timeout | 数値 | × | 3000 | タイムアウト （ミリ秒） |
| serverDomain | 文字列 | × | `*client*.tt.omtrdc.net` | デフォルトのホスト名を上書き |
| セキュア | ブール値 | × | true | HTTP スキームを適用するための設定を解除 |
| ロガー | オブジェクト | × | NOOP ロガー | デフォルトの NOOP ロガーを置き換えます |
| targetLocationHint | 文字列 | × | None | ターゲットの場所のヒント |
| fetchApi | 関数 | × | global.fetch または window.fetch | [fetch](https://fetch.spec.whatwg.org/) は、SDK で http リクエストに使用されます。 デフォルトでは、node-fetch またはブラウザー実装の fetch が使用されます。 ただし、`fetchApi` を使用して代替実装を指定することもできます |
| propertyToken | 文字列 | × | None | **ターゲットプロパティトークン**。 ここで指定した場合、すべての `getOffers` 呼び出しでこの値が使用されます。 **オンデバイス判定の場合**、SDK は、`propertyToken` で設定されたプロパティトークンの対象アクティビティを含んだアーティファクトのみをダウンロードします |
| decisioningMethod | 文字列 | × | server-side | 使用する判定方法を決定します（[ オンデバイス ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、サーバーサイド、ハイブリッド） |
| pollingInterval | 数値 | × | 300000 （5 分） | [ オンデバイス判定ルールアーティファクト ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) のポーリング間隔（ミリ秒） |
| artifactLocation | 文字列 | × | None | [ オンデバイス判定ルールアーティファクト ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) への完全修飾 URL。 内部的に決定された場所を上書きします。 |
| artifactPayload | オブジェクト | × | None | [ オンデバイス判定ルールアーティファクト ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md) の JSON ペイロード。 指定した場合、URL からリクエストする代わりに使用されます。 |
| [events](sdk-events.md) | Object&lt;String,Function> | × | None | イベント名のキーとコールバック関数の値を含むオプションのオブジェクト |
| telemetryEnabled | ブール値 | × | true | 有効化すると、Adobeは SDK 機能の使用状況とパフォーマンスのテレメトリデータを収集します。 個人データは収集されません。  |

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
