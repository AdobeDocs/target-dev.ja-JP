---
title: データ収集の設定
description: データ収集に必要なすべてのタスクが正しい順序で実行されていることを確認します。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 66e0f18d-c78c-463b-8c47-132ef6332927
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 2%

---

# データ収集の設定

*データ収集* 図の手順に従って、データ収集に必要なすべてのタスクが正しい順序で実行されていることを確認します。

>[!TIP]
>
>全画面表示するには、このトピックの画像をクリックします。

データレイヤーは、ページ読み込み中またはページ読み込み後のデータレイヤー変更の準備が整いました。

[SDK の初期化フェーズ ](/help/dev/patterns/recs-atjs/initialize-sdk.md) で既にデータをマッピングしている場合、次の場合はこの図の手順を実行する必要があります。

* データレイヤーは同じページ上で任意の方法で拡張され、その追加データを [!DNL Target] に送信します
* 製品カタログデータを [!DNL Target Recommendations] に送信する

## データの収集ダイアグラム {#diagram}

次の図のステップ番号は、以下のセクションに対応しています。

![ データ収集図 ](/help/dev/patterns/recs-atjs/assets/data-collection-diagram.png){width="600" zoomable="yes"}

次のリンクをクリックして、目的のセクションに移動します。

* [2.1：データマッピングの設定](#configure)
* [2.2 エンティティ属性へのリンク](#entity-attributes)
* [2.3 Adobe Target Track API の実行](#fire-api)

## 2.1：データマッピングの設定 {#configure}

この手順は、[!DNL Adobe Target] に送信する必要があるすべてのデータが設定されていることを確認するのに役立ちます。

+++詳細を見る

![ データマッピング図の設定 ](/help/dev/patterns/recs-atjs/assets/configure-data-mapping-combined.png){width="400" zoomable="yes"}

**前提条件**

* データレイヤーは、[!DNL Target] に送信する必要があるすべてのデータで準備が整っている必要があります。

**読み取り**

[targetPageParams 関数](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**アクション**

`targetPageParams()` 関数を使用して、[!DNL Target] に送信する必要のあるすべての必要なデータを設定します。

+++

[このページの上部の図に戻ります。](#diagram)

## 2.2: エンティティ属性へのリンク {#entity-attributes}

エンティティ属性にリンクして、[!DNL Target Recommendations] の製品カタログを更新します。

+++詳細を見る

**読み取り**

* [ エンティティの属性 ](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**注意点**

* エンティティ属性を渡す別の方法は、[!DNL Target] UI の商品カタログを更新して [Recommendations商品フィード ](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank} を使用することです。
* エンティティ属性の受け渡しは、製品カタログデータをデータレイヤーで使用できるページにのみ適用できます。
* 任意の呼び出しで `entity.event.detailsOnly=true` パラメーターを渡す場合は、が優先されます。

+++

[このページの上部の図に戻ります。](#diagram)

## 2.3 Adobe Target Track API の実行 {#fire-api}

この手順は、[!DNL Target] に送信する必要があるすべてのデータを確実に送信するのに役立ちます。

+++詳細を見る

![Fire Adobe Target Track API の図 ](/help/dev/patterns/recs-atjs/assets/fire-track-api-combined.png){width="400" zoomable="yes"}

**前提条件**

* すべてのデータマッピングは、[targetPageParams 関数 ](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) を使用しておこなう必要があります。

**読み取り**

* [adobe.target.trackEvent （） メソッド](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)

**アクション**

[adobe.target.trackEvent （） メソッド ](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) を使用して、[!DNL Target] に送信する必要のあるすべてのデータを送信します。

+++

[このページの上部の図に戻ります。](#diagram)

手順 3:[ エクスペリエンスのレンダリング ](/help/dev/patterns/recs-atjs/render-experiences.md) に進みます。
