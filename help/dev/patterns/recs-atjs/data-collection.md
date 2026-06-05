---
title: データ収集の設定
description: データ収集に必要なあらゆるタスクが、適切な順序で実行されていることを確認する。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 66e0f18d-c78c-463b-8c47-132ef6332927
TQID: https://experienceleague.adobe.com/fg3xJnwYAVyz-N-xzT5Piu35Ajd2UMEvuTvTQs2wj3c
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 401
ht-degree: 1%

---

# データ収集の設定

*データ収集*&#x200B;図の手順に従って、データ収集に必要なすべてのタスクが正しい順序で実行されるようにします。

>[!TIP]
>
>このトピックの画像をクリックして、フルスクリーンに展開します。

データレイヤーは、ページ読み込み中に準備ができているか、ページ読み込み後にデータレイヤーが変更されます。

[initialize SDK フェーズ &#x200B;](/help/dev/patterns/recs-atjs/initialize-sdk.md)中に既にデータをマッピングしている場合は、次の場合に、この図の手順を実行する必要があります。

* データ層は同じページ上で何らかの方法で拡張されており、その追加データを[!DNL Target]に送信する必要があります
* 商品カタログデータを[!DNL Target Recommendations]に送信する

## データダイアグラムの収集 {#diagram}

次の図の手順の番号は、以下の節に対応しています。

![&#x200B; データ収集図](/help/dev/patterns/recs-atjs/assets/data-collection-diagram.png){width="600" zoomable="yes"}

次のリンクをクリックして、目的のセクションに移動します。

* [2.1: データマッピングの設定](#configure)
* [2.2 エンティティ属性へのリンク](#entity-attributes)
* [2.3 Adobe Target Track APIの起動](#fire-api)

## 2.1: データマッピングの設定 {#configure}

この手順を実行すると、[!DNL Adobe Target]に送信する必要があるすべてのデータが設定されます。

+++詳細を見る

![&#x200B; データマッピングダイアグラムの設定](/help/dev/patterns/recs-atjs/assets/configure-data-mapping-combined.png){width="400" zoomable="yes"}

**前提条件**

* データ層は、[!DNL Target]に送信する必要があるすべてのデータで準備できなければなりません。

**読み取り**

[targetPageParams関数](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**アクション**

`targetPageParams()`関数を使用して、[!DNL Target]に送信する必要があるすべての必須データを設定します。

+++

[このページの上部にある図に戻ります。](#diagram)

## 2.2: エンティティ属性へのリンク {#entity-attributes}

エンティティ属性にリンクして、[!DNL Target Recommendations]の製品カタログを更新します。

+++詳細を見る

**読み取り**

* [エンティティの属性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**注意点**

* エンティティ属性を渡す別の方法として、[!DNL Target] UIで製品カタログを更新して、[推奨事項](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank}を使用する方法があります。
* エンティティ属性の渡しは、製品カタログデータがデータレイヤーで使用可能なページにのみ適用されます。
* 任意の呼び出しで`entity.event.detailsOnly=true` パラメーターを渡すことは優先されます。

+++

[このページの上部にある図に戻ります。](#diagram)

## 2.3 Adobe Target Track APIの起動 {#fire-api}

この手順を実行すると、[!DNL Target]に送信する必要があるすべてのデータが確実に送信されます。

+++詳細を見る

![Fire Adobe Target Track API ダイアグラム &#x200B;](/help/dev/patterns/recs-atjs/assets/fire-track-api-combined.png){width="400" zoomable="yes"}

**前提条件**

* すべてのデータマッピングは、[targetPageParams関数](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)を使用して実行されている必要があります。

**読み取り**

* [adobe.target.trackEvent （） メソッド](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)

**アクション**

[adobe.target.trackEvent （） メソッド &#x200B;](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)を使用して、[!DNL Target]に送信する必要があるすべてのデータを送信します。

+++

[このページの上部にある図に戻ります。](#diagram)

手順3に進みます：[&#x200B; エクスペリエンスをレンダリング &#x200B;](/help/dev/patterns/recs-atjs/render-experiences.md)
