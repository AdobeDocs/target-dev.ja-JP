---
title: Experience Cloud A4T レポートとの統合
description: Experience Cloudとの統合、A4T レポート、Analytics for Target の統合
keywords: 配信 api, サーバーサイド，サーバーサイド，統合，a4t
exl-id: 0d09d7a1-528d-4e6a-bc6c-f7ccd61f5b75
feature: Implement Server-side
source-git-commit: cbae0f1758fb0dee4837e8c237f8617ecb46eb25
workflow-type: tm+mt
source-wordcount: '372'
ht-degree: 7%

---

# [!UICONTROL Analytics for Target] （A4T）レポート

[!DNL Adobe Target] は、オンデバイス判定アクティビティとサーバーサイド [!DNL Target] ーディエンスアクティビティの両方で A4T レポートをサポートしています。 A4T レポートを有効にする設定オプションは 2 つあります。

* [!DNL Adobe Target] が analytics ペイロードを自動的に [!DNL Adobe Analytics] に転送する
* ユーザーは、[!DNL Adobe Target] から Analytics ペイロードをリクエストします。 （[!DNL Adobe Target] は [!DNL Adobe Analytics] ペイロードを呼び出し元に返します。）

>[!NOTE]
>
>オンデバイス判定では、[!DNL Adobe Target] が分析ペイロードを [!DNL Adobe Analytics] に自動的に転送する A4T レポートのみをサポートしています。 [!DNL Adobe Target] からの分析ペイロードの取得はサポートされていません。

## 前提条件

1. [!DNL Adobe Target] をレポートソースとして使用して [!DNL Adobe Analytics] UI でアクティビティを設定し、アカウントが A4T に対して有効になっていることを確認します。
1. API ユーザーは、Adobe [!UICONTROL Marketing Cloud Visitor ID] を生成し、[!DNL Target] リクエストの実行時にこの ID が使用できることを確認します。

## [!DNL Adobe Target] が [!DNL Analytics] ペイロードを自動的に転送します

次の識別子 [!DNL Adobe Target] 指定されている場合、analytics ペイロードを [!DNL Adobe Analytics] に自動的に転送できます。

1. `supplementalDataId`:[!DNL Adobe Analytics] と [!DNL Adobe Target] の間でつなぐために使用する ID。 [!DNL Adobe Target] と [!DNL Adobe Analytics] がデータを正しくステッチするには、`supplementalDataId` と [!DNL Adobe Target] の両方に同じ [!DNL Adobe Analytics] を渡す必要があります。
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

ユーザーは、指定された mbox の [!DNL Adobe Analytics] ペイロードを取得し、[!DNL Adobe Analytics]Data Insertion API[ を介して ](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) に送信できます。 [!DNL Adobe Target] リクエストが発生したら、`client_side` をリクエストの `logging` フィールドに渡します。 このリクエストは、[!DNL Analytics] をレポートソースとして使用しているアクティビティに指定された mbox が存在する場合、ペイロードを返します。

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

[!DNL Target] からの応答に `analytics -> payload` プロパティ内の値が含まれる場合は、そのまま [!DNL Adobe Analytics] に転送します。 [!DNL Adobe Analytics] のペイロードの処理方法がわかっています。 これは、次のフォーマットを使用したGET リクエストで実行できます。

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/{content_type_num}/{code_ver}/{session}?pe=tnt&tnta={payload}&c.&a.&target.&sessionId={sessionId}&.target&.a&.c&mid={mid}
```

### クエリ文字列のパラメーターと変数

| フィールド名 | 必須 | 説明 |
| --- | --- | --- |
| `rsid` | ○ | レポートスイート ID |
| `content_type_num` | ○ | 常に「0」に設定 |
| `code_ver` | ○ | 常に「MOBILE-1.0」に設定 |
| `session` | ○ | 常に「0」に設定 |
| `pe` | ○ | ページイベント。 常に `tnt` に設定 |
| `tnta` | ○ | サーバーから [!DNL Analytics] で返され [!DNL Target] ペイロードは `analytics -> payload -> tnta` です |
| `sessionId` | ○ | 進行中 [!DNL Target] セッションのセッション ID |
| `mid` | ○ | Marketing Cloud 訪問者 ID |

### 必須のヘッダー値

| ヘッダー名 | ヘッダー値 |
| --- | --- |
| ホスト | Analytics データ収集サーバー（例：`adobeags421.sc.omtrdc.net`） |

### A4T データ挿入 HTTP Get 呼び出しの例

読みやすさを考慮して URL エンコードされていないバージョン （API 呼び出しには使用されない形式）:

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0/0?tnta=253236:0:0|0,253236:0:0|2,253236:0:0|1,253613:0:0|0,253613:0:0|2,253613:0:0|1&c.&a.&target.&sessionId=45c08980-f4b9-4e11-96db-067d58e49f74&.target&.a&.c&pe=tnt&mid=69170113867710665996968872592584719577
```

URL エンコードされたバージョン （API 呼び出しに使用される形式）:

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0/0?tnta=253236%3A0%3A0%7C0%2C253236%3A0%3A0%7C2%2C253236%3A0%3A0%7C1%2C253613%3A0%3A0%7C0%2C253613%3A0%3A0%7C2%2C253613%3A0%3A0%7C1&c.%26a.%26target.%26sessionId=45c08980-f4b9-4e11-96db-067d58e49f74%26.target%26.a%26.c&pe=tnt&mid=69170113867710665996968872592584719577 
```
