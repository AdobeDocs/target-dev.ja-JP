---
title: Experience Cloud A4T レポートとの統合
description: Experience Cloudとの統合、A4T レポート、Analytics for Targetとの統合
keywords: 配信api, サーバーサイド，サーバーサイド，統合，a4t
exl-id: 0d09d7a1-528d-4e6a-bc6c-f7ccd61f5b75
feature: Implement Server-side
TQID: https://experienceleague.adobe.com/Qx5xwszkQLumkFhGJDbvyIofPe7qxUDN922iqmhsClk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 392
ht-degree: 10%

---

# [!UICONTROL Analytics for Target] （A4T） レポート

[!DNL Adobe Target]は、オンデバイス決定とサーバーサイド [!DNL Target]の両方のアクティビティに対するA4T レポートをサポートしています。 A4T レポートを有効にするには、次の2つの設定オプションがあります。

* [!DNL Adobe Target]は、分析ペイロードを自動的に[!DNL Adobe Analytics]に転送します。または
* ユーザーは[!DNL Adobe Target]から分析ペイロードを要求しています。 （[!DNL Adobe Target]は[!DNL Adobe Analytics] ペイロードを呼び出し元に返します。）

>[!NOTE]
>
>オンデバイス判定では、A4T レポートのみがサポートされ、そのうち[!DNL Adobe Target]は分析ペイロードを[!DNL Adobe Analytics]に自動的に転送します。 [!DNL Adobe Target]からの分析ペイロードの取得はサポートされていません。

## 前提条件

1. [!DNL Adobe Analytics]をレポートソースとして[!DNL Adobe Target] UIのアクティビティを設定し、アカウントがA4Tに対して有効になっていることを確認します。
1. API ユーザーはAdobe [!UICONTROL Marketing Cloud Visitor ID]を生成し、[!DNL Target] リクエストの実行時にこのIDが使用可能であることを確認します。

## [!DNL Adobe Target]は[!DNL Analytics] ペイロードを自動的に転送します

次の識別子が指定されている場合、[!DNL Adobe Target]は分析ペイロードを[!DNL Adobe Analytics]に自動的に転送できます。

1. `supplementalDataId`: [!DNL Adobe Analytics]と[!DNL Adobe Target]の間をつなぎ合わせるために使用されるID。 [!DNL Adobe Target]と[!DNL Adobe Analytics]がデータを正しく結合するには、同じ`supplementalDataId`を[!DNL Adobe Target]と[!DNL Adobe Analytics]の両方に渡す必要があります。
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

## ユーザーは[!DNL Adobe Target]から分析ペイロードを取得します

ユーザーは、特定のmboxの[!DNL Adobe Analytics] ペイロードを取得し、[&#x200B; データ挿入API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)を介して[!DNL Adobe Analytics]に送信できます。 [!DNL Adobe Target] リクエストが実行されたら、リクエストの`client_side`を`logging` フィールドに渡します。 指定されたmboxがレポートソースとして[!DNL Analytics]を使用しているアクティビティに存在する場合、このリクエストはペイロードを返します。

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

`logging = client_side`を指定すると、mbox フィールドにペイロードが表示されます。

[!DNL Target]からの応答に`analytics -> payload` プロパティに何かが含まれている場合は、そのまま[!DNL Adobe Analytics]に転送します。 [!DNL Adobe Analytics]はこのペイロードの処理方法を知っています。 これは、次のフォーマットを使用してGET リクエストで実行できます。

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/{content_type_num}/{code_ver}/{session}?pe=tnt&tnta={payload}&c.&a.&target.&sessionId={sessionId}&.target&.a&.c&mid={mid}
```

### クエリー文字列パラメーターと変数

| フィールド名 | 必須 | 説明 |
| --- | --- | --- |
| `rsid` | ○ | レポートスイート ID |
| `content_type_num` | ○ | 常に「0」に設定 |
| `code_ver` | ○ | 常に「MOBILE-1.0」に設定 |
| `session` | ○ | 常に「0」に設定 |
| `pe` | ○ | ページイベント： 常に`tnt`に設定 |
| `tnta` | ○ | [!DNL Analytics] ペイロードが`analytics -> payload -> tnta`の[!DNL Target] サーバーによって返されました |
| `sessionId` | ○ | 進行中セッションの[!DNL Target] セッション ID |
| `mid` | ○ | Marketing Cloud 訪問者 ID |

### 必須ヘッダー値

| ヘッダー名 | ヘッダー値 |
| --- | --- |
| ホスト | Analytics データ収集サーバー（例：`adobeags421.sc.omtrdc.net`） |

### A4T データ挿入のサンプル HTTP Get呼び出し

読みやすさのURL エンコードされていないバージョン （API呼び出しに使用しない形式）:

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0/0?tnta=253236:0:0|0,253236:0:0|2,253236:0:0|1,253613:0:0|0,253613:0:0|2,253613:0:0|1&c.&a.&target.&sessionId=45c08980-f4b9-4e11-96db-067d58e49f74&.target&.a&.c&pe=tnt&mid=69170113867710665996968872592584719577
```

URL エンコードされたバージョン（API呼び出しに使用される形式）:

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0/0?tnta=253236%3A0%3A0%7C0%2C253236%3A0%3A0%7C2%2C253236%3A0%3A0%7C1%2C253613%3A0%3A0%7C0%2C253613%3A0%3A0%7C2%2C253613%3A0%3A0%7C1&c.%26a.%26target.%26sessionId=45c08980-f4b9-4e11-96db-067d58e49f74%26.target%26.a%26.c&pe=tnt&mid=69170113867710665996968872592584719577 
```
