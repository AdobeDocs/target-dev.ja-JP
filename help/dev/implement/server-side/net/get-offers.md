---
title: で getOffers() を使用します。 [!DNL Adobe Target] .NET SDK を使用する場合
description: getOffers() を使用して決定を実行し、次の場所からエクスペリエンスを取得する方法を説明します。 [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 4d1d1cbd-c7e5-4146-9fea-08e01923874d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '575'
ht-degree: 14%

---

# オファーの取得 (.NET)

## 説明

`GetOffers()` は、決定を実行し、次の場所からエクスペリエンスを取得するために使用されます。 [!DNL Adobe Target].

## メソッド

`TargetClient.GetOffers` メソッドの署名。

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.GetOffers(TargetDeliveryRequest request)
```

`TargetDeliveryRequest` 次を使用して作成： `TargetDeliveryRequest.Builder`.

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryRequest.Builder TargetDeliveryRequest.Builder()
```

## パラメーター

The `TargetDeliveryRequest.Builder` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| コンテキスト | コンテキスト | ○ | リクエストのコンテキストを指定します |
| sessionId | 文字列 | × | 複数の [!DNL Target] リクエスト |
| thirdPartyId | 文字列 | × | 各呼び出しで送信できるユーザーの会社識別子。 |
| cookie | リスト | × | 以前に返された Cookie のリスト [!DNL Target] 同じユーザーのリクエスト。 |
| customerIds | マップ | × | VisitorId 互換形式の顧客 ID |
| execute | ExecuteRequest | × | ページ読み込みまたは mbox リクエストを実行します。 サーバー側で即座に評価されます |
| prefetch | PrefetchRequest | × | プリフェッチするビュー、ページ読み込み、または mbox リクエスト。 コンバージョン時に返される通知トークンを含むが返されます。 |
| 通知 | リスト | × | プリフェッチされたコンテンツが表示された内容に関する通知を送信するために使用されます。 |
| requestId | 文字列 | × | 応答で返されるリクエスト ID。 存在しない場合は自動的に生成されます。 |
| impressionId | 文字列 | × | 存在する場合、同じ ID を持つ 2 番目以降のリクエストでは、アクティビティ/指標のインプレッション数は増分されません。 存在しない場合は自動的に生成されます。 |
| environmentId | 長い | × | 有効なクライアント環境 ID。 指定されていない場合、指定されたホストに基づいてホストが決定されます。 |
| プロパティ | プロパティ | × | 「トークン」フィールドで at_property を指定します。 配信のスコープを制御するために使用できます。 |
| trace | トレース | × | Delivery API のトレースを有効にします。 |
| qaMode | QAMode | × | リクエストで QA モードを有効にするには、このオブジェクトを使用します。 |
| locationHint | 文字列 | × | [!DNL Target] エッジクラスターの位置のヒント。 このリクエストの特定のエッジクラスターをターゲットにするために使用します。 |
| 訪問者 | 訪問者 | × | カスタム訪問者 API オブジェクトを提供するために使用します。 |
| ID | VisitorId | × | 訪問者の識別子を格納したオブジェクト。 例： tntId、thirdParyId、mcId、customerIds。 |
| experienceCloud | ExperienceCloud | × | Analytics との統合を指定します。 指定されなかった場合、cookie を使用して自動的に入力されます。 |
| tntId | 文字列 | × | のプライマリ識別子 [!DNL Target] を設定します。 targetCookies から取得済み。 指定されていない場合は自動生成されます。 |
| mcId | 文字列 | × | 異なるAdobeソリューション (ECID) 間でデータを結合および共有するために使用されます。 targetCookies から取得済み。 指定されていない場合は自動生成されます。 |
| trackingServer | 文字列 | × | Adobe Analytics Server [!DNL Adobe Target] および [!DNL Adobe Analytics] を使用して、データを正しく結合できます。 |
| trackingServerSecure | 文字列 | × | The [!UICONTROL Adobe Analytics Secure Server] ～のために [!DNL Adobe Target] および [!DNL Adobe Analytics] を使用して、データを正しく結合できます。 |
| decisioningMethod | DecisioningMethod | × | オンデバイス判定の ON_DEVICE またはハイブリッド判定方法を明示的に設定するために使用できます。 |

各フィールドの値は、 [Target 配信 API](/help/dev/implement/delivery-api/overview.md) リクエストの仕様。

## 応答

The `TargetDeliveryResponse` 返送者： `TargetClient.GetOffers()` は次の構造を持ちます。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| リクエスト | TargetDeliveryRequest&#x200B; | [Target 配信 API](/help/dev/implement/delivery-api/overview.md) リクエスト |
| 応答 | DeliveryResponse &#x200B; | [Target 配信 API](/help/dev/implement/delivery-api/overview.md)*応答 |
| ステータス | HttpStatusCode | 応答 HTTP ステータスコード |
| メッセージ | string | 応答ステータスメッセージまたはエラーメッセージ |
| 場所 | 場所 | [!DNL Target] 場所名（グローバル mbox 名、リモート判定のみが使用可能な mbox/ビューなど） |
| GetCookies | 辞書 | このユーザーのセッションメタデータの辞書を返します。 次に渡す必要があります [!DNL Target] このユーザーに対するリクエスト。 |
| VisitorState | IDictionary | 訪問者の状態をクライアント側で設定して、Visitor API JavaScript ライブラリを初期化します。 |

The `TargetCookie` ユーザーセッションのデータを保存するために使用されるオブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| 名前 | string | cookie 名 |
| 値 | string | Cookie の値 |
| MaxAge | int | The `MaxAge` オプションは、現在の時刻（秒）を基準に有効期限を設定する便利な方法です。 |

Cookie の有効期限を気にする必要はありません。 [!DNL Target] ハンドル `MaxAge` を SDK 内に追加します。

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
