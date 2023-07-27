---
title: Experience CloudA4T レポートとの統合
description: Experience Cloudとの統合、A4T レポート、Analytics for Target の統合
keywords: 配信 api，サーバー側，サーバー側，統合， a4t
exl-id: 0d09d7a1-528d-4e6a-bc6c-f7ccd61f5b75
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '360'
ht-degree: 8%

---

# Analytics for Target（A4T）レポート

[!DNL Adobe Target] は、デバイス上での判定とサーバー側での Target アクティビティの両方に対して A4T レポートをサポートしています。 A4T レポートを有効にするには、次の 2 つの設定オプションがあります。

* [!DNL Adobe Target] analytics ペイロードをに自動的に転送します。 [!DNL Adobe Analytics]または
* ユーザーは、次の分析ペイロードをリクエストします： [!DNL Adobe Target]. ([!DNL Adobe Target] は、 [!DNL Adobe Analytics] 呼び出し元にペイロードを戻す。)

>[!NOTE]
>
>オンデバイス判定は、A4T レポートのみをサポートします [!DNL Adobe Target] analytics ペイロードをに自動的に転送します。 [!DNL Adobe Analytics]. からの分析ペイロードの取得 [!DNL Adobe Target] はサポートされていません。

## 前提条件

1. でアクティビティを設定します。 [!DNL Adobe Target] を使用した UI [!DNL Adobe Analytics] をレポートソースとして追加し、A4T でアカウントが有効になっていることを確認します。
1. API ユーザーはAdobe Marketing Cloud訪問者 ID を生成し、Target リクエストの実行時にこの ID を利用できるようにします。

## [!DNL Adobe Target] Analytics ペイロードを自動的に転送します

[!DNL Adobe Target] は、分析ペイロードをに自動的に転送できます。 [!DNL Adobe Analytics] 次の識別子が指定されている場合：

1. `supplementalDataId`：間でステッチに使用される ID [!DNL Adobe Analytics] および [!DNL Adobe Target]. 次の条件を満たすため [!DNL Adobe Target] および [!DNL Adobe Analytics] データを正しく結び付けるには `supplementalDataId` 両方に渡す必要があります [!DNL Adobe Target] および [!DNL Adobe Analytics].
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

## ユーザーが以下から分析ペイロードを取得する [!DNL Adobe Target]

ユーザーは、 [!DNL Adobe Analytics] 特定の mbox のペイロードをに送信し、 [!DNL Adobe Analytics] 経由 [Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md). When in an [!DNL Adobe Target] リクエストが発生した場合、pass `client_side` から `logging` フィールドに値を入力します。 Analytics をレポートソースとして使用しているアクティビティに指定した mbox が存在する場合、ペイロードが返されます。

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

指定した後 `logging = client_side`の場合、ペイロードが mbox フィールドに格納されます。

Target からの応答に `analytics -> payload` プロパティを次のように転送します： [!DNL Adobe Analytics]. [!DNL Adobe Analytics] は、このペイロードの処理方法を知っています。 これは、次の形式を使用して、GETリクエストでおこなうことができます。

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/0/CODEVERSION?pe=tnt&tnta={payload}&mid={mid}&vid={vid}&aid={aid}
```

### クエリー文字列パラメーターおよび変数

| フィールド名 | 必須 | 説明 |
| --- | --- | --- |
| `rsid` | ○ | レポートスイート ID |
| `pe` | ○ | ページイベント。 常にに設定 `tnt` |
| `tnta` | ○ | で Target サーバーによって返された Analytics ペイロード `analytics -> payload -> tnta` |
| `mid` | ○ | Marketing Cloud 訪問者 ID |

### 必須ヘッダー値

| ヘッダー名 | ヘッダー値 |
| --- | --- |
| ホスト | Analytics データ収集サーバー ( 例： `adobeags421.sc.omtrdc.net`) |

### A4T データ挿入 HTTP Get 呼び出しの例

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0?pe=tnt&tnta=285408:0:0|2&mid=2304820394812039
```
