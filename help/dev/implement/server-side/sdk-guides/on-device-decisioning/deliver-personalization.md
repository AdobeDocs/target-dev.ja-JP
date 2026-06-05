---
title: Adobe Target SDKを使用したパーソナライゼーション
description: '[!UICONTROL  オンデバイス判定]を使用してパーソナライゼーションを配信する方法について説明します。'
feature: APIs/SDKs
exl-id: bac64c78-0d3a-40d7-ae2b-afa0f1b8dc4f
TQID: https://experienceleague.adobe.com/IufE4ByFgQ8WwHZ5YVHbbyvN6jBBNGCK4IC98m9zGsc
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 587
ht-degree: 1%

---

# パーソナライゼーションを実現

## 手順の概要

1. 組織の[!UICONTROL  オンデバイス決定]を有効にする
1. [!UICONTROL  エクスペリエンスのターゲット設定] （XT） アクティビティの作成
1. オーディエンスごとにパーソナライズされたエクスペリエンスを定義
1. オーディエンスごとにパーソナライズされたエクスペリエンスを検証
1. レポートの設定
1. KPIを追跡するための指標の追加
1. アプリケーションにパーソナライズされたオファーを実装する
1. コンバージョンイベントを追跡するコードの実装
1. [!UICONTROL  エクスペリエンスターゲティング ] （XT）のパーソナライゼーションアクティビティをアクティブ化する

あなたが旅行会社だと仮定してください。 特定の旅行パッケージを25% オフのパーソナライズされたオファーで提供したい場合。 ユーザーの心に響くオファーを提供するために、目的地の都市のランドマークを表示することにしました。 また、パーソナライズされたオファーの配信が、ほぼゼロの遅延で実行されることを確認し、ユーザーエクスペリエンスに悪影響を与えたり、結果をゆがめたりしないようにすることもできます。

## &#x200B;1. 組織の[!UICONTROL  オンデバイス決定]を有効にする

1. オンデバイス判定を有効にすると、A/B アクティビティがほぼゼロの遅延で実行されます。 この機能を有効にするには、[!DNL Adobe Target]で&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 実装]** > **[!UICONTROL アカウントの詳細]**&#x200B;に移動し、**[!UICONTROL オンデバイス決定]** トグルを有効にします。

   ![alt画像](assets/asset-odd-toggle.png)

   >[!NOTE]
   >
   >[!UICONTROL  オンデバイス決定] トグルを有効または無効にするには、管理者または承認者[ ユーザーの役割](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html)が必要です。

   **[!UICONTROL オンデバイス決定]** トグルを有効にすると、[!DNL Adobe Target]は、クライアントに対して&#x200B;*ルールアーティファクト*&#x200B;の生成を開始します。

## &#x200B;2. [!UICONTROL  エクスペリエンスのターゲット設定] （XT） アクティビティの作成

1. [!DNL Adobe Target]で、**[!UICONTROL アクティビティ]** ページに移動し、**[!UICONTROL アクティビティの作成]** > **[!UICONTROL エクスペリエンスのターゲット設定]**&#x200B;を選択します。

   ![alt画像](assets/asset-xt.png)

1. **[!UICONTROL エクスペリエンスのターゲット設定アクティビティを作成]** モーダルで、デフォルトの&#x200B;**[!UICONTROL Web]** オプションを選択したままにし（1）、エクスペリエンスコンポーザーとして&#x200B;**[!UICONTROL Form]**&#x200B;を選択し（2）、ワークスペースとプロパティを選択し（3）、**[!UICONTROL 次へ]** （4）をクリックします。

   ![alt画像](assets/asset-xt-next.png)

## &#x200B;3. オーディエンスごとにパーソナライズされたエクスペリエンスを定義

1. アクティビティ作成の&#x200B;**[!UICONTROL エクスペリエンス]** ステップで、**[!UICONTROL オーディエンスの変更]**&#x200B;をクリックして、カリフォルニア州サンフランシスコに旅行したい訪問者のオーディエンスを作成します。

   ![alt画像](assets/asset-change-audience.png)

1. **[!UICONTROL オーディエンスを作成]** モーダルで、`destinationCity = San Francisco`というカスタムルールを定義します。 これは、サンフランシスコに旅行したいユーザーのグループを定義します。

   ![alt画像](assets/asset-audience-sf.png)

1. まだ&#x200B;**[!UICONTROL エクスペリエンス]**&#x200B;の手順では、アプリケーション内でゴールデンゲートBridgeに関する特別オファーをレンダリングする場所（1）の名前を入力しますが、サンフランシスコに向かう場合に限ります。 ここで示す例では、ホームページは、HTML オファー（2）用に選択された場所です。この場所は、**[!UICONTROL コンテンツ]**&#x200B;領域で定義されています。

   ![alt画像](assets/asset-content-sf.png)

1. 「**[!UICONTROL エクスペリエンスのターゲット設定を追加]**」をクリックして、別のターゲティングオーディエンスを追加します。 今回は、`destinationCity = New York`のオーディエンスルールを定義して、ニューヨークへの旅行を希望するオーディエンスをターゲットにします。 エンパイアステートビルに関する特別オファーをレンダリングするアプリケーション内の場所を定義します。 ここに示す例では、`homepage`は&#x200B;**[!UICONTROL Content]**&#x200B;領域で定義されているHTML オファー（2）に対して選択された場所です。

   ![alt画像](assets/asset-content-ny.png)

## &#x200B;4. オーディエンスごとにパーソナライズされたエクスペリエンスを検証

「**[!UICONTROL ターゲティング]**」手順で、オーディエンスごとに必要なパーソナライズされたエクスペリエンスが設定されていることを確認します。

![alt画像](assets/asset-verify-sf-ny.png)

## &#x200B;5. レポートの設定

**[!UICONTROL 目標と設定]** ステップで、**[!UICONTROL レポートSource]**&#x200B;として&#x200B;**[!UICONTROL Adobe Target]**&#x200B;を選択して[!DNL Adobe Target] UIでアクティビティの結果を表示するか、**[!UICONTROL Adobe Analytics]**&#x200B;を選択してAdobe Analytics UIでアクティビティの結果を表示します。

![alt画像](assets/asset-reporting-sf-ny.png)

## &#x200B;6. KPIを追跡するための指標の追加

アクティビティの成功を測定するには、**[!UICONTROL 目標指標]**&#x200B;を選択します。 この例では、コンバージョンの成功は、ユーザーがパーソナライズされた宛先オファーをクリックしたかどうかに基づいています。

## &#x200B;7. パーソナライズされたオファーをアプリケーションに実装する

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

## &#x200B;8. コンバージョンイベントを追跡するコードの実装

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

## &#x200B;9. エクスペリエンスターゲティング（XT）アクティビティをアクティブ化する

![alt画像](assets/asset-xt-activate.png)
