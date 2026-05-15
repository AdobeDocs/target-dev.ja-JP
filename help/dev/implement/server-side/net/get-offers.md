---
title: .NET SDKの使用時に [!DNL Adobe Target] でgetOffers （）を使用する
description: getOffers （）を使用して決定を実行し、 [!DNL Adobe Target]からエクスペリエンスを取得する方法を説明します。
feature: APIs/SDKs
exl-id: 4d1d1cbd-c7e5-4146-9fea-08e01923874d
TQID: https://experienceleague.adobe.com/T-oUyDgCJZ8hqQZgCb3-Z-d9WeMaffwq8krMHhGvYlI
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: f7c7de77-382f-4f48-8b36-61a170f06d3d
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 572
ht-degree: 14%

---

# オファーを取得（.NET）

## 説明

`GetOffers()`は、決定を実行し、[!DNL Adobe Target]からエクスペリエンスを取得するために使用されます。

## メソッド

`TargetClient.GetOffers` メソッドの署名。

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.GetOffers(TargetDeliveryRequest request)
```

`TargetDeliveryRequest`は`TargetDeliveryRequest.Builder`を使用して作成されています。

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryRequest.Builder TargetDeliveryRequest.Builder()
```

## パラメーター

`TargetDeliveryRequest.Builder` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| コンテキスト | コンテキスト | ○ | リクエストのコンテキストを指定します |
| sessionId | 文字列 | × | 複数の[!DNL Target]要求のリンクに使用 |
| thirdPartyId | 文字列 | × | 電話のたびに送信できるユーザーの会社ID |
| cookie | リスト | × | 同じユーザーの以前の[!DNL Target] リクエストで返されたCookieのリスト。 |
| customerId | マップ | × | VisitorIdと互換性のある形式の顧客ID |
| 実行 | ExecuteRequest | × | PageLoadまたはmboxes リクエストを実行します。 サーバーサイドですぐに評価されます |
| 先行取得 | PrefetchRequest | × | ビュー、PageLoadまたはmbox リクエストを先行取得します。 変換時に返される通知トークンで返します。 |
| 通知 | リスト | × | プリフェッチされたコンテンツの表示内容に関する通知を送信するために使用されます |
| requestId | 文字列 | × | 応答で返されるリクエスト ID。 存在しない場合は自動生成。 |
| impressionId | 文字列 | × | 存在する場合、同じIDを持つ2回目以降のリクエストは、アクティビティ/指標へのインプレッションを増やしません。 存在しない場合は自動生成。 |
| environmentId | 長い | × | 有効なクライアント環境ID。 指定しない場合は、指定されたホストに基づいてホストが決定されます。 |
| プロパティ | プロパティ | × | トークンフィールドを使用してat_propertyを指定します。 配信の範囲を制御するために使用できます。 |
| trace | トレース | × | 配信APIのトレースを有効にします。 |
| qaMode | QAMode | × | このオブジェクトを使用して、リクエストでQA モードを有効にします。 |
| locationHint | 文字列 | × | [!DNL Target] エッジ クラスターの場所のヒント。 このリクエストで特定のエッジクラスターをターゲットにするために使用されます。 |
| 訪問者 | 訪問者 | × | カスタム Visitor API オブジェクトを提供するために使用します。 |
| ID | VisitorId | × | 訪問者の識別子を含むオブジェクト。 例： tntId、thirdParyId、mcId、customerIds。 |
| experienceCloud | ExperienceCloud | × | Audience ManagerおよびAnalyticsとの統合を指定します。 提供されていない場合は、Cookieを使用して自動的に入力されます。 |
| tntId | 文字列 | × | ユーザーの[!DNL Target]のプライマリ ID。 targetCookieから取得しました。 指定されていない場合は自動生成。 |
| mcId | 文字列 | × | 異なるAdobe ソリューション（ECID）間のデータの結合と共有に使用されます。 targetCookieから取得しました。 指定されていない場合は自動生成。 |
| trackingServer | 文字列 | × | [!DNL Adobe Target]と[!DNL Adobe Analytics]が正しくデータをつなぎ合わせるために、Adobe Analytics Serverを使用します。 |
| trackingServerSecure | 文字列 | × | [!UICONTROL Adobe Analytics Secure Server]は、[!DNL Adobe Target]と[!DNL Adobe Analytics]がデータを正しく結合するために使用されます。 |
| decisioningMethod | DecisioningMethod | × | オンデバイス判定のためのON_DEVICEまたはハイブリッド判定メソッドを明示的に設定するために使用できます |

各フィールドの値は、[Target配信API](/help/dev/implement/delivery-api/overview.md) リクエスト仕様に準拠している必要があります。

## 応答

`TargetClient.GetOffers()`が返した`TargetDeliveryResponse`の構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| リクエスト | TargetDeliveryRequest&#x200B; | [Target配信API](/help/dev/implement/delivery-api/overview.md) リクエスト |
| 応答 | DeliveryResponse&#x200B; | [Target Delivery API](/help/dev/implement/delivery-api/overview.md)*応答 |
| ステータス | HttpStatusCode | 応答HTTP ステータスコード |
| メッセージ | string | 応答ステータスメッセージまたはエラーメッセージ |
| 場所 | 場所 | リモート決定のみが使用できるグローバル mbox名とmbox/viewsを含む[!DNL Target]の場所の名前 |
| GetCookies | 辞書 | このユーザーのセッションメタデータのディクショナリを返します。 このユーザーの次の[!DNL Target] リクエストで渡す必要があります。 |
| VisitorState | IDictionary | Visitor API Javascript ライブラリの初期化のためにクライアントサイドで設定する訪問者の状態 |

ユーザーセッションのデータを保存するために使用される`TargetCookie` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| 名前 | string | cookie 名 |
| 値 | string | Cookieの値 |
| MaxAge | int | `MaxAge` オプションは、現在の時間を秒単位で有効期限を設定するのに便利です |

Cookieの廃止を心配する必要はありません。 [!DNL Target]は、SDK内の`MaxAge`を処理します。

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
