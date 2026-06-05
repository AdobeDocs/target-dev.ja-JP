---
title: create メソッドを使用してNode.js SDKを初期化します。
description: create メソッドを使用してNode.js SDKを初期化し、 [!DNL Target]  クライアントをインスタンス化して、実験とパーソナライズされたエクスペリエンスのために [!DNL Adobe Target] を呼び出す方法を説明します。
feature: APIs/SDKs
exl-id: 71516e44-508a-4d8d-9f2b-7c54243e9c60
TQID: https://experienceleague.adobe.com/uawle0-l5bcv-FuXMLkPc8kIf8DvbkRqAYelr-ehNLk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 332
ht-degree: 18%

---

# Node.js SDKの初期化

## 説明

Node.js SDKを初期化し、[!UICONTROL Target] クライアントをインスタンス化して、[!DNL Adobe Target]を呼び出して実験およびパーソナライズされたエクスペリエンスを実行するには、`create` メソッドを使用します。

## メソッド

**作成**

```js {line-numbers="true"}
TargetClient.create(options: Object): TargetClient
```

## パラメーター

`options`の構造は次のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| クライアント | 文字列 | ○ | None | [!UICONTROL Adobe Target クライアント ID] |
| organizationId | 文字列 | ○ | None | [!UICONTROL Experience Cloud組織ID] |
| 環境 | 文字列 | × | 本番 | ターゲット環境名。 [!DNL Target] UIでは、[!UICONTROL 管理] > [!UICONTROL 環境]です。 |
| timeout | 数値 | × | 3000 | タイムアウト （ミリ秒単位） |
| serverDomain | 文字列 | × | `*client*.tt.omtrdc.net` | デフォルトのホスト名を上書き |
| 安全 | ブール値 | × | true | HTTP スキームを適用する設定を解除 |
| ロガー | オブジェクト | × | NOOP ロガー | デフォルトのNOOP ロガーを置換します |
| targetLocationHint | 文字列 | × | None | ターゲット場所のヒント |
| fetchApi | 関数 | × | global.fetchまたはwindow.fetch | [fetch](https://fetch.spec.whatwg.org/)は、SDKでhttp リクエストに使用されています。 デフォルトでは、node-fetchまたはfetchのブラウザー実装が使用されます。 ただし、別の実装は`fetchApi`を使用して提供できます |
| propertyToken | 文字列 | × | None | **ターゲットプロパティトークン**。 ここで指定した場合、すべての`getOffers`呼び出しがこの値を使用します。 **オンデバイス判定**&#x200B;の場合、SDKは`propertyToken`で設定されたプロパティ トークンの適格アクティビティを含むアーティファクトのみをダウンロードします |
| decisioningMethod | 文字列 | × | サーバーサイド | 使用する決定方法を決定します（[&#x200B; オンデバイス &#x200B;](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、サーバーサイド、ハイブリッド） |
| pollingInterval | 数値 | × | 300000 （5分） | [&#x200B; オンデバイス決定ルール アーティファクト &#x200B;](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)のポーリング間隔（ミリ秒単位） |
| artifactLocation | 文字列 | × | None | [&#x200B; オンデバイス決定ルール アーティファクト &#x200B;](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)への完全修飾URL。 内部で決定された場所を上書きします。 |
| artifactPayload | オブジェクト | × | None | [&#x200B; デバイス上の決定ルール アーティファクト &#x200B;](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)のJSON ペイロード。 指定した場合は、URLからリクエストする代わりに使用されます。 |
| [events](sdk-events.md) | Object&lt;String,Function> | × | None | イベント名キーとコールバック関数値を持つオプションのオブジェクト |
| telemetryEnabled | ブール値 | × | true | 有効にすると、AdobeはSDK機能の使用状況およびパフォーマンスのテレメトリデータを収集します。 個人データは収集されません。 |

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
