---
title: ターゲットに通知
description: ' [!DNL Target] が追跡する必要があるすべてのイベントが、trackEvent メソッドを使用して送信されていることを確認します。'
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: efccadab-d139-4423-8613-c2743d87b3a0
TQID: https://experienceleague.adobe.com/u-RPLXjG8UBI7bDu2HgPFFnNBU--Yr0UydVX-Q-dcTc
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 369
ht-degree: 0%

---

# [!DNL Target]に通知

この手順を完了すると、[!DNL Adobe Target]に送信する必要があるすべてのイベントが`trackEvent` メソッドを使用して送信されます。

[!DNL Target]で追跡する必要があるイベントは、プライマリコンバージョンイベントまたは成功指標にすることができます。

>[!TIP]
>
>このトピックの画像をクリックして、フルスクリーンに展開します。

## [!DNL Target]図に通知 {#diagram}

次の図の手順の番号は、以下の節に対応しています。

![ ターゲットダイアグラムに通知](/help/dev/patterns/recs-atjs/assets/diagram-notify-target.png){width="600" zoomable="yes"}

## 4.1: [!DNL Adobe Target] Track APIを実行する

この手順は、[!DNL Target]に送信する必要があるすべてのイベントが`trackEvent` メソッドを使用して送信されることを確認するのに役立ちます。

+++詳細を見る

![Fire Adobe Target Track API ダイアグラム ](/help/dev/patterns/recs-atjs/assets/fire-adobe-target-track-api-diagram-combined.png){width="400" zoomable="yes"}

以下の「*前提条件*」セクションに記載されているように、注文コンバージョン属性を送信します。 mboxの名前は問題ではありませんが、変換には`orderConfirmPage`を使用します。

この呼び出しに注文コンバージョン属性を含める必要はありません。 これらの呼び出しは、主なコンバージョンイベントの前にミニコンバージョンイベントと考えることができる成功指標を記録するのが理想的です。 `CardIds`は、`Add to Cart` イベントに基づいてカートベースのレコメンデーションに含める必要があります。

+++

**前提条件**

* ビジネス部門と話し合い、コンバージョンや成功指標として考慮できるあらゆるイベントを特定します。 また、収益を生成するコンバージョンイベントを特定して、その詳細をイベントデータと共に[!DNL Target]に送信できるようにする必要があります。
* コンバージョンイベントで送信できるように、次の属性がデータレイヤーで使用可能であることを確認します。 コンバージョンイベントは、製品の購入やカートに追加イベントなどの収益を生成します。

   * `productPurchaseId`：注文の一環として購入された製品ID。 コンマを使って複数の商品を分けます。
   * `orderTotal`：購入の注文合計。
   * `orderId`：購入の注文ID。

  次の図は、[!UICONTROL 確認] ページでのみ実行される [!DNL tags] in [!DNL Experience Platform]](https://experienceleague.adobe.com/docs/tags.html){target=_blank}の[ ルールを示しています。

  ![ アクション設定ページ ](/help/dev/patterns/recs-atjs/assets/action-configuration.png){width="400" zoomable="yes"}

* カート追加用のイベントを追跡している場合は、`cartIds`をパラメーターとして送信します。 `cardIds`には、製品IDのコンマ区切りリストを渡すことができます。

**読み取り**

* [adobe.target.trackEvent （） メソッド](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
* [カートベースの条件のcartId](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=en#cart-based){target=_blank}

**アクション**

* `adobe.target-trackEvent()` メソッドを使用して、[!DNL Target]に送信する必要があるすべてのデータを送信します。
