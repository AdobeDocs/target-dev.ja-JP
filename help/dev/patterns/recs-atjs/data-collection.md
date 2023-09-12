---
title: データ収集の設定
description: データ収集に必要なタスクがすべて正しい順序で実行されていることを確認します。
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 6cd78f8e3cbdd97a09b0cb6ca3af55994e85f819
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 3%

---

# データ収集の設定

次の手順に従います： *データ収集* 図を参照して、データ収集に必要なすべてのタスクが正しい順序で実行されていることを確認してください。

>[!TIP]
>
>このトピックの画像をクリックすると、全画面表示に展開されます。

ページの読み込み中、またはページの読み込み後にデータレイヤーが変更される際に、データレイヤーの準備が整います。

次の期間に既にデータをマッピングしている場合： [SDK フェーズの初期化](/help/dev/patterns/recs-atjs/initialize-sdk.md)次の場合は、この図の手順を実行する必要があります。

* 同じページ上でデータレイヤーが何らかの方法で拡張され、その追加データをに送信する必要がある場合。 [!DNL Target]
* 製品カタログデータをに送信する [!DNL Target Recommendations]

## データを収集図 {#diagram}

次の図に示すステップ番号は、以下のセクションに対応しています。

![データ収集図](/help/dev/patterns/recs-atjs/assets/data-collection-diagram.png){width="600" zoomable="yes"}

次のリンクをクリックして、目的のセクションに移動します。

* [2.1：データマッピングの設定](#configure)
* [2.2 エンティティの属性へのリンク](#entity-attributes)
* [2.3 Adobe Target Track API の実行](#fire-api)

## 2.1：データマッピングの設定 {#configure}

この手順は、に送信する必要のあるすべてのデータを確実に送信するのに役立ちます。 [!DNL Adobe Target] が設定されている。

+++詳細を見る

![データマッピング図の設定](/help/dev/patterns/recs-atjs/assets/cofigure-data-mapping.png){width="400" zoomable="yes"}

**前提条件**

* データレイヤーには、に送信する必要のあるすべてのデータの準備が整っている必要があります。 [!DNL Target].

**読み取り**

[targetPageParams 関数 [targetPageParams かんすう ]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**アクション**

以下を使用します。 `targetPageParams()` 関数を使用して、 [!DNL Target].

+++

[このページの上部にある図に戻ります。](#diagram)

## 2.2：エンティティの属性へのリンク {#entity-attributes}

商品カタログを更新するエンティティ属性へのリンク [!DNL Target Recommendations].

+++詳細を見る

**読み取り**

* [エンティティの属性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**注意点**

* エンティティ属性を渡す代わりの方法は、 [!DNL Target] 使用する UI [Recommendations製品フィード](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank}.
* エンティティ属性を渡すことは、データレイヤーで product-catalog データを使用できるページにのみ適用できます。
* を渡す `entity.event.detailsOnly=true` パラメーターは、どの呼び出しでも優先されます。

+++

[このページの上部にある図に戻ります。](#diagram)

## 2.3 Adobe Target Track API の実行 {#fire-api}

この手順は、に送信する必要のあるすべてのデータを確実に送信するのに役立ちます。 [!DNL Target] が送信されます。

+++詳細を見る

![Fire Adobe Target Track API の図](/help/dev/patterns/recs-atjs/assets/fire-track-api.png){width="400" zoomable="yes"}

**前提条件**

* すべてのデータマッピングは、 [targetPageParams 関数 [targetPageParams かんすう ]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md).

**読み取り**

* [adobe.target.trackEvent() メソッド](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)

**アクション**

用途 [adobe.target.trackEvent() メソッド](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) に送信する必要のあるすべてのデータを送信する [!DNL Target].

+++

[このページの上部にある図に戻ります。](#diagram)

