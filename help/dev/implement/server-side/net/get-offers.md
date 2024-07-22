---
title: .NET SDK を使用する場合は  [!DNL Adobe Target]  で getOffers （）を使用します
description: getOffers （）を使用して決定を実行し、 [!DNL Adobe Target] からエクスペリエンスを取得する方法を説明します。
feature: APIs/SDKs
exl-id: 4d1d1cbd-c7e5-4146-9fea-08e01923874d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 14%

---

# オファーを取得（.NET）

## 説明

`GetOffers()` を使用して、決定を実行し、[!DNL Adobe Target] からエクスペリエンスを取得します。

## メソッド

メソ `TargetClient.GetOffers` ドのシグネチャ。

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.GetOffers(TargetDeliveryRequest request)
```

`TargetDeliveryRequest` は `TargetDeliveryRequest.Builder` を使用して作成されます。

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryRequest.Builder TargetDeliveryRequest.Builder()
```

## パラメーター

`TargetDeliveryRequest.Builder` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| コンテキスト | コンテキスト | ○ | リクエストのコンテキストを指定します |
| sessionId | 文字列 | × | 複数の [!DNL Target] リクエストのリンクに使用 |
| thirdPartyId | 文字列 | × | すべての呼び出しで送信できるユーザーの会社の識別子 |
| cookie | リスト | × | 同じユーザーの以前の [!DNL Target] リクエストで返された cookie のリスト。 |
| customerIds | マップ | × | VisitorId 互換フォーマットの顧客 ID |
| 実行 | ExecuteRequest | × | 実行する PageLoad または mbox リクエスト。 サーバー側で直ちに評価されます |
| プリフェッチ | PrefetchRequest | × | ビュー、PageLoad、または mbox 要求をプリフェッチします。 変換時に返される通知トークンを含むを返します。 |
| 通知 | リスト | × | プリフェッチされたコンテンツが表示されたかに関する通知を送信するために使用されます |
| requestId | 文字列 | × | 応答で返されるリクエスト ID。 存在しない場合は自動的に生成されます。 |
| impressionId | 文字列 | × | 同じ ID を持つ 2 番目以降のリクエストが存在する場合、アクティビティ/指標に対するインプレッションを増分しません。 存在しない場合は自動的に生成されます。 |
| environmentId | 長い | × | 有効なクライアント環境 ID。 指定されていない場合、ホストは指定されたホストに基づいて決定されます。 |
| プロパティ | プロパティ | × | トークンフィールドを使用して at_property を指定します。 配信の範囲を制御するために使用できます。 |
| trace | トレース | × | 配信 API のトレースを有効にします。 |
| qaMode | QAMode | × | このオブジェクトを使用して、リクエストで QA モードを有効にします。 |
| locationHint | 文字列 | × | [!DNL Target] エッジクラスターの場所のヒント。 このリクエストで特定のエッジクラスターをターゲット設定するために使用されます。 |
| 訪問者 | 訪問者 | × | カスタム訪問者 API オブジェクトを提供するために使用されます。 |
| ID | VisitorId | × | 訪問者の識別情報を含むオブジェクト 例： tntId、thirdParyId、mcId、customerIds。 |
| experienceCloud | ExperienceCloud | × | Audience Managerおよび Analytics との統合を指定します。 指定されていない場合、Cookie を使用して自動的に入力されます。 |
| tntId | 文字列 | × | プライマリの [!DNL Target] のユーザー識別子。 targetCookies から取得されました。 自動生成（指定されていない場合）。 |
| mcId | 文字列 | × | 異なるAdobeソリューション（ECID）間でのデータの結合と共有に使用します。 targetCookies から取得されました。 自動生成（指定されていない場合）。 |
| trackingServer | 文字列 | × | [!DNL Adobe Target] と [!DNL Adobe Analytics] がデータを正しく結び付けるために、Adobe Analytics サーバー。 |
| trackingServerSecure | 文字列 | × | [!DNL Adobe Target] と [!DNL Adobe Analytics] がデータを正しく結び付けるための [!UICONTROL Adobe Analytics Secure Server]。 |
| decisioningMethod | DecisioningMethod | × | オンデバイス判定のために ON_DEVICE または HYBRID Decisioning メソッドを明示的に設定する場合に使用できます |

各フィールドの値は、[ ターゲット配信 API](/help/dev/implement/delivery-api/overview.md) リクエストの仕様に準拠する必要があります。

## 応答

`TargetClient.GetOffers()` から返される `TargetDeliveryResponse` の構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| リクエスト | TargetDeliveryRequest &#x200B; | [ ターゲット配信 API](/help/dev/implement/delivery-api/overview.md) リクエスト |
| 応答 | DeliveryResponse&#x200B; | [ ターゲット配信 API](/help/dev/implement/delivery-api/overview.md)*応答 |
| ステータス | HttpStatusCode | 応答 HTTP ステータスコード |
| メッセージ | string | 応答ステータスメッセージまたはエラーメッセージ |
| 場所 | 場所 | [!DNL Target] ロケーション名（グローバル mbox 名および remote decisioning のみ使用可能な mbox/ビューを含む） |
| GetCookies | 辞書 | このユーザーのセッションメタデータの辞書を返します。 これは、このユーザーの次のリクエストで渡 [!DNL Target] 必要があります。 |
| VisitorState | IDictionary | 訪問者 API JavaScript ライブラリの初期化用にクライアントサイドで設定される訪問者状態 |

ユーザーセッション用のデータの保存に使用される `TargetCookie` オブジェクトは、次の構造を持ちます。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| 名前 | string | cookie 名 |
| 値 | string | Cookie の値 |
| MaxAge | int | `MaxAge` オプションは、Expires を現在の時刻（秒）を基準に設定する場合に便利です |

Cookie の有効期限を心配する必要はありません。 [!DNL Target] は SDK 内で `MaxAge` を処理します。

## 例

## \.NET

```dotnet {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeClient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

var targetClient = TargetClient.Create(targetClientConfig);

var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .SetExecute(new ExecuteRequest(mboxes: mboxRequests))
    .Build();

var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
```
