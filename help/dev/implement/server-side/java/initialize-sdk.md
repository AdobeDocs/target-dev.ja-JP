---
title: create メソッドを使用したJava SDKの初期化
description: create メソッドを使用してJava SDKを初期化し、[!UICONTROL TargetClient]をインスタンス化して、実験とパーソナライズされたエクスペリエンスのために [!DNL Adobe Target] を呼び出す方法を説明します。
feature: APIs/SDKs
exl-id: 0e0ddead-7de8-4549-b81c-e72598558e4b
TQID: https://experienceleague.adobe.com/B1Ev7NnjlFMg4VoicF6Z4whyqfJYDjCwPeYRKEk2viY
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 471
ht-degree: 16%

---

# Java SDKの初期化

## 説明

`create` メソッドを使用して、Java SDKを初期化し、[!UICONTROL Target Client]をインスタンス化して、実験およびパーソナライズされたエクスペリエンスのために[!DNL Adobe Target]を呼び出します。

## メソッド

[!UICONTROL TargetClient]は`TargetClient.create`を使用して作成されています。

### 作成

```javascript {line-numbers="true"}
TargetClient TargetClient.create(ClientConfig clientConfig)
```

ClientConfigは`ClientConfig.builder`を使用して作成されます。

```javascript {line-numbers="true"}
ClientConfigBuilder ClientConfig.builder()
```

## パラメーター

`ClientConfigBuilder`の構造は次のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| クライアント | 文字列 | ○ | None | [!UICONTROL  ターゲットクライアント Id] |
| organizationId | 文字列 | ○ | None | [!UICONTROL Experience Cloud組織ID] |
| connectTimeout | 数値 | × | 10000 | すべてのリクエストの接続タイムアウト （ミリ秒単位） |
| socketTimeout | 数値 | × | 10000 | すべてのリクエストのソケットのタイムアウト （ミリ秒単位） |
| maxConnectionsPerHost | 数値 | × | 100 | [!DNL Target] ホストあたりの最大接続数 |
| maxConnectionsTotal | 数値 | × | 200 | すべての[!DNL Target] ホストを含む最大接続 |
| connectionTtlMs | 数値 | × | -1 | TTL （Total time to live）では、永続的な接続の最大有効期間をミリ秒単位で定義します。 デフォルトでは、接続は無期限に維持されます |
| idleConnectionValidationMs | 数値 | × | 1000 | 永続的な接続が再利用される前に再検証されるまでの非アクティブな時間（ミリ秒単位） |
| evictIdleConnectionsAfterSecs | 数値 | × | 20 | 接続プールからアイドル接続を削除する時間（秒） |
| enableRetries | ブール値 | × | true | ソケットタイムアウトの自動再試行（最大4） |
| logRequests | ブール値 | × | false | デバッグに[!DNL Target]件のリクエストと応答を記録します |
| logRequestStatus | ブール値 | × | false | [!DNL Target]の応答時間、ステータス、URLを記録します |
| serverDomain | 文字列 | × | `*client*.tt.omtrdc.net` | デフォルトのホスト名を上書き |
| 安全 | ブール値 | × | true | HTTP スキームを適用する設定を解除 |
| requestInterceptor | HttpRequestInterceptor | × | Null | カスタムリクエストインターセプターを追加 |
| defaultPropertyToken | 文字列 | × | None | `getOffers`呼び出しごとに既定のプロパティ トークンを設定します。 **オンデバイス判定**&#x200B;の場合、SDKは`defaultPropertyToken`で設定されたプロパティ トークンの適格アクティビティを含むアーティファクトのみをダウンロードします |
| defaultDecisioningMethod | DecisioningMethod列挙 | × | SERVER_SIDE | オンデバイス判定を有効にするには、ON_DEVICEまたはHYBRIDに設定する必要があります |
| telemetryEnabled | ブール値 | × | true | [!DNL Target] サーバーへのリクエスト中に、追加のデータ収集をオプトアウトできるようにします |
| proxyConfig | ClientProxyConfig | × | None | クライアントが独自のプロキシの詳細を提供できるようにします |
| exceptionHandler | TargetExceptionHandler | × | None | ルール処理中にカスタム例外処理を実装するために使用できます |
| httpClient | HttpClient | × | None | ユーザーは[!DNL Target] HTTP クライアントをカスタム HTTP クライアントに置き換えることができます |
| onDeviceEnvironment | 文字列 | × | 本番 | ステージングなど、異なるオンデバイス環境を指定するために使用できます |
| onDeviceConfigHostname | 文字列 | × | `assets.adobetarget.com` | オンデバイス決定アーティファクトファイルのダウンロードに使用する別のホストを指定するために使用できます |
| onDeviceDecisioningPollingIntSecs | int | × | 300 （5分） | オンデバイス決定アーティファクトファイルのフェッチ間の秒数 |
| onDeviceArtifactPayload | バイト [] | × | None | 以前のアーティファクトペイロードを使用したオンデバイス判定を提供して、即座に実行できるようにします |
| onDeviceDecisioningHandler | OnDeviceDecisioningHandler | × | None | オンデバイス決定イベントのコールバックを登録します |
| onDeviceAllMatchingRulesMboxes | List\&lt;String\> | × | None | ユーザーは、デバイス上の決定時にすべての一致するルールコンテンツが返されるmboxを指定できます |

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
