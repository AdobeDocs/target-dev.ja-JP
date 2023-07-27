---
title: で getOffers() を使用します。 [!DNL Adobe Target] Java SDK を使用する場合
description: getOffers() を使用して決定を実行し、次の場所からエクスペリエンスを取得する方法を説明します。 [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 9d7bf956-9d6a-4b4f-a401-2e6814f17f3d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '654'
ht-degree: 13%

---

# オファーの取得 (Java)

## 説明

`getOffers()` は、決定を実行し、次の場所からエクスペリエンスを取得するために使用されます。 [!DNL Adobe Target].

## メソッド

### getOffers

The `TargetClient.getOffers` メソッドのシグネチャは次のように表示されます。

**リクエスト**

```javascript {line-numbers="true"}
TargetDeliveryResponse TargetClient.getOffers(TargetDeliveryRequest request)
```

TargetDeliveryRequest が `TargetDeliveryRequest.builder`.

**応答**

```javascript {line-numbers="true"}
TargetDeliveryRequestBuilder TargetDeliveryRequest.builder()
```

## パラメーター

The `[!UICONTROL TargetDeliveryRequestBuilder]` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| コンテキスト | コンテキスト | ○ | リクエストのコンテキストを指定します |
| sessionId |  | 文字列 | × | 複数の [!DNL Target] リクエスト |
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
| mcId | 文字列 | × | 異なる [!DNL Adobe] ソリューション (ECID)。 targetCookies から取得済み。 指定されていない場合は自動生成されます。 |
| trackingServer | 文字列 | × | Adobe Analytics Server [!DNL Adobe Target] および [!DNL Adobe Analytics] を使用して、データを正しく結合できます。 |
| trackingServerSecure | 文字列 | × | The [!UICONTROL Adobe Analytics Secure Server] ～のために [!DNL Adobe Target] および [!DNL Adobe Analytics] を使用して、データを正しく結合できます。 |
| decisioningMethod | DecisioningMethod | × | オンデバイス判定の ON_DEVICE またはハイブリッド判定方法を明示的に設定するために使用できます。 |

各フィールドの値は、 *[!UICONTROL Target ビュー配信 API]* リクエストの仕様。 詳しくは、 *[!UICONTROL Target ビュー配信 API]*&#x200B;を参照してください。 [http://developers.adobetarget.com/api/#view-delivery-overview](http://developers.adobetarget.com/api/#view-delivery-overview)


## 応答

The `TargetDeliveryResponse` 返送者： `TargetClient.getOffers(`) は次の構造を持ちます。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| request | TargetDeliveryRequest&#x200B; | *[!DNL Target]* request |
| 応答 | DeliveryResponse | *[!DNL Target]* 応答 |
| cookie | リスト | このユーザーのセッションメタデータのリスト。 このユーザーの次のターゲットリクエストで渡す必要があります。 |
| visitorState | マップ | 訪問者 API で使用する訪問者の状態をクライアント側で設定する |
| responseStatus | ResponseStatus | 応答のステータスを表すオブジェクト |

The `ResponseStatus` 応答には、次のフィールドが含まれます。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| status | int | から返された HTTP ステータス [!DNL Target] |
| message | 文字列 | HTTP ステータスが 200 でない場合のステータスメッセージ |
| remoteMboxes | 文字列のリスト | オンデバイス判定に使用されます。 完全にデバイス上で判断できないリモートアクティビティを持つ mbox のリストが含まれます。 |
| remoteViews | 文字列のリスト | オンデバイス判定に使用されます。 完全にデバイス上で判断できないリモートアクティビティを持つビューのリストが含まれます。 |

The `TargetCookie` ユーザーセッションのデータを保存するために使用されるオブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| name | 文字列 | cookie 名 |
| value | 文字列 | cookie の値の場合、値は文字列に変換されます |
| maxAge | 数値 | maxAge オプションは、設定の有効期限を現在の時間（秒）に対して設定する際に便利です |

Cookie の有効期限を気にする必要はありません。 Target は、SDK 内で maxAge を処理します。

## 例

**リクエスト**

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
