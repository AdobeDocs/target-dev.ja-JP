---
title: create メソッドを使用して.NET SDK を初期化します
description: create メソッドを使用して Java SDK を初期化し、[!UICONTROL TargetClient] をインスタンス化して、実験やパーソナライズされたエクスペリエンスを  [!DNL Adobe Target]  び出す呼び出しを行う方法を説明します。
feature: APIs/SDKs
exl-id: 501010c3-22f4-49a8-b2ac-c7307232d180
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '350'
ht-degree: 16%

---

# .NET SDK の初期化

## 説明

`Create` メソッドを使用して.NET SDK を初期化し、[!UICONTROL Target Client] をインスタンス化して、実験およびパーソナライズされたエクスペリエンスのために [!DNL Adobe Target] を呼び出します。

.NET 依存関係の挿入を使用する場合は、`services.AddTargetLibrary()`；を呼び出してサービス設定手順で SDK を追加し、アプリのコンストラクターに `ITargetClient targetClient` を挿入します。

その後、SDK の `Initialize` メソッドを使用して SDK を設定し、初期化手順を完了します。

## メソッド

`TargetClient` は `TargetClient.Create` を使用して作成されます。

## C\#

```csharp {line-numbers="true"}
TargetClient TargetClient.Create(TargetClientConfig clientConfig)
```

`ClientConfig` は ClientConfig.Builder を使用して作成されます。

## C\#

```csharp {line-numbers="true"}
TargetClientConfig.Builder TargetClientConfig.Builder()
```

## パラメーター

`TargetClientConfig.Builder` の構造は以下のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| クライアント | string | ○ | None | [!UICONTROL Target Client Id] |
| OrganizationId | string | ○ | None | [!UICONTROL Experience Cloud Organization ID] |
| タイムアウト | int | × | 10000 | すべての要求のタイムアウト （ミリ秒） |
| プロキシ |  | WebProxy | × | null | すべての [!DNL Target] リクエストのプロキシ |
| RetryPolicy | ポリシー | × | null | すべての [!DNL Target] リクエストの再試行ポリシー |
| AsyncRetryPolicy | AsyncPolicy | × | null | すべての [!DNL Target] 要求の非同期再試行ポリシー |
| ロガー | ILogger | × | null | [!DNL Target] 要求および応答のデバッグログに使用します |
| ServerDomain | string | × | `client.tt.omtrdc.net` | デフォルトのホスト名を上書き |
| セキュア | ブール | × | true | HTTP スキームを適用するための設定を解除 |
| DefaultPropertyToken | string | × | null | `getOffers` 呼び出しごとにデフォルトプロパティトークンを設定します |
| TelemetryEnabled | ブール | × | true | SDK の使用エクスペリエンスを向上させるためのテレメトリデータの送信 |
| DecisioningMethod | DecisioningMethod 列挙 | × | ServerSide | オンデバイス判定を有効にするには、オンデバイスまたはハイブリッドに設定する必要があります |
| OnDeviceDecisioningReady | アクション | × | null | オンデバイス判定準備イベント用にデリゲート （オンデバイス判定の準備が整ったら 1 回呼び出されます） |
| ArtifactDownloadSuccessful | アクション | × | null | オンデバイス判定アーティファクトダウンロード成功のデリゲート（成功したアーティファクトダウンロードのたびに呼び出されます） |
| ArtifactDownloadFailed | アクション | × | null | オンデバイス判定アーティファクトのダウンロード失敗のデリゲート（失敗したアーティファクトのダウンロードのたびに呼び出されます） |
| OnDeviceEnvironment | string | × | 実稼動 | ステージングなど、別のオンデバイス環境を指定するために使用できます |
| OnDeviceConfigHostname | string | × | `assets.adobetarget.com` | オンデバイス判定アーティファクトファイルのダウンロードに使用する別のホストを指定するために使用できます |
| OnDeviceDecisioningPollingIntSecs | int | × | 300 （5 分） | オンデバイス判定アーティファクトファイルのフェッチ間隔（秒） |
| OnDeviceArtifactPayload | string | × | null | ローカルアーティファクトペイロードを使用してオンデバイス判定を提供し、すぐに実行できるようにします |

## 例

## C\#

```csharp {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeclient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

targetClient = TargetClient.Create(targetClientConfig);

// make calls to Adobe Target
```
