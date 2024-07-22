---
title: ユーザーの識別とバケット化
description: ユーザーの識別とバケット化
exl-id: 4fcf235b-6a58-442c-ae13-9d05ec1033fc
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '1130'
ht-degree: 3%

---

# ユーザーの識別とバケット化

## ユーザー ID

[!DNL Adobe Target] 内でユーザーを識別する方法は複数あります。 [!UICONTROL Target] では、次の識別子を使用します。

| フィールド名 | 説明 |
| --- | --- |
| `tntID` | `tntId` は、ユーザーの [!DNL Target] のプライマリ識別子です。 この ID を指定できます。指定しない [!DNL Target]、リクエストに ID が含まれていない場合は自動生成されます。 |
| `thirdPartyId` | `thirdPartyId` は、ユーザーに対する会社の識別子で、呼び出しごとに送信できます。 ユーザーが会社のサイトにログインすると、会社は通常、訪問者のアカウント、ロイヤルティカード、メンバーシップ番号、またはその会社に適用されるその他の識別子に関連付けられた ID を作成します。 |
| `marketingCloudVisitorId` | `marketingCloudVisitorId` は、異なるAdobeソリューション間でデータを結合して共有するために使用されます。 marketingCloudVisitorId は、Adobe AnalyticsおよびAdobe Audience Managerとの統合に必要です。 |
| `customerIds` | Experience Cloudの訪問者 ID と共に、追加の [ 顧客 ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja) および各訪問者の認証済みステータスも利用できます。 |

## [!DNL Target] ID （tntID）

[!DNL Target] ID または `tntId` は、デバイス ID と見なすことができます。 この `tntId` は、リクエストで指定されていない場合、[!DNL Adobe Target] によって自動的に生成されます。 同じユーザーが使用するデバイスに適切なコンテンツを配信するには、後続のリクエストでこの `tntId` を含める必要があります。

次の呼び出し例は、`tntId` が [!DNL Target] に渡されない状況を示しています。

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

`tntId` がない場合、[!DNL Adobe Target] は `tntId` を生成し、次のように応答で提供します。

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

この例では、生成された `tntId` は `10abf6304b2714215b1fd39a870f01afc.35_0` です。 この `tntId` は、セッションをまたいで同じユーザーに対して使用する必要があることに注意してください。

## サードパーティ ID （thirdPartyId）

組織が ID を使用して訪問者を識別する場合は、`thirdPartyID` を使用してコンテンツを配信できます。 `thirdPartyID` は、Web、モバイル、IoT チャネルのいずれかからビジネスとやり取りするかに関係なく、エンドユーザーを識別するためにビジネスで使用される永続的な ID です。 つまり、`thirdPartyId` は、チャネルをまたいで利用できるユーザープロファイルデータを参照します。 ただし、行う [!DNL Adobe Target] Delivery API 呼び出しごとに `thirdPartyID` を指定する必要があります。

次の呼び出し例では、`thirdPartyId` の使用方法を示しています。

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

このシナリオでは、元の呼び出し [!DNL Adobe Target] 渡されなかったので、`tntId` が生成され、指定された `thirdPartyId` にマッピングされます。

## Marketing Cloud訪問者 ID （marketingCloudVisitorId）

`marketingCloudVisitorId` は、Adobe Experience Cloudのすべてのソリューションをまたいで訪問者を特定する、普遍的で永続的な ID です。 ID サービスを実装する場合、この ID を使用すると、[!DNL Adobe Target]、Adobe Analytics、Adobe Audience Managerなど、様々なExperience Cloudソリューションで同じサイト訪問者とそのデータを特定できます。 [!DNL Target] を [!DNL Adobe Analytics] および [!DNL Adobe Audience Manager] と統合する場合、`marketingCloudVisitorId` が必要であることに注意してください。

次の呼び出し例は、Experience CloudID サービスから取得された `marketingCloudVisitorId` を [!DNL Target] に渡す方法を示しています。

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

このシナリオでは、元の呼び出し [!DNL Target] 渡されなかったので、`tntId` が生成され、指定された `marketingCloudVisitorId` にマッピングされます。

## 顧客 ID （customerIds）

[ 顧客 ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja) は、Experience Cloudの訪問者 ID に追加したり、関連付けたりできます。 `customerIds` を送信する場合は常に、`marketingCloudVisitorId` も指定する必要があります。 さらに、各訪問者毎に、各 `customerId` と共に認証状況を提供することができる。 次の認証ステータスを使用できます。

| 認証状態 | ユーザーステータス |
| --- | --- |
| `unknown` | 不明または認証されていません。 この状態は、ディスプレイ広告をクリックして訪問者がサイトに訪問するようなシナリオに使用できます。 |
| `authenticated` | ユーザーは現在、Web サイトまたはアプリのアクティブセッションで認証されています。 |
| `logged_out` | ユーザーは過去に認証されましたが、アクティブにログアウトしました。認証状態から切断しようとしたユーザー。 ユーザーは、認証済みとして扱われることを希望していません。 |

`customerId` ーザーが認証状態にある場合にのみ、customerId に保存およびリンクされているユーザープロファイルデータを参照で [!DNL Target] ます。 `customerId` が不明な状態または `logged_out` の状態の場合、無視され、その `customerId` に関連付けられている可能性のあるユーザープロファイルデータは、オーディエンスのターゲティングに利用されません。

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

上の例は、`authenticatedState` を持つ `customerId` を送信する方法を示しています。 `customerId` を送信する場合、`integrationCode`、`id`、`authenticatedState` および `marketingCloudVisitorId` が必要です。 `integrationCode` は、CRS で指定した [ 顧客属性ファイル ](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html?lang=ja) のエイリアスです。

## 結合プロファイル

`tntId`、`thirdPartyID`、`marketingCloudVisitorId` を同じリクエストで組み合わせることができます。 このシナリオ [!DNL Adobe Target] は、これらすべての ID のマッピングを維持し、訪問者にピン留めします。 様々な識別子を使用してプロファイルを [ リアルタイムで結合および同期 ](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html) する方法について説明します。

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

上記の例は、`tntId`、`thirdPartyID`、`marketingCloudVisitorId` を同じリクエストで組み合わせる方法を示しています。

## バケット化

[!DNL Adobe Target] アクティビティの設定方法に応じて、ユーザーはエクスペリエンスの表示にバケット化されます。 [!DNL Adobe Target] では、バケット化は次のようになります。

* **決定論的**:MurmurHash3 は、ユーザー ID が一貫している限り、ユーザーのバケット化を確実に行い、毎回適切なバリエーションを表示するために使用されます。
* **スティッキー**:[!DNL Adobe Target] は、ユーザーに表示されるバリエーションをユーザープロファイルに保存し、セッションやチャネルをまたいでバリエーションが一貫してユーザーに表示されるようにします。 サーバー側の決定を使用する場合、バリエーションと粘着性が保証されます。 オンデバイス判定を使用すると、べたつきが保証されません。

## エンドツーエンドのバケット化ワークフロー

実際のバケット化アルゴリズムを詳しく説明する前に、トラフィック割り当ての割合に基づいてアクティビティを選択する場合と、アクティビティ内のエクスペリエンスを選択する場合の両方で同様の手順が使用されていることをハイライトしておくことが重要です。

### アクティビティ選択手順

1. デバイス ID （通常は UUID）を生成します
1. クライアントコードの取得
1. アクティビティ ID の取得
1. salt （通常は「activity」のような文字列）を取得します。
1. MurmurHash3 を使用してハッシュを計算
1. ハッシュの絶対値を取得します
1. ハッシュの絶対値を 10000 で除算します
1. 残りを 10000 で割ると、0 ～ 1 の値が得られます
1. 結果に 100% を掛けます
1. アクティビティのトラフィック割り当ての割合を、取得した割合と比較します。 トラフィック配分の割合が低い場合は、アクティビティが選択されます。 それ以外の場合は、アクティビティはスキップされます。

### エクスペリエンス選択手順

1. デバイス ID （通常は UUID）を生成します
1. クライアントコードの取得
1. アクティビティ ID の取得
1. 塩を入手します。通常は「経験」のような文字列です
1. MurmurHash3 を使用してハッシュを計算
1. ハッシュの絶対値を取得します
1. ハッシュの絶対値を 10000 で除算します
1. 残りを 10000 で割ると、0 ～ 1 の値が得られます
1. 結果にアクティビティ内のエクスペリエンスの合計数を掛けます
1. 結果を丸めます。 これにより、エクスペリエンスインデックスが生成されます。

### 例

次の例を想定します。

* クライアント コード `acmeclient` を使用するクライアント C
* ID `1111` と 3 つのエクスペリエンス（`E1`、`E2`、`E3`）を持つアクティビティ A
* エクスペリエンスの配分は、`E1` ～ 33%、`E2` ～ 33%、`E3` ～ 34% です。

選択フローは次のようになります。

1. デバイス ID `702ff4d0-83b1-4e2e-a0a6-22cbe460eb15`
1. クライアントコード `acmeclient`
1. アクティビティ ID `1111`
1. 塩 `experience`
1. ハッシュ値 `acmeclient.1111.702ff4d0-83b1-4e2e-a0a6-22cbe460eb15.experience`、ハッシュ値 `-919077116`
1. ハッシュ `919077116` の絶対値
1. 10000、`7116` で除算した後の剰余
1. 剰余の後の値を 10000、`0.7116` で割る
1. エクスペリエンスの合計数に値を乗じた結果 `3 * 0.7116 = 2.1348`
1. エクスペリエンスインデックスは `2` です。これは、`0` ベースのインデックスを使用しているので、3 番目のエクスペリエンスを意味します。
