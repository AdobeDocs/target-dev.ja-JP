---
title: Adobe Target SDK を使用したパーソナライゼーションの配信
description: '[!UICONTROL on-device decisioning] を使用してパーソナライゼーションを配信する方法を説明します。'
feature: APIs/SDKs
exl-id: bac64c78-0d3a-40d7-ae2b-afa0f1b8dc4f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# パーソナライゼーションの配信

## 手順の概要

1. 組織の [!UICONTROL on-device decisioning] を有効にする
1. [!UICONTROL Experience Targeting] （XT）アクティビティの作成
1. オーディエンスごとにパーソナライズされたエクスペリエンスの定義
1. オーディエンスごとにパーソナライズされたエクスペリエンスの検証
1. レポートの設定
1. KPI を追跡するための指標の追加
1. パーソナライズされたオファーをアプリケーションに実装
1. コンバージョンイベントを追跡するコードの実装
1. [!UICONTROL Experience Targeting] （XT）パーソナライゼーションアクティビティの有効化

あなたがツーリング会社だとします。 特定の旅行パッケージに対して 25% オフのパーソナライズされたオファーを配信する場合。 オファーがユーザーの共感を得られるよう、宛先の都市のランドマークを表示することにしました。 また、パーソナライズされたオファーの配信がゼロに近い待ち時間で実行されるようにすることで、ユーザーエクスペリエンスに悪影響を与えず、結果をゆがめることもありません。

## 1.組織の [!UICONTROL on-device decisioning] を有効にする

1. オンデバイス判定を有効にすることで、A/B アクティビティがほぼゼロの待ち時間で実行されるようになります。 この機能を有効にするには、[!DNL Adobe Target] で **[!UICONTROL Administration]**/**[!UICONTROL Implementation]**/**[!UICONTROL Account details]** に移動し、「**[!UICONTROL On-Device Decisioning]**」トグルを有効にします。

   ![alt 画像 ](assets/asset-odd-toggle.png)

   >[!NOTE]
   >
   >[!UICONTROL On-Device Decisioning] の切り替えを有効または無効にするには、管理者または承認者 [ ユーザーの役割 ](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) が必要です。

   「**[!UICONTROL On-Device Decisioning]**」切替スイッチを有効 [!DNL Adobe Target] すると、クライアントの *ルールアーティファクト* の生成を開始します。

## 2. [!UICONTROL Experience Targeting] （XT）アクティビティの作成

1. [!DNL Adobe Target] で、**[!UICONTROL Activities]** ページに移動し、**[!UICONTROL Create Activity]**/**[!UICONTROL Experience Targeting]** を選択します。

   ![alt 画像 ](assets/asset-xt.png)

1. **[!UICONTROL Create Experience Targeting Activity]** モーダルでは、デフォルトの **[!UICONTROL Web]** オプションを選択した状態（1）のままにし、experience composer として **[!UICONTROL Form]** を選択し（2）、ワークスペースとプロパティを選択し（3）、「**[!UICONTROL Next]**」（4）をクリックします。

   ![alt 画像 ](assets/asset-xt-next.png)

## 3. オーディエンスごとにパーソナライズされたエクスペリエンスを定義する

1. アクティビティ作成の **[!UICONTROL Experiences]** の手順で、「**[!UICONTROL Change Audience]**」をクリックし、カリフォルニア州サンフランシスコへの旅行を希望する訪問者のオーディエンスを作成します。

   ![alt 画像 ](assets/asset-change-audience.png)

1. **[!UICONTROL Create Audience]** モーダルで、`destinationCity = San Francisco` の場所にカスタムルールを定義します。 これは、サンフランシスコに旅行したいユーザーのグループを定義します。

   ![alt 画像 ](assets/asset-audience-sf.png)

1. **[!UICONTROL Experiences]** の手順で、ゴールデンゲートBridgeに関する特別なオファーをレンダリングするアプリケーション内の場所の名前（1）を入力します。 次の例では、ホームページは、HTMLオファー（2）に対して選択された場所であり、**[!UICONTROL Content]** の領域で定義されています。

   ![alt 画像 ](assets/asset-content-sf.png)

1. 「**[!UICONTROL Add Experience Targeting]**」をクリックして、別のターゲティングオーディエンスを追加します。 今回は、`destinationCity = New York` のオーディエンスルールを定義して、ニューヨークに旅行するオーディエンスをターゲットにします。 エンパイアステートビルに関する特別なオファーをレンダリングする場所を、アプリケーション内で定義します。 ここに示す例では、`homepage` はHTMLオファー（2）に対して選択された場所であり、**[!UICONTROL Content]** 領域で定義されています。

   ![alt 画像 ](assets/asset-content-ny.png)

## 4. オーディエンスごとにパーソナライズされたエクスペリエンスを検証する

**[!UICONTROL Targeting]** の手順では、オーディエンスごとに目的のパーソナライズされたエクスペリエンスを設定したことを確認します。

![alt 画像 ](assets/asset-verify-sf-ny.png)

## 5. レポートの設定

**[!UICONTROL Goals & Settings]** の手順で、[!DNL Adobe Target] UI でアクティビティの結果を表示する **[!UICONTROL Reporting Source]** として **[!UICONTROL Adobe Target]** を選択するか、Adobe Analytics UI で表示する **[!UICONTROL Adobe Analytics]** を選択します。

![alt 画像 ](assets/asset-reporting-sf-ny.png)

## 6. KPI を追跡するための指標の追加

アクティビティの成功を測定する **[!UICONTROL Goal Metric]** を選択します。 この例では、コンバージョンが成功するかは、ユーザーがパーソナライズされた宛先オファーをクリックしたかどうかに基づきます。

## 7. パーソナライズされたオファーをアプリケーションに実装する

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
    execute: {
      pageLoad: {
        parameters: {
          destinationCity: "San Francisco"
        }
      }
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

Context context = new Context().channel(ChannelType.WEB);

ExecuteRequest executeRequest = new ExecuteRequest();

RequestDetails pageLoad = new RequestDetails();
pageLoad.setParameters(
    new HashMap<String, String>() {
      {
        put("destinationCity", "San Francisco");
      }
    });

executeRequest.setPageLoad(pageLoad);

TargetDeliveryRequest request = TargetDeliveryRequest.builder()
  .context(context)
  .execute(executeRequest)
  .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
```

>[!ENDTABS]

## 8. コンバージョンイベントを追跡するコードの実装

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity

//When a conversion happens
TargetClient.sendNotifications({
    targetCookie,
    "request" : {
      "notifications" : [
        {
          type: "click",
          timestamp : Date.now(),
          id: "conversion",
          mbox : {
            name : "destinationOffer"
          }
        }
      ]
    }
})
```

>[!TAB Java]

```java {line-numbers="true"
ClientConfig config = ClientConfig.builder()
  .client("acmeclient")
  .organizationId("1234567890@AdobeOrg")
  .build();
TargetClient targetClient = TargetClient.create(config);

Context context = new Context().channel(ChannelType.WEB);

ExecuteRequest executeRequest = new ExecuteRequest();

RequestDetails pageLoad = new RequestDetails();
pageLoad.setParameters(
    new HashMap<String, String>() {
      {
        put("destinationCity", "San Francisco");
      }
    });

executeRequest.setPageLoad(pageLoad);
NotificationDeliveryService notificationDeliveryService = new NotificationDeliveryService();

Notification notification = new Notification();
notification.setId("conversion");
notification.setImpressionId(UUID.randomUUID().toString());
notification.setType(MetricType.CLICK);
notification.setTimestamp(System.currentTimeMillis());
notification.setTokens(
    Collections.singletonList(
        "IbG2Jz2xmHaqX7Ml/YRxRGqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="));

TargetDeliveryRequest targetDeliveryRequest =
    TargetDeliveryRequest.builder()
        .context(context)
        .execute(executeRequest)
        .notifications(Collections.singletonList(notification))
        .build();

TargetDeliveryResponse offers = targetClient.getOffers(request);
notificationDeliveryService.sendNotification(request);
```

>[!ENDTABS]

## 9. エクスペリエンスのターゲット設定（XT）アクティビティをアクティブ化する

![alt 画像 ](assets/asset-xt-activate.png)
