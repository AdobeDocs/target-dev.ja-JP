---
title: Experience Cloud A4T レポートとの統合
description: Experience Cloudとの統合、A4T レポート、Analytics for Target の統合
keywords: 配信 api, サーバーサイド，サーバーサイド，統合，a4t
exl-id: 0d09d7a1-528d-4e6a-bc6c-f7ccd61f5b75
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '342'
ht-degree: 7%

---

# Analytics for Target（A4T）レポート

[!DNL Adobe Target] は、オンデバイス判定アクティビティとサーバー側 Target アクティビティの両方で A4T レポートをサポートしています。 A4T レポートを有効にする設定オプションは 2 つあります。

* [!DNL Adobe Target] が analytics ペイロードを自動的に [!DNL Adobe Analytics] に転送する
* ユーザーは、[!DNL Adobe Target] から Analytics ペイロードをリクエストします。 （[!DNL Adobe Target] は [!DNL Adobe Analytics] ペイロードを呼び出し元に返します。）

>[!NOTE]
>
>オンデバイス判定では、[!DNL Adobe Target] が分析ペイロードを [!DNL Adobe Analytics] に自動的に転送する A4T レポートのみをサポートしています。 [!DNL Adobe Target] からの分析ペイロードの取得はサポートされていません。

## 前提条件

1. [!DNL Adobe Analytics] をレポートソースとして使用して [!DNL Adobe Target] UI でアクティビティを設定し、アカウントが A4T に対して有効になっていることを確認します。
1. API ユーザーは、Adobe Marketing Cloud訪問者 ID を生成し、Target リクエストの実行時にこの ID が使用できるようにします。

## [!DNL Adobe Target] が Analytics ペイロードを自動的に転送します

次の識別子 [!DNL Adobe Target] 指定されている場合、analytics ペイロードを [!DNL Adobe Analytics] に自動的に転送できます。

1. `supplementalDataId`:[!DNL Adobe Analytics] と [!DNL Adobe Target] の間でつなぐために使用する ID。 [!DNL Adobe Target] と [!DNL Adobe Analytics] がデータを正しくステッチするには、[!DNL Adobe Target] と [!DNL Adobe Analytics] の両方に同じ `supplementalDataId` を渡す必要があります。
1. `trackingServer`: [!DNL Adobe Analytics] サーバー。

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");

const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
  request: {     
    id: {
      marketingCloudVisitorId : "2304820394812039",
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId:"23423432"
    },
    experienceCloud: {
      analytics: {
        logging: "server_side",
        supplementalDataId: "7D3AA246CC99FD7F-1B3DD2E75595498E",
        trackingServer: "jimsbrims.sc.omtrds.net"
      }
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

>[!TAB Java]

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

AnalyticsRequest analyticsRequest =
    new AnalyticsRequest()
        .trackingServer("jimsbrims.sc.omtrds.net")
        .logging(LoggingType.SERVER_SIDE)
        .supplementalDataId("7D3AA246CC99FD7F-1B3DD2E75595498E");
ExperienceCloud expCloud =
    new ExperienceCloud()
        .setAnalytics(analyticsRequest);

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .experienceCloud(expCloud)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

## ユーザーが [!DNL Adobe Target] から分析ペイロードを取得します

ユーザーは、指定された mbox の [!DNL Adobe Analytics] ペイロードを取得し、[Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) を介して [!DNL Adobe Analytics] に送信できます。 [!DNL Adobe Target] リクエストが発生したら、`client_side` をリクエストの `logging` フィールドに渡します。 これにより、Analytics をレポートソースとして使用するアクティビティに指定された mbox が存在する場合、ペイロードが返されます。

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};
const targetClient = TargetClient.create(CONFIG);
targetClient.getOffers({
  request: {     
    id: {
      marketingCloudVisitorId : "2304820394812039",
      tntId: "d359234570e044f14e1faeeba02d6ab23439914e.35_0",
      thirdPartyId:"23423432"
    },
    experienceCloud: {
      analytics: {
        logging: "client_side"
      }
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

>[!TAB Java]

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

AnalyticsRequest analyticsRequest =
    new AnalyticsRequest()
        .logging(LoggingType.CLIENT_SIDE);
ExperienceCloud expCloud =
    new ExperienceCloud()
        .setAnalytics(analyticsRequest);

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .experienceCloud(expCloud)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

`logging = client_side` を指定すると、ペイロードが mbox フィールドに取り込まれます。

Target からの応答に `analytics -> payload` プロパティ内の値が含まれる場合は、そのまま [!DNL Adobe Analytics] に転送します。 [!DNL Adobe Analytics] のペイロードの処理方法がわかっています。 これは、次のフォーマットを使用したGETリクエストで実行できます。

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/0/CODEVERSION?pe=tnt&tnta={payload}&mid={mid}&vid={vid}&aid={aid}
```

### クエリ文字列のパラメーターと変数

| フィールド名 | 必須 | 説明 |
| --- | --- | --- |
| `rsid` | ○ | レポートスイート ID |
| `pe` | ○ | ページイベント。 常に `tnt` に設定 |
| `tnta` | ○ | `analytics -> payload -> tnta` で Target サーバーから返された Analytics ペイロード |
| `mid` | ○ | Marketing Cloud 訪問者 ID |

### 必須のヘッダー値

| ヘッダー名 | ヘッダー値 |
| --- | --- |
| ホスト | Analytics データ収集サーバー（例：`adobeags421.sc.omtrdc.net`） |

### A4T データ挿入 HTTP Get 呼び出しの例

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0?pe=tnt&tnta=285408:0:0|2&mid=2304820394812039
```
