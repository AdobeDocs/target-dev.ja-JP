---
title: ユーザーの識別とグループ化
description: ユーザーの識別とグループ化
exl-id: 4fcf235b-6a58-442c-ae13-9d05ec1033fc
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '1143'
ht-degree: 7%

---

# ユーザーの識別とグループ化

## ユーザー ID

内でユーザーを識別する方法は複数あります [!DNL Adobe Target]. [!UICONTROL Target] は次の識別子を使用します。

| フィールド名 | 説明 |
| --- | --- |
| `tntID` | The `tntId` は、 [!DNL Target] を設定します。 この ID を指定できます。または [!DNL Target] リクエストに含まれていない場合は自動生成されます。 |
| `thirdPartyId` | The `thirdPartyId` は、呼び出しのたびに送信できる、ユーザーの会社識別子です。 ユーザーが会社のサイトにログインすると、通常、会社は訪問者のアカウント、ロイヤルティカード、メンバーシップ番号、またはその会社で適用可能なその他の識別子に結び付けられる ID を作成します。 |
| `marketingCloudVisitorId` | The `marketingCloudVisitorId` は、異なるデータソリューション間でデータを結合および共有するためにAdobeされます。 Adobe AnalyticsおよびAdobe Audience Managerとの統合には、marketingCloudVisitorId が必要です。 |
| `customerIds` | Experience Cloud訪問者 ID と共に、 [顧客 ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja) また、各訪問者の認証済みステータスも利用できます。 |

## [!DNL Target] ID (tntID)

The [!DNL Target] ID、または `tntId`は、デバイス ID と見なすことができます。 この `tntId` が次の項目で自動的に生成されます： [!DNL Adobe Target] を返します。 後続のリクエストには、これを含める必要があります `tntId` 同じユーザーが使用するデバイスに適切なコンテンツを配信するため。

以下の呼び出し例は、 `tntId` が次の値に渡されない [!DNL Target].

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

がない場合 `tntId`, [!DNL Adobe Target] はを生成します。 `tntId` とは、次のようにレスポンスで提供します。

```json {line-numbers="true"}
{
  "status": 200,
  "requestId": "5b586f83-890c-46ae-93a2-610b1caa43ef",
  "client": "acmeclient",
  "id": {
      "tntId": "10abf6304b2714215b1fd39a870f01afc.35_0"
  },
  "edgeHost": "mboxedge35.tt.omtrdc.net",
  ...
}
```

この例では、 `tntId` 次に該当 `10abf6304b2714215b1fd39a870f01afc.35_0`. これに注意してください `tntId` は、セッションをまたいで同じユーザーに対して使用する必要があります。

## サードパーティ ID (thirdPartyId)

組織で ID を使用して訪問者を識別している場合、 `thirdPartyID` コンテンツを配信します。 A `thirdPartyID` は、Web、モバイル、IoT の各チャネルからビジネスとやり取りするかどうかに関係なく、ビジネスがエンドユーザーを識別するために使用する永続的な ID です。 つまり、 `thirdPartyId` は、チャネル間で使用できるユーザープロファイルデータを参照します。 ただし、 `thirdPartyID` 次の期間ごと [!DNL Adobe Target] おこなう配信 API 呼び出し。

以下のサンプル呼び出しは、 `thirdPartyId`.

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      thirdPartyId: "B234A029348"
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .thirdPartyId("B234A029348");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

このシナリオでは、 [!DNL Adobe Target] を生成します `tntId` 元の呼び出しに渡されなかったので、これは指定された `thirdPartyId`.

## Marketing Cloud訪問者 ID (marketingCloudVisitorId)

The `marketingCloudVisitorId` は、Adobe Experience Cloudのすべてのソリューションにわたって訪問者を識別する、普遍的、永続的な ID です。 組織が ID サービスを実装している場合、この ID を使用すると、異なるExperience Cloudソリューション ( [!DNL Adobe Target]、Adobe Analytics、Adobe Audience Manager 次の点に注意してください： `marketingCloudVisitorId` を統合する場合は必須です [!DNL Target] 次を使用 [!DNL Adobe Analytics] および [!DNL Adobe Audience Manager].

次のサンプル呼び出しは、 `marketingCloudVisitorId` Experience CloudID サービスから取得された [!DNL Target].

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      marketingCloudVisitorId: "10527837386392355901041112038610706884"
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .marketingCloudVisitorId("10527837386392355901041112038610706884");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

このシナリオでは、 [!DNL Target] を生成します `tntId` 元の呼び出しに渡されなかったので、これは指定された `marketingCloudVisitorId`.

## 顧客 ID (customerIds)

[顧客 ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja) は、訪問者訪問者 ID に追加したり、Experience CloudID に関連付けたりできます。 送信時 `customerIds`、 `marketingCloudVisitorId` また、を指定する必要があります。 また、各 `customerId` 各訪問者に対して 次の認証ステータスが使用される場合があります。

| 認証状態 | ユーザーステータス |
| --- | --- |
| `unknown` | 不明または認証なし。この状態は、表示広告をクリックして訪問者がサイトに到着した場合などのシナリオで使用できます。 |
| `authenticated` | ユーザーは現在、Web サイトまたはアプリのアクティブセッションで認証されています。 |
| `logged_out` | ユーザーは過去に認証されましたが、アクティブにログアウトしました。ユーザーが認証済み状態から切断しようとした。 ユーザーは、認証済みとして扱われることを希望していません。 |

なお、 `customerId` が認証済み状態になっている場合、 [!DNL Target] customerId に保存され、リンクされているユーザープロファイルデータを参照します。 次の場合、 `customerId` が不明な、または `logged_out` 状態、無視され、それに関連付けられる可能性のあるすべてのユーザープロファイルデータ `customerId` は、オーディエンスのターゲティングには利用されません。

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      marketingCloudVisitorId : "10527837386392355901041112038610706884",
      customerIds: [{
        id: "134325423",
        integrationCode : "crm_data",
        authenticatedState : "authenticated"
      }]
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

CustomerId customerId = new CustomerId()
  .id("134325423")
  .integrationCode("crm_data")
  .authenticatedState(AuthenticatedState.AUTHENTICATED);
VisitorId id = new VisitorId()
  .marketingCloudVisitorId("10527837386392355901041112038610706884")
  .customerIds(Arrays.asList(customerId));
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

上記の例は、 `customerId` と `authenticatedState`. 送信時に `customerId`、 `integrationCode`, `id`、および `authenticatedState` 同様に `marketingCloudVisitorId` が必要です。 The `integrationCode` は、 [顧客属性ファイル](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html?lang=ja) CRS を通じて提供された。

## 結合プロファイル

組み合わせ可能な `tntId`, `thirdPartyID`、および `marketingCloudVisitorId` 同じリクエストに含まれます。 このシナリオでは、 [!DNL Adobe Target] はこれらのすべての ID のマッピングを維持し、訪問者にピン留めします。 プロファイルの概要 [リアルタイムで結合および同期されました](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html) 様々な識別子を使用している。

>[!BEGINTABS]

>[!TAB Node.js SDK]

```javascript {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {
    id: {
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId: "B234A029348",
      marketingCloudVisitorId : "10527837386392355901041112038610706884"
    },
    execute: {
      mboxes: [{
        name: "some-mbox"
      }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!TAB Java SDK]

```java {line-numbers="true"}
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

VisitorId id = new VisitorId()
  .tntId("d359234570e044f14e1faeeba02d6ab23439914e.35_0")
  .thirdPartyId("B234A029348")
  .marketingCloudVisitorId("10527837386392355901041112038610706884");
Context context = new Context().channel(ChannelType.WEB);
MboxRequest mbox = new MboxRequest()
  .name("some-mbox")
  .index(0);
ExecuteRequest executeRequest = new ExecuteRequest()
  .mboxes(Arrays.asList(mbox));

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

上記の例は、 `tntId`, `thirdPartyID`、および `marketingCloudVisitorId` 同じリクエストに含まれます。

## グループ化

ユーザーのエクスペリエンスの表示は、 [!DNL Adobe Target] アクティビティ。 In [!DNL Adobe Target]では、グループ化は次のようになります。

* **決定論的**: MurmurHash3 は、ユーザー ID が一貫している限り、ユーザーがグループ化され、適切なバリエーションが毎回表示されるようにするために使用されます。
* **固定**: [!DNL Adobe Target] ユーザープロファイルにユーザーに表示されるバリエーションを保存して、セッションやチャネルをまたいでユーザーに一貫してバリエーションが表示されるようにします。 サーバー側判定を使用する場合、バリエーションと定着度は保証されます。 オンデバイス判定を使用する場合、スティッキネスは保証されません。

## エンドツーエンドのグループ化ワークフロー

実際のグループ化アルゴリズムを調べる前に、同様の手順を使用して、トラフィック配分の割合に基づいてアクティビティを選択すると共に、アクティビティ内のエクスペリエンスを選択する両方が重要です。

### アクティビティの選択手順

1. デバイス ID（通常は UUID）を生成する
1. クライアントコードの取得
1. アクティビティ ID の取得
1. 通常は「activity」のような文字列である塩を取得します。
1. MurmerHash3 を使用してハッシュを計算します。
1. ハッシュの絶対値を取得します
1. ハッシュの絶対値を10000で割ります
1. 余りを10000で除算します。0 ～ 1 の値を生成します。
1. 結果に 100%を掛けます
1. アクティビティトラフィックの配分率を、取得した割合と比較します。 トラフィックの配分率が低い場合は、「 」アクティビティが選択されます。 それ以外の場合は、「 」アクティビティはスキップされます。

### エクスペリエンスの選択手順

1. デバイス ID（通常は UUID）を生成する
1. クライアントコードの取得
1. アクティビティ ID の取得
1. 塩を取得します。これは通常、「experience」のような文字列です。
1. MurmerHash3 を使用してハッシュを計算します。
1. ハッシュの絶対値を取得します
1. ハッシュの絶対値を10000で割ります
1. 余りを10000で除算します。0 ～ 1 の値を生成します。
1. 結果にアクティビティ内のエクスペリエンスの合計数を掛けます
1. 結果を丸めます。 これにより、エクスペリエンスインデックスが生成されます。

### 例

次のような場合を想定します。

* クライアントコード付きクライアント C `acmeclient`
* ID を持つアクティビティ A `1111` 3 つのエクスペリエンス `E1`, `E2`, `E3`
* エクスペリエンスは次のように分布します。 `E1` - 33%、 `E2` - 33%、 `E3` - 34%

選択フローは次のようになります。

1. デバイス ID `702ff4d0-83b1-4e2e-a0a6-22cbe460eb15`
1. クライアントコード `acmeclient`
1. アクティビティ ID `1111`
1. 塩 `experience`
1. ハッシュ化する値 `acmeclient.1111.702ff4d0-83b1-4e2e-a0a6-22cbe460eb15.experience`，ハッシュ値 `-919077116`
1. ハッシュの絶対値 `919077116`
1. 10000で除算した後の余り `7116`
1. 剰余の後の値は10000で割ります。 `0.7116`
1. 値にエクスペリエンスの合計数を掛けた結果 `3 * 0.7116 = 2.1348`
1. エクスペリエンスのインデックスはです。 `2`は、を使用しているので、3 番目のエクスペリエンスを意味します。 `0` ベースのインデックス作成。
