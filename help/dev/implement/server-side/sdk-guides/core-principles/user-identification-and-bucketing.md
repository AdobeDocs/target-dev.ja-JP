---
title: ユーザーの特定とグループ化
description: ユーザーの特定とグループ化
exl-id: 4fcf235b-6a58-442c-ae13-9d05ec1033fc
feature: Implement Server-side
TQID: https://experienceleague.adobe.com/V9hK5oj7F-SV2wou2sz-Ve3RVJ1EMsFJDmcNF4ctV5o
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1172
ht-degree: 6%

---

# ユーザーの特定とグループ化

## ユーザーID

[!DNL Adobe Target]内でユーザーを識別する方法は複数あります。 [!UICONTROL Target]は次の識別子を使用します：

| フィールド名 | 説明 |
| --- | --- |
| `tntID` | `tntId`は、ユーザーの[!DNL Target]のプライマリ IDです。 このIDを指定できます。リクエストにIDが含まれていない場合は、[!DNL Target]が自動生成します。 |
| `thirdPartyId` | `thirdPartyId`は会社のユーザーIDで、電話のたびに送信できます。 ユーザーが企業のサイトにログインすると、通常、その企業は、訪問者のアカウント、ロイヤルティカード、会員番号、またはその他の該当する識別子に関連付けられたIDを作成します。 |
| `marketingCloudVisitorId` | `marketingCloudVisitorId`は、異なるAdobe ソリューション間でデータを結合して共有するために使用されます。 Adobe AnalyticsおよびAdobe Audience Managerとの統合には、marketingCloudVisitorIdが必要です。 |
| `customerIds` | Experience Cloud訪問者IDに加えて、追加の[顧客ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja)と各訪問者の認証済みステータスも使用できます。 |

## [!DNL Target] ID （tntID）

[!DNL Target] ID、つまり`tntId`は、デバイス IDと見なすことができます。 この`tntId`は、リクエストで指定されていない場合、[!DNL Adobe Target]によって自動的に生成されます。 同じユーザーが使用するデバイスに適切なコンテンツを配信するには、後続のリクエストにこの`tntId`を含める必要があります。

次のサンプル呼び出しは、`tntId`が[!DNL Target]に渡されない状況を示しています。

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

`tntId`がない場合、[!DNL Adobe Target]は`tntId`を生成し、次のように応答に提供します。

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

この例では、生成された`tntId`は`10abf6304b2714215b1fd39a870f01afc.35_0`です。 この`tntId`は、セッション間で同じユーザーに使用する必要があることに注意してください。

## サードパーティ ID （thirdPartyId）

組織がIDを使用して訪問者を識別する場合は、`thirdPartyID`を使用してコンテンツを配信できます。 `thirdPartyID`は、エンドユーザーがweb、モバイル、またはIoT チャネルからビジネスと対話するかどうかに関係なく、エンドユーザーを識別するためにビジネスで使用する永続的IDです。 つまり、`thirdPartyId`は、チャネル全体で利用できるユーザープロファイルデータを参照します。 ただし、作成する[!DNL Adobe Target]配信API呼び出しごとに`thirdPartyID`を指定する必要があります。

次のサンプル呼び出しは、`thirdPartyId`の使用を示しています。

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

このシナリオでは、[!DNL Adobe Target]は元の呼び出しに渡されなかったので`tntId`を生成します。これは指定された`thirdPartyId`にマッピングされます。

## Marketing Cloud訪問者ID （marketingCloudVisitorId）

`marketingCloudVisitorId`は、Adobe Experience Cloud内のすべてのソリューションで訪問者を識別する、ユニバーサルで永続的なIDです。 組織がID サービスを実装する場合、このIDを使用すると、[!DNL Adobe Target]、Adobe Analytics、Adobe Audience Managerなど、異なるExperience Cloud ソリューションで同じサイト訪問者とそのデータを識別できます。 [!DNL Target]を[!DNL Adobe Analytics]および[!DNL Adobe Audience Manager]と統合する場合は、`marketingCloudVisitorId`が必要であることに注意してください。

次のサンプル呼び出しは、Experience Cloud ID サービスから取得した`marketingCloudVisitorId`を[!DNL Target]に渡す方法を示しています。

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

このシナリオでは、[!DNL Target]は元の呼び出しに渡されなかったので`tntId`を生成します。これは指定された`marketingCloudVisitorId`にマッピングされます。

## 顧客ID （customerId）

[お客様ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja)は、Experience Cloudの訪問者IDに追加したり、関連付けたりできます。 `customerIds`を送信するたびに、`marketingCloudVisitorId`も指定する必要があります。 さらに、各訪問者に対して、認証ステータスを各`customerId`と共に提供できます。 次の認証ステータスを使用できます。

| 認証状態 | ユーザーステータス |
| --- | --- |
| `unknown` | 不明または認証なし。 この状態は、訪問者がディスプレイ広告をクリックしてサイトにアクセスするようなシナリオに使用できます。 |
| `authenticated` | ユーザーは現在、Web サイトまたはアプリのアクティブセッションで認証されています。 |
| `logged_out` | ユーザーは過去に認証されましたが、アクティブにログアウトしました。 ユーザーは認証状態から切断することを意図しました。 ユーザーは、認証済みとして扱われることを希望していません。 |

`customerId`が認証状態の場合にのみ、[!DNL Target]はcustomerIdに保存されリンクされているユーザープロファイルデータを参照します。 `customerId`が不明または`logged_out`状態の場合、それは無視され、その`customerId`に関連付けられている可能性のあるユーザープロファイルデータはオーディエンスターゲティングに活用されません。

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

上記の例は、`authenticatedState`を含む`customerId`を送信する方法を示しています。 `customerId`を送信する場合、`integrationCode`、`id`、`authenticatedState`および`marketingCloudVisitorId`が必要です。 `integrationCode`は、CRSを通じて指定した[顧客属性ファイル &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html?lang=ja)のエイリアスです。

## 結合プロファイル

同じリクエストで`tntId`、`thirdPartyID`および`marketingCloudVisitorId`を組み合わせることができます。 このシナリオでは、[!DNL Adobe Target]は、これらすべてのIDのマッピングを管理し、訪問者に固定します。 様々な識別子を使用してプロファイルを[統合し、リアルタイムで](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html)同期する方法について説明します。

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

上記の例では、同じリクエストで`tntId`、`thirdPartyID`および`marketingCloudVisitorId`を組み合わせる方法を示しています。

## Bucketing

ユーザーは、[!DNL Adobe Target] アクティビティの設定方法に応じて、表示されるエクスペリエンスに分類されます。 [!DNL Adobe Target]では、グループ化は次のとおりです。

* **決定性**: MurmurHash3は、ユーザーIDが一貫している限り、ユーザーがグループ化され、毎回適切なバリエーションが表示されるようにするために使用されます。
* **スティッキー**: [!DNL Adobe Target]は、ユーザープロファイルに表示されるバリエーションを保存して、バリエーションがセッションとチャネルをまたいで一貫してユーザーに表示されるようにします。 サーバーサイドの意思決定では、バリエーションと粘り強さが保証されます。 オンデバイス判定を使用する場合、粘着性は保証されません。

## エンドツーエンドのグループ化ワークフロー

実際のバケットアルゴリズムに入る前に、同様の手順を使用して、トラフィックの割り当て率に基づいてアクティビティを選択したり、アクティビティ内のエクスペリエンスを選択したりすることが重要です。

### アクティビティの選択ステップ

1. デバイス ID （通常はUUID）の生成
1. クライアントコードの取得
1. アクティビティ IDを取得
1. 塩を得る、これは通常「アクティビティ」のようないくつかの文字列です
1. MurmurHash3を使用してハッシュを計算する
1. ハッシュの絶対値を取得する
1. ハッシュの絶対値を10000で割る
1. 残りを10000で割ると、0 ～ 1の値が生成されます
1. 結果を100%乗算します
1. アクティビティトラフィックの割り当て率を、取得した率と比較します。 トラフィック配分率が低い場合は、アクティビティが選択されます。 それ以外の場合、アクティビティはスキップされます。

### エクスペリエンスの選択ステップ

1. デバイス ID （通常はUUID）の生成
1. クライアントコードの取得
1. アクティビティ IDを取得
1. 塩を入手してください。これは通常、「経験」のような文字列です
1. MurmurHash3を使用してハッシュを計算する
1. ハッシュの絶対値を取得する
1. ハッシュの絶対値を10000で割る
1. 残りを10000で割ると、0 ～ 1の値が生成されます
1. アクティビティ内のエクスペリエンスの合計数に結果を乗算します
1. 結果を丸めます。 これにより、エクスペリエンスのインデックスが生成されます。

### 例

次の例を想定します。

* クライアント C （クライアント コード `acmeclient`）
* ID `1111`と3つのエクスペリエンス `E1`、`E2`、`E3`を持つアクティビティ A
* エクスペリエンスの分布は、`E1` - 33%、`E2` - 33%、`E3` - 34%です

選択フローは次のようになります。

1. デバイス ID `702ff4d0-83b1-4e2e-a0a6-22cbe460eb15`
1. クライアントコード `acmeclient`
1. アクティビティ ID `1111`
1. 塩`experience`
1. 値をハッシュ `acmeclient.1111.702ff4d0-83b1-4e2e-a0a6-22cbe460eb15.experience`、ハッシュ値`-919077116`
1. ハッシュ `919077116`の絶対値
1. 分割後の残り10000、`7116`
1. 残差後の値を10000で割った値、`0.7116`
1. エクスペリエンスの合計数`3 * 0.7116 = 2.1348`に値を乗算した結果
1. エクスペリエンスインデックスは`2`です。これは、`0` ベースのインデックスを使用しているため、3番目のエクスペリエンスを意味します。
