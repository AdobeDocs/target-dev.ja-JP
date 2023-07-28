---
title: ターゲットに通知
description: 追跡が必要なすべてのイベントを [!DNL Target] は、 trackEvent メソッドを使用して送信されます。
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 1291a095a7befed5f795f34099e0411930788e29
workflow-type: tm+mt
source-wordcount: '335'
ht-degree: 0%

---

# 通知 [!DNL Target]

この手順を完了すると、 [!DNL Adobe Target] は、 `trackEvent` メソッド。

で追跡する必要があるイベント [!DNL Target] は、プライマリコンバージョンイベントまたは成功指標にすることができます。

>[!TIP]
>
>このトピックの画像をクリックすると、全画面表示に展開されます。

## 通知 [!DNL Target] 図 {#diagram}

次の図に示すステップ番号は、以下の節に対応しています。

![Target 図を通知](/help/dev/patterns/assets/diagram-notify-target.png){width="600" zoomable="yes"}

## 火 [!DNL Adobe Target] API の追跡

この手順により、に送信する必要のあるすべてのイベントを確実に実行できます。 [!DNL Target] は、 `trackEvent` メソッド。

+++詳細を見る

![Fire Adobe Target Track API の図](/help/dev/patterns/assets/fire-adobe-target-track-api-diagram.png){width="100" zoomable="yes"}

注文コンバージョン属性は、 *前提条件* 」の節を参照してください。 mbox の名前は問題になりませんが、変換は、 `orderConfirmPage`.

この呼び出しに注文コンバージョン属性を含める必要はありません。 これらの呼び出しは、メインのコンバージョンイベントの前のミニコンバージョンイベントと考えられる成功指標を記録するのが理想的です。 `CardIds` は、 `Add to Cart` イベント。

**前提条件**

* ビジネスチームと話し合い、コンバージョン指標や成功指標と見なされるすべてのイベントを特定します。 また、売上高を生み出すコンバージョンイベントを識別して、その詳細をに送信する必要があります。 [!DNL Target] とイベントデータを組み合わせて使用します。
* コンバージョンイベントで送信できるよう、データレイヤーで次の属性が使用可能であることを確認します。 コンバージョンイベントは、製品購入や買い物かごへの追加イベントなどの売上高を生成します。

   * `productPurchaseId`：注文の一部として購入された製品 ID。 複数の製品はコンマで区切ります。
   * `orderTotal`：購入の注文合計。
   * `orderId`：購入の注文 ID。

* 買い物かごへの追加のイベントを追跡している場合は、 `cartIds` をパラメーターとして使用します。 製品 ID のコンマ区切りリストを `cardIds`.

**読み取り**

* [adobe.target.trackEvent() メソッド](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
* [買い物かごに基づく条件の買い物かご ID](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=en#cart-based){target=_blank}

**アクション**

* 用途 `adobe.target-trackEvent()` に送信する必要のあるすべてのデータを送信する方法 [!DNL Target].







