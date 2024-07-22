---
title: create メソッドを使用して Java SDK を初期化します
description: create メソッドを使用して Java SDK を初期化し、[!UICONTROL TargetClient] をインスタンス化して、実験やパーソナライズされたエクスペリエンスを  [!DNL Adobe Target]  び出す呼び出しを行う方法を説明します。
feature: APIs/SDKs
exl-id: 0e0ddead-7de8-4549-b81c-e72598558e4b
source-git-commit: 1d080b5e402e5d55039bf06611b44678cc6c36de
workflow-type: tm+mt
source-wordcount: '452'
ht-degree: 17%

---

# Java SDK の初期化

## 説明

`create` メソッドを使用して Java SDK を初期化し、[!UICONTROL Target Client] をインスタンス化して、実験およびパーソナライズされたエクスペリエンスのために [!DNL Adobe Target] を呼び出します。

## メソッド

[!UICONTROL TargetClient] は `TargetClient.create` を使用して作成されます。

### 作成

```javascript {line-numbers="true"}
TargetClient TargetClient.create(ClientConfig clientConfig)
```

ClientConfig は `ClientConfig.builder` を使用して作成されます。

```javascript {line-numbers="true"}
ClientConfigBuilder ClientConfig.builder()
```

## パラメーター

`ClientConfigBuilder` の構造は以下のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| クライアント | 文字列 | ○ | None | [!UICONTROL Target Client Id] |
| organizationId | 文字列 | ○ | None | [!UICONTROL Experience Cloud Organization ID] |
| connectTimeout | 数値 | × | 10000 | すべての要求の接続タイムアウト （ミリ秒） |
| socketTimeout | 数値 | × | 10000 | すべての要求のソケットタイムアウト （ミリ秒） |
| maxConnectionsPerHost | 数値 | × | 100 | [!DNL Target] ホストあたりの最大接続数 |
| maxConnectionsTotal | 数値 | × | 200 | 最大接続数（すべての [!DNL Target] ホストを含む） |
| connectionTtlMs | 数値 | × | -1 | Total time to live （TTL）は、永続接続の最大有効期間をミリ秒単位で定義します。 デフォルトでは、接続は無期限に有効なままになります |
| idleConnectionValidationMs | 数値 | × | 1000 | 休止状態が続く期間（ミリ秒）。この期間を過ぎると、持続的接続が再利用されるまで再検証されます |
| evictIdleConnectionsAfterSecs | 数値 | × | 20 | 接続プールからアイドル接続を削除する時間（秒） |
| enableRetries | ブール値 | × | true | ソケットタイムアウトの自動再試行（最大 4） |
| logRequests | ブール値 | × | false | リクエスト [!DNL Target] 応答をデバッグに記録 |
| logRequestStatus | ブール値 | × | false | 応答時間 [!DNL Target] ステータスおよび URL のログ記録 |
| serverDomain | 文字列 | × | `*client*.tt.omtrdc.net` | デフォルトのホスト名を上書き |
| セキュア | ブール値 | × | true | HTTP スキームを適用するための設定を解除 |
| requestInterceptor | HttpRequestInterceptor | × | ヌル | カスタムリクエストインターセプターを追加 |
| defaultPropertyToken | 文字列 | × | None | `getOffers` 呼び出しごとにデフォルトのプロパティトークンを設定します。 **オンデバイス判定の場合**、SDK は、`defaultPropertyToken` で設定されたプロパティトークンの対象アクティビティを含んだアーティファクトのみをダウンロードします |
| defaultDecisioningMethod | DecisioningMethod 列挙 | × | サーバー側 | オンデバイス判定を有効にするには、ON_DEVICE またはハイブリッドに設定する必要があります |
| telemetryEnabled | ブール値 | × | true | [!DNL Target] サーバーへのリクエスト時に追加のデータ収集をオプトアウトできる |
| proxyConfig | ClientProxyConfig | × | None | クライアントが独自のプロキシ詳細を指定することを許可します |
| exceptionHandler | TargetExceptionHandler | × | None | ルール処理中にカスタム例外処理を実装するために使用できます |
| httpClient | HttpClient | × | None | ユーザーが [!DNL Target] HTTP クライアントをカスタム HTTP クライアントに置き換えることを許可します |
| onDeviceEnvironment | 文字列 | × | 実稼動 | ステージングなど、別のオンデバイス環境を指定するために使用できます |
| onDeviceConfigHostname | 文字列 | × | `assets.adobetarget.com` | オンデバイス判定アーティファクトファイルのダウンロードに使用する別のホストを指定するために使用できます |
| onDeviceDecisioningPollingIntSecs | int | × | 300 （5 分） | オンデバイス判定アーティファクトファイルのフェッチ間隔（秒） |
| onDeviceArtifactPayload | byte[] | × | None | 以前のアーティファクトペイロードを使用してオンデバイス判定を提供し、すぐに実行できるようにします |
| onDeviceDecisioningHandler | OnDeviceDecisioningHandler | × | None | オンデバイス判定イベントのコールバックを登録します |
| onDeviceAllMatchingRulesMbox | リスト\&lt; 文字列\> | × | None | オンデバイス判定中にすべての一致するルールコンテンツを返す mbox をユーザーが指定できるようにします |

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
