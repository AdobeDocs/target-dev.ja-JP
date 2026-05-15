---
title: Java SDKを使用する場合は、 [!DNL Adobe Target] でgetOffers （）を使用します
description: getOffers （）を使用して決定を実行し、 [!DNL Adobe Target]からエクスペリエンスを取得する方法を説明します。
feature: APIs/SDKs
exl-id: 9d7bf956-9d6a-4b4f-a401-2e6814f17f3d
TQID: https://experienceleague.adobe.com/2oYkwezf-GkZnybeQUKUbCE6sPAHNQkg3Z1KH8s2a-g
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: f7c7de77-382f-4f48-8b36-61a170f06d3d
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 652
ht-degree: 13%

---

# オファーの取得（Java）

## 説明

`getOffers()`は、決定を実行し、[!DNL Adobe Target]からエクスペリエンスを取得するために使用されます。

## メソッド

### getOffers

`TargetClient.getOffers` メソッドの署名は次のように表示されます。

**リクエスト**

```javascript {line-numbers="true"}
TargetDeliveryResponse TargetClient.getOffers(TargetDeliveryRequest request)
```

TargetDeliveryRequestは`TargetDeliveryRequest.builder`を使用して作成されます。

**応答**

```javascript {line-numbers="true"}
TargetDeliveryRequestBuilder TargetDeliveryRequest.builder()
```

## パラメーター

`[!UICONTROL TargetDeliveryRequestBuilder]` オブジェクトの構造は次のとおりです。

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
| mcId | 文字列 | × | 異なる[!DNL Adobe] ソリューション （ECID）間のデータの結合と共有に使用されます。 targetCookieから取得しました。 指定されていない場合は自動生成。 |
| trackingServer | 文字列 | × | [!DNL Adobe Target]と[!DNL Adobe Analytics]が正しくデータをつなぎ合わせるために、Adobe Analytics Serverを使用します。 |
| trackingServerSecure | 文字列 | × | [!UICONTROL Adobe Analytics Secure Server]は、[!DNL Adobe Target]と[!DNL Adobe Analytics]がデータを正しく結合するために使用されます。 |
| decisioningMethod | DecisioningMethod | × | オンデバイス判定のためのON_DEVICEまたはハイブリッド判定メソッドを明示的に設定するために使用できます |

各フィールドの値は、*[!UICONTROL Target View Delivery API]* リクエスト仕様に準拠する必要があります。 *[!UICONTROL Target View Delivery API]*&#x200B;について詳しくは、[http://developers.adobetarget.com/api/#view-delivery-overview](http://developers.adobetarget.com/api/#view-delivery-overview)を参照してください。


## 応答

`TargetClient.getOffers(`によって返された`TargetDeliveryResponse`には、次の構造があります。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| request | TargetDeliveryRequest&#x200B; | *[!DNL Target]* リクエスト |
| 応答 | DeliveryResponse | *[!DNL Target]*&#x200B;件の応答 |
| cookie | リスト | このユーザーのセッションメタデータのリスト。 このユーザーの次のターゲットリクエストで渡す必要があります。 |
| visitorState | マップ | Visitor APIで使用するクライアントサイドで設定する訪問者の状態 |
| responseStatus | ResponseStatus | 応答のステータスを表すオブジェクト |

応答の`ResponseStatus`には、次のフィールドが含まれています。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| status | int | [!DNL Target]からHTTP ステータスが返されました |
| message | 文字列 | HTTP ステータスが200でない場合のステータスメッセージ |
| remoteMboxes | 文字列のリスト | オンデバイス判定に使用されます。 完全にオンデバイスで決定できないリモート アクティビティを持つmboxのリストが含まれます。 |
| remoteViews | 文字列のリスト | オンデバイス判定に使用されます。 完全にオンデバイスで決定できないリモート アクティビティを持つビューのリストが含まれます。 |

ユーザーセッションのデータを保存するために使用される`TargetCookie` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| name | 文字列 | cookie 名 |
| value | 文字列 | Cookie値の場合、値は文字列に変換されます |
| maxAge | 数値 | maxAge オプションは、現在の時間に対して秒単位で有効期限を設定するのに便利です |

Cookieの廃止を心配する必要はありません。 Targetは、SDK内でmaxAgeを処理します。

## 例

**要求**

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);

List<MboxRequest> mboxRequests = new ArrayList<>();
mboxRequests.add((MboxRequest) new MboxRequest().name("a1-serverside-ab").index(1));

TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
        .context(new Context().channel(ChannelType.WEB))
        .execute(new ExecuteRequest().setMboxes(mboxRequests))
        .build();
```

**応答**

```javascript {line-numbers="true"}
TargetDeliveryResponse targetResponse = targetJavaClient.getOffers(targetDeliveryRequest);
```
