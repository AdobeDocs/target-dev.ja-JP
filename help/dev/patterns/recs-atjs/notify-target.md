---
title: Notify Target
description: で追跡する必要のあるすべてのイベントは、trackEvent メソ  [!DNL Target]  ドを使用して送信してください。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: efccadab-d139-4423-8613-c2743d87b3a0
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '346'
ht-degree: 1%

---

# [!DNL Target] に通知

この手順を完了すると、[!DNL Adobe Target] に送信する必要のあるすべてのイベントが `trackEvent` メソッドを使用して送信されるようになります。

[!DNL Target] で追跡する必要があるイベントは、プライマリコンバージョンイベントまたは成功指標のいずれかです。

>[!TIP]
>
>全画面表示するには、このトピックの画像をクリックします。

## 図 [!DNL Target] 通知 {#diagram}

次の図のステップ番号は、以下の節に対応しています。

![ ターゲット図への通知 ](/help/dev/patterns/recs-atjs/assets/diagram-notify-target.png){width="600" zoomable="yes"}

## 4.1:Fire [!DNL Adobe Target] Track API

この手順は、[!DNL Target] に送信する必要のあるすべてのイベントを、`trackEvent` メソッドを使用して確実に送信するのに役立ちます。

+++詳細を見る

![Fire Adobe Target Track API の図 ](/help/dev/patterns/recs-atjs/assets/fire-adobe-target-track-api-diagram-combined.png){width="400" zoomable="yes"}

以下の *前提条件* セクションで説明されているように、注文コンバージョン属性を送信します。 mbox の名前は重要ではありませんが、変換には `orderConfirmPage` を使用します。

この呼び出しに注文コンバージョン属性を含める必要はありません。 これらの呼び出しは理想的に、メインコンバージョンイベントの前にミニコンバージョンイベントと考えられる成功指標を記録します。 `Add to Cart` イベントに基づいて、買い物かごベースのレコメンデーションに `CardIds` を含める必要があります。

**前提条件**

* ビジネスチームと会い、コンバージョン指標または成功指標と見なされるすべてのイベントを特定します。 また、イベントデータと共にこれらの詳細を [!DNL Target] に送信できるように、収益を生成するコンバージョンイベントを識別する必要があります。
* コンバージョンイベントで送信できるように、データレイヤーで次の属性が使用可能であることを確認します。 コンバージョンイベントによって売上高（製品購入や買い物かごに追加イベントなど）が生成されます。

   * `productPurchaseId`：注文の一環として購入された製品 ID。 コンマを使用して複数の製品を区切ります。
   * `orderTotal`：購入の注文合計。
   * `orderId`：購入の注文 ID。

  次の図は、[!UICONTROL Confirmation] ページでのみ実行する [ [!DNL tags] in [!DNL Experience Platform]](https://experienceleague.adobe.com/docs/tags.html?lang=ja){target=_blank} のルールを示しています。

  ![ アクション設定ページ ](/help/dev/patterns/recs-atjs/assets/action-configuration.png){width="400" zoomable="yes"}

* 買い物かごへの追加についてイベントをトラッキングしている場合は、`cartIds` をパラメーターとして送信します。 `cardIds` に製品 ID のコンマ区切りリストを渡すことができます。

**読み取り**

* [adobe.target.trackEvent （） メソッド](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
* [ 買い物かごベースの条件の cartIds](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=ja#cart-based){target=_blank}

**アクション**

* メソッド `adobe.target-trackEvent()` 使用して、[!DNL Target] に送信する必要のあるすべてのデータを送信します。
