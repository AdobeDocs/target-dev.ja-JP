---
title: create メソッドを使用して.NET SDKを初期化する
description: create メソッドを使用してJava SDKを初期化し、[!UICONTROL TargetClient]をインスタンス化して、実験とパーソナライズされたエクスペリエンスのために [!DNL Adobe Target] を呼び出す方法を説明します。
feature: APIs/SDKs
exl-id: 501010c3-22f4-49a8-b2ac-c7307232d180
TQID: https://experienceleague.adobe.com/uOEojoWWjXmcDl2yY1UmSRD-EXL0j9p-p-eE8PXa7Rk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: b18c88053a47a97d6718a69cb72cb4e5d99969c8
workflow-type: tm+mt
source-wordcount: 369
ht-degree: 15%

---

# .NET SDKの初期化

## 説明

`Create` メソッドを使用して、.NET SDKを初期化し、[!UICONTROL Target Client]をインスタンス化して、実験およびパーソナライズされたエクスペリエンスのために[!DNL Adobe Target]を呼び出します。

.NET依存関係インジェクションを使用する場合は、`services.AddTargetLibrary()`；を呼び出してサービス設定手順でSDKを追加し、アプリのコンストラクターに`ITargetClient targetClient`を挿入するだけです。

この後、SDKの`Initialize` メソッドを使用してSDKを設定し、初期化手順を完了します。

## メソッド

`TargetClient`は`TargetClient.Create`を使用して作成されています。

## C&#35;

```csharp {line-numbers="true"}
TargetClient TargetClient.Create(TargetClientConfig clientConfig)
```

`ClientConfig`はClientConfig.Builderを使用して作成されます。

## C&#35;

```csharp {line-numbers="true"}
TargetClientConfig.Builder TargetClientConfig.Builder()
```

## パラメーター

`TargetClientConfig.Builder`の構造は次のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| クライアント | string | ○ | None | [!UICONTROL &#x200B; ターゲットクライアント Id] |
| OrganizationId | string | ○ | None | [!UICONTROL Experience Cloud組織ID] |
| タイムアウト | int | × | 10000 | すべてのリクエストのタイムアウト （ミリ秒単位） |
| プロキシ | WebProxy | × | null | すべての[!DNL Target]要求のプロキシ |
| RetryPolicy | ポリシー | × | null | すべての[!DNL Target]要求に対する再試行ポリシー |
| AsyncRetryPolicy | AsyncPolicy | × | null | すべての[!DNL Target] リクエストの非同期再試行ポリシー |
| ロガー | ILogger | × | null | [!DNL Target]件のリクエストと応答のデバッグログに使用されます |
| ServerDomain | string | × | `client.tt.omtrdc.net` | デフォルトのホスト名を上書き |
| セキュア | ブール | × | true | HTTP スキームを適用する設定を解除 |
| DefaultPropertyToken | string | × | null | `getOffers`呼び出しごとに既定のプロパティ トークンを設定します |
| TelemetryEnabled | ブール | × | true | SDKの使用体験を向上させるためにテレメトリデータを送信する |
| DecisioningMethod | DecisioningMethod列挙 | × | ServerSide | オンデバイス判定を有効にするには、オンデバイスまたはハイブリッドに設定する必要があります |
| OnDeviceDecisioningReady | アクション | × | null | オンデバイス決定準備完了イベント用にデリゲート（オンデバイス決定準備完了の際に1回呼び出されます） |
| ArtifactDownloadSucceeded | アクション | × | null | オンデバイス判定アーティファクトのダウンロードに成功した場合のデリゲート（成功した各アーティファクトダウンロードで呼び出されます） |
| ArtifactDownloadFailed | アクション | × | null | オンデバイス判定アーティファクトのダウンロード失敗のデリゲート（失敗した各アーティファクトのダウンロードで呼び出されます） |
| OnDeviceEnvironment | string | × | 本番 | ステージングなどの異なるオンデバイス環境を指定するために使用できます |
| OnDeviceConfigHostname | string | × | `assets.adobetarget.com` | オンデバイス決定アーティファクトファイルのダウンロードに使用する別のホストを指定するために使用できます |
| OnDeviceDecisioningPollingIntSecs | int | × | 300 （5分） | オンデバイス決定アーティファクトファイルのフェッチ間の秒数 |
| OnDeviceArtifactPayload | string | × | null | ローカルのアーティファクトペイロードを使用したオンデバイス判定を提供して、即座に実行を実行します |

## 例

## C&#35;

```csharp {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeclient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

targetClient = TargetClient.Create(targetClientConfig);

// make calls to Adobe Target
```
