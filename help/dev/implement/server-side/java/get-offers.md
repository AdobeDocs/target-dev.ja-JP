---
title: Java SDK を使用する場合は  [!DNL Adobe Target] getOffers （）を使用します
description: getOffers （）を使用して決定を実行し、 [!DNL Adobe Target] からエクスペリエンスを取得する方法を説明します。
feature: APIs/SDKs
exl-id: 9d7bf956-9d6a-4b4f-a401-2e6814f17f3d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '635'
ht-degree: 13%

---

# オファーを取得（Java）

## 説明

`getOffers()` を使用して、決定を実行し、[!DNL Adobe Target] からエクスペリエンスを取得します。

## メソッド

### getOffers

`TargetClient.getOffers` メソッドのシグネチャを次に示します。

**リクエスト**

```javascript {line-numbers="true"}
TargetDeliveryResponse TargetClient.getOffers(TargetDeliveryRequest request)
```

TargetDeliveryRequest は `TargetDeliveryRequest.builder` を使用して作成されます。

**応答**

```javascript {line-numbers="true"}
TargetDeliveryRequestBuilder TargetDeliveryRequest.builder()
```

## パラメーター

`[!UICONTROL TargetDeliveryRequestBuilder]` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| コンテキスト | コンテキスト | ○ | リクエストのコンテキストを指定します |
| sessionId |  | 文字列 | × | 複数の [!DNL Target] リクエストのリンクに使用 |
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
| mcId | 文字列 | × | 異なる [!DNL Adobe] ソリューション（ECID）間でのデータの結合と共有に使用します。 targetCookies から取得されました。 自動生成（指定されていない場合）。 |
| trackingServer | 文字列 | × | [!DNL Adobe Target] と [!DNL Adobe Analytics] がデータを正しく結び付けるために、Adobe Analytics サーバー。 |
| trackingServerSecure | 文字列 | × | [!DNL Adobe Target] と [!DNL Adobe Analytics] がデータを正しく結び付けるための [!UICONTROL Adobe Analytics Secure Server]。 |
| decisioningMethod | DecisioningMethod | × | オンデバイス判定のために ON_DEVICE または HYBRID Decisioning メソッドを明示的に設定する場合に使用できます |

各フィールドの値は、リクエストの仕様に準拠 *[!UICONTROL Target View Delivery API]* ている必要があります。 *[!UICONTROL Target View Delivery API]* について詳しくは、[http://developers.adobetarget.com/api/#view-delivery-overviewを参照してください ](http://developers.adobetarget.com/api/#view-delivery-overview)


## 応答

`TargetClient.getOffers(`）が返す `TargetDeliveryResponse` は、次のような構造になっています。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| request | TargetDeliveryRequest &#x200B; | *[!DNL Target]* リクエスト |
| response | DeliveryResponse | *[!DNL Target]* response |
| cookie | リスト | このユーザーのセッションメタデータのリスト。 このユーザーの次のターゲットリクエストで渡される必要があります。 |
| visitorState | マップ | 訪問者 API で使用するためにクライアント側で設定される訪問者状態 |
| responseStatus | ResponseStatus | 応答のステータスを表すオブジェクト |

応答の `ResponseStatus` には、次のフィールドが含まれます。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| status | int | [!DNL Target] から返された HTTP ステータス |
| message | 文字列 | HTTP ステータスが 200 でない場合のステータスメッセージ |
| remoteMbox | 文字列のリスト | オンデバイス判定に使用されます。 完全にオンデバイスで決定できないリモートアクティビティを含む mbox のリストが含まれています。 |
| remoteViews | 文字列のリスト | オンデバイス判定に使用されます。 完全にオンデバイスで決定できないリモートアクティビティがあるビューのリストが含まれています。 |

ユーザーセッション用のデータの保存に使用される `TargetCookie` オブジェクトは、次の構造を持ちます。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| name | 文字列 | cookie 名 |
| value | 文字列 | Cookie の値。値は文字列に変換されます |
| maxAge | 数値 | maxAge オプションは、現在の時刻（秒）を基準に有効期限を設定する場合に便利です |

Cookie の有効期限を心配する必要はありません。 Target は SDK 内で maxAge を処理します。

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
