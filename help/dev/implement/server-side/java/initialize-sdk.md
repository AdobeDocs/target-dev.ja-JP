---
title: create メソッドを使用して Java SDK を初期化する
description: create メソッドを使用して Java SDK を初期化し、 [!UICONTROL TargetClient] 電話をかける [!DNL Adobe Target] 実験とパーソナライズされたエクスペリエンスの場合。
feature: APIs/SDKs
exl-id: 0e0ddead-7de8-4549-b81c-e72598558e4b
source-git-commit: 1d080b5e402e5d55039bf06611b44678cc6c36de
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 18%

---

# Java SDK の初期化

## 説明

以下を使用します。 `create` メソッドを使用して、Java SDK を初期化し、 [!UICONTROL Target クライアント] 電話をかける [!DNL Adobe Target] 実験とパーソナライズされたエクスペリエンスの場合。

## メソッド

[!UICONTROL TargetClient] 次を使用して作成： `TargetClient.create`.

### 作成

```javascript {line-numbers="true"}
TargetClient TargetClient.create(ClientConfig clientConfig)
```

ClientConfig は `ClientConfig.builder`.

```javascript {line-numbers="true"}
ClientConfigBuilder ClientConfig.builder()
```

## パラメーター

`ClientConfigBuilder` は次の構造を持ちます。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| client | 文字列 | ○ | None | [!UICONTROL Target クライアント ID] |
| organizationId | 文字列 | ○ | None | [!UICONTROL Experience Cloud組織 ID] |
| connectTimeout | 数値 | × | 10000 | すべてのリクエストの接続タイムアウト（ミリ秒） |
| socketTimeout | 数値 | × | 10000 | すべての要求のソケットタイムアウト（ミリ秒） |
| maxConnectionsPerHost | 数値 | × | 100 | 1 回あたりの最大接続数 [!DNL Target] ホスト |
| maxConnectionsTotal | 数値 | × | 200 | すべての [!DNL Target] ホスト |
| connectionTtlMs | 数値 | × | -1 | Total time to live (TTL) は、持続接続の最大寿命をミリ秒単位で定義します。 デフォルトでは、接続は無期限に有効な状態に保たれます |
| idleConnectionValidationMs | 数値 | × | 1000 | 永続的な接続が再利用される前に再検証される非アクティブな期間（ミリ秒） |
| evictIdleConnectionsAfterSecs | 数値 | × | 20 | 接続プールからアイドル状態の接続を削除する時間（秒） |
| enableRetries | ブール値 | × | true | ソケットタイムアウトの自動再試行（最大 4 回） |
| logRequests | ブール値 | × | false | ログ [!DNL Target] デバッグでのリクエストと応答 |
| logRequestStatus | ブール値 | × | false | ログ [!DNL Target] 応答時間、ステータス、URL |
| serverDomain | 文字列 | × | `*client*.tt.omtrdc.net` | デフォルトのホスト名を上書き |
| secure | ブール値 | × | true | HTTP スキームを強制するには設定を解除します |
| requestInterceptor | HttpRequestInterceptor | × | Null | カスタム要求インターセプターの追加 |
| defaultPropertyToken | 文字列 | × | None | のデフォルトのプロパティトークンを `getOffers` を呼び出します。 **オンデバイス判定の場合**&#x200B;に設定されている場合、SDK は、 `defaultPropertyToken` |
| defaultDecisioningMethod | DecisioningMethod enum | × | SERVER_SIDE | オンデバイス判定を有効にするには、ON_DEVICE または HYBRID に設定する必要があります |
| telemetryEnabled | ブール値 | × | true | お客様が [!DNL Target] サーバー |
| proxyConfig | ClientProxyConfig | × | None | クライアントが独自のプロキシの詳細を提供することを許可します |
| exceptionHandler | TargetExceptionHandler | × | None | ルールの処理中にカスタム例外処理を実装するために使用できます。 |
| httpClient | HttpClient | × | None | ユーザーが [!DNL Target] カスタム HTTP クライアントを使用した HTTP クライアント |
| onDeviceEnvironment | 文字列 | × | 実稼動 | ステージングなど、別のオンデバイス環境を指定するために使用できます。 |
| onDeviceConfigHostname | 文字列 | × | `assets.adobetarget.com` | 別のホストを指定して、オンデバイス判定アーティファクトファイルのダウンロードに使用できます |
| onDeviceDecisioningPollingIntSecs | int | × | 300（5 分） | オンデバイス判定アーティファクトファイルの取得間隔（秒） |
| onDeviceArtifactPayload | バイト[] | × | None | 以前のアーティファクトのペイロードを使用したオンデバイス判定機能を提供し、即座に実行できます。 |
| onDeviceDecisioningHandler | OnDeviceDecisioningHandler | × | None | オンデバイス判定イベントのコールバックを登録します |
| onDeviceAllMatchingRulesMboxes | リスト\&lt;string> | × | None | ユーザーが、一致するすべてのルールコンテンツがオンデバイス判定中に返される mbox を指定できるようにします |

## 例

### Java

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient.create(clientConfig);

// make calls to Adobe Target
```
