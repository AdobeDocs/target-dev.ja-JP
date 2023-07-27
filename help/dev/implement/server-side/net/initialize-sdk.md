---
title: create メソッドを使用して.NET SDK を初期化します。
description: create メソッドを使用して Java SDK を初期化し、 [!UICONTROL TargetClient] 電話をかける [!DNL Adobe Target] 実験とパーソナライズされたエクスペリエンスの場合。
feature: APIs/SDKs
exl-id: 501010c3-22f4-49a8-b2ac-c7307232d180
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '359'
ht-degree: 17%

---

# .NET SDK を初期化します。

## 説明

以下を使用します。 `Create` メソッドを使用して、 .NET SDK を初期化し、 [!UICONTROL Target クライアント] 電話をかける [!DNL Adobe Target] 実験とパーソナライズされたエクスペリエンスの場合。

.NET Dependency Injection を使用する場合は、を呼び出して、サービス設定手順で SDK を追加します。 `services.AddTargetLibrary()`；次に挿入 `ITargetClient targetClient` を設定します。

その後、 `Initialize` メソッドを使用して SDK を設定し、初期化手順を完了します。

## メソッド

`TargetClient` 次を使用して作成： `TargetClient.Create`.

## C\
#

```csharp {line-numbers="true"}
TargetClient TargetClient.Create(TargetClientConfig clientConfig)
```

`ClientConfig` は ClientConfig.Builder を使用して作成されます。

## C\
#

```csharp {line-numbers="true"}
TargetClientConfig.Builder TargetClientConfig.Builder()
```

## パラメーター

`TargetClientConfig.Builder` は次の構造を持ちます。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| クライアント | string | ○ | None | [!UICONTROL Target クライアント ID] |
| 組織 ID | string | ○ | None | [!UICONTROL Experience Cloud組織 ID] |
| タイムアウト | int | × | 10000 | すべてのリクエストのタイムアウト（ミリ秒） |
| プロキシ |  | WebProxy | × | null | すべてのプロキシ [!DNL Target] リクエスト |
| RetryPolicy | ポリシー | × | null | すべてのポリシーを再試行 [!DNL Target] リクエスト |
| AsyncRetryPolicy | AsyncPolicy | × | null | すべての非同期再試行ポリシー [!DNL Target] リクエスト |
| Logger | Ilogger | × | null | のデバッグログに使用されます。 [!DNL Target] リクエストと応答 |
| ServerDomain | string | × | `client.tt.omtrdc.net` | デフォルトのホスト名を上書き |
| セキュア | bool | × | true | HTTP スキームを強制するには設定を解除します |
| DefaultPropertyToken | string | × | null | のデフォルトのプロパティトークンを `getOffers` 呼び出し |
| テレメトリ有効 | bool | × | true | テレメトリデータを送信して SDK の使用エクスペリエンスを向上させる |
| DecisioningMethod | DecisioningMethod enum | × | ServerSide | オンデバイス判定を有効にするには、OnDevice または Hybrid に設定する必要があります |
| OnDeviceDecisioningReady | アクション | × | null | オンデバイス判定の準備完了イベント用にデリゲートします（オンデバイス判定の準備ができたときに 1 回呼び出されます）。 |
| ArtifactDownloadSucceeded | アクション | × | null | オンデバイス判定アーティファクトのダウンロード成功のためにデリゲート（成功したアーティファクトのダウンロードのたびに呼び出されます） |
| ArtifactDownloadFailed | アクション | × | null | オンデバイス判定アーティファクトのダウンロード失敗に対するデリゲート（失敗したアーティファクトのダウンロードごとに呼び出されます） |
| OnDeviceEnvironment | string | × | 実稼動 | ステージングなど、別のオンデバイス環境を指定するために使用できます |
| OnDeviceConfigHostname | string | × | `assets.adobetarget.com` | 別のホストを指定して、オンデバイス判定アーティファクトファイルのダウンロードに使用できます |
| OnDeviceDecisioningPollingIntSecs | int | × | 300（5 分） | オンデバイス判定アーティファクトファイルの取得間隔（秒） |
| OnDeviceArtifactPayload | string | × | null | ローカルアーティファクトのペイロードを使用してオンデバイス判定を提供し、即座に実行できます。 |

## 例

## C\
#

```csharp {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeclient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

targetClient = TargetClient.Create(targetClientConfig);

// make calls to Adobe Target
```
