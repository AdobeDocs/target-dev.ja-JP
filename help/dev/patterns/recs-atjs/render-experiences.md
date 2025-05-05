---
title: エクスペリエンスのレンダリング
description: エクスペリエンスのレンダリングに必要なすべての手順が、正しい順序で実行されていることを確認します。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 7cf0c70b-a4bc-46f4-9b33-099bdb7dd9a9
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '908'
ht-degree: 4%

---

# エクスペリエンスのレンダリング

*エクスペリエンスのレンダリング* 図の手順に従って、エクスペリエンスのレンダリングに必要なすべてのタスクが正しい順序で実行されるようにします。

>[!NOTE]
>
>*SDK を初期化* の [ 自動ページ読み込みリクエストの設定 ](/help/dev/patterns/recs-atjs/initialize-sdk.md#automatic) 手順で自動ページ読み込みリクエストを有効にした場合、Adobe Target SDK を呼び出して地域の場所リクエストを使用して追加のエクスペリエンスをレンダリングする場合を除き、このアクティビティをスキップできます。

>[!TIP]
>
>全画面表示するには、このトピックの画像をクリックします。

## エクスペリエンス図をレンダリング {#diagram}

at.js で使用できる自動標準のちらつき処理は、[!UICONTROL Automatic Page Load Request] に有効にした場合にのみ有効になります。 このオプションを選択すると、[!DNL Target] からエクスペリエンスを取得する際に、HTML本文全体が非表示になります。 この場合、フリッカーの処理はユーザーの責任です。 ガイダンスとして、ちらつきの処理に使用できる実装パターンを検索します。

>[!NOTE]
>
>次の図のステップ番号は、以下のセクションに対応しています。 ステップ番号は特定の順序ではなく、アクティビティの作成時に [!DNL Target] UI で実行されるステップの順序は反映されません。

![ エクスペリエンスをレンダリングの図 ](/help/dev/patterns/recs-atjs/assets/diagram-render-experiences-new.png){width="600" zoomable="yes"}

次のリンクをクリックして、目的のセクションに移動します。

* [3.1：プロモーション](#promotion)
* [3.2：買い物かごベースの条件](#cart)
* [3.3：人気度ベースの条件](#popularity)
* [3.4：品目ベースの基準](#item)
* [3.5：ユーザーベースの条件](#user)
* [3.6：カスタム条件](#custom)
* [3.7：インクルージョンルールで使用する属性の提供](#inclusion)
* [3.8:excludedIds の指定](#exclude)
* [3.9:Recommendationsの商品カタログを更新するためのエンティティ属性を提供する](#entity-attributes)
* [3.10：包含ルールのキーとして使用するプロファイル属性を指定する](#keys)
* [3.11：ページ読み込みリクエストの実行](#fire)
* [3.12：地域のロケーションリクエストの実行](#location)

## 3.1：プロモーション {#promotion}

プロモーション対象の項目を追加し、アクティビティの作成時に [!DNL Target] UI で「前面」または「背面」プロモーションを選択して、Recommendations のデザインでの配置を制御します。

+++詳細を見る

**使用可能なオプション**

* ID で昇格
* [ コレクションで昇格 ](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html?lang=ja){target=_blank}
* [ 属性別に昇格 ](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=ja){target=_blank}

**必要なエンティティ パラメーター**

* 「属性で昇格」オプションを使用する場合は、プロモーションの項目属性を渡す必要があります。

**読み取り**

* [ プロモーションの追加 ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/adding-promotions.html?lang=ja){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 3.2：買い物かごベースの条件 {#cart}

ユーザーの買い物かごの中身に基づいてお勧めを紹介します。

+++詳細を見る

**利用可能な条件**

* [!UICONTROL People Who Viewed These, Viewed Those]
* [!UICONTROL People Who Viewed These, Bought Those]
* [!UICONTROL People Who Bought These, Bought Those]

**必要なエンティティ パラメーター**

* cartIds

**読み取り**

* [ 買い物かごベース ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=ja#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 3.3：人気度ベースの条件 {#popularity}

サイト全体でのアイテムの全体的な人気度に基づいて、または訪問者のお気に入りのカテゴリや最も多く閲覧されたカテゴリ、ブランド、ジャンルなどのアイテムの人気度に基づいて、レコメンデーションを作成します。

+++詳細を見る

**利用可能な条件**

* [!UICONTROL Most Viewed Across the Site]
* [!UICONTROL Most Viewed by Category]
* [!UICONTROL Most Viewed by Item Attribute]
* [!UICONTROL Top Sellers Across the Site]
* [!UICONTROL Top Sellers by Category]
* [!UICONTROL Top Sellers by Item Attribute]
* [!UICONTROL Top by Analytics Metric]

**必要なエンティティ パラメーター**

* 基準が現在の属性または項目属性に基づいている場合は、人気度の `entity.categoryId` または項目属性。
* サイト全体で最も多く閲覧/トップセールスに関しては何も渡さない

**読み取り**

* [ 人気度ベース ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=ja#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 3.4：品目ベースの基準 {#item}

ユーザーが表示している項目または最近表示した項目に類似した項目を見つけることに基づいてお勧めを紹介します。

+++詳細を見る

**利用可能な条件**

* [!UICONTROL People Who Viewed This, Viewed That]
* [!UICONTROL People Who Viewed This, Bought That]
* [!UICONTROL People Who Bought This, Bought That]
* [!UICONTROL Items with Similar Attributes]

**必要なエンティティ パラメーター**

* `entity.id`
* プロファイル属性がキーとして使用されている場合

**読み取り**

* [ 項目ベース ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=ja#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 3.5：ユーザーベースの条件 {#user}

ユーザーの行動に基づいてお勧めを紹介します。

+++詳細を見る

**利用可能な条件**

* [!UICONTROL Recently Viewed Items]
* [!UICONTROL Recommended for You]

**必要なエンティティ パラメーター**

* `entity.id`

**読み取り**

* [ ユーザーベース ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=ja#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 3.6：カスタム条件 {#custom}

アップロードしたカスタムファイルに基づいてお勧めを紹介します。

+++詳細を見る

**利用可能な条件**

* [!UICONTROL Custom algorithm]

**必要なエンティティ パラメーター**

`entity.id` または、カスタムアルゴリズムのキーとして使用される属性

**読み取り**

* [ カスタム条件 ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=ja#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 3.7：インクルージョンルールで使用する属性の提供 {#inclusion}

+++詳細を見る

**読み取り**

* [ 動的および静的インクルージョンルールの使用 ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html?lang=ja){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 3.8:excludedIds の指定 {#exclude}

レコメンデーションから除外したいエンティティのエンティティ ID を渡します。 例えば、既に買い物かごにある項目は除外した方がよいでしょう。

+++詳細を見る

**読み取り**

* [ エンティティを動的に除外することはできますか？](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=ja#exclude){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 3.9：製 [!DNL Recommendations] の製品カタログを更新するためのエンティティ属性を提供する {#entity-attributes}

+++詳細を見る

**読み取り**

* [ エンティティの属性 ](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=ja){target=_blank}

また、[!DNL Target] UI を使用して [ 製品フィード ](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html?lang=ja){target=_blank} を作成し、[!DNL Recommendations] の製品カタログを更新することで、この手順を実行することもできます。

+++

[このページの上部の図に戻ります。](#diagram)

## 3.10：包含ルールのキーとして使用するプロファイル属性を指定する {#keys}

前述のRecommendations条件でインクルージョンルールのキーとして使用されるプロファイル属性を指定します。

+++ 詳細を表示

**読み取り**

* [ プロファイル属性 ](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html?lang=ja){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 3.11：ページ読み込みリクエストの実行 {#fire}

この手順では、リクエストで `execute` / `pageLoad` ペイロードを含む [!DNL Delivery API] 呼び出しをトリガーにします。 `getOffers()` メソッドは、エクスペリエンスを取得し、ページ上 `applyOffers()` エクスペリエンスをレンダリングします。 [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=ja){target=_blank} （VEC）で作成されたエクスペリエンスをレンダリングするには、`pageLoad` リクエストが必要です。

+++詳細を見る

![ ページ読み込みリクエストのトリガー図 ](/help/dev/patterns/recs-atjs/assets/fire-page-load-request-combined.png){width="400" zoomable="yes"}

**前提条件**

* すべてのデータマッピングは、`targetPageParams` 関数を使用しておこなう必要があります。

**読み取り**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**アクション**

* `getOffers` メソッドと `applyOffers` メソッドを使用して、ページ読み込みリクエスト API 呼び出しを使用したエクスペリエンスを取得します。

+++

[このページの上部の図に戻ります。](#diagram)

## 3.12：地域のロケーションリクエストの実行（#location）

この手順では、リクエストに `execute` / `mboxes` ペイロードを含む [!DNL Delivery API] 呼び出しをトリガーにします。 `getOffers` メソッドは、エクスペリエンスを取得し、`applyOffers` でエクスペリエンスをページにレンダリングします。 `execute`/`mboxes` ペイロードで複数の mbox を送信できます。

+++詳細を見る

![ 火災の地域ロケーションリクエストの図 ](/help/dev/patterns/recs-atjs/assets/fire-regional-location-request-combined.png){width="400" zoomable="yes"}

**前提条件**

* すべてのデータマッピングは、`targetPageParams` 関数を使用しておこなう必要があります。

**読み取り**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**アクション**

* `getOffers` メソッドと `applyOffers` メソッドを使用して、ページ読み込みリクエスト API 呼び出しを使用したエクスペリエンスを取得します。

+++

[このページの上部の図に戻ります。](#diagram)

手順 4:[Notify Target](/help/dev/patterns/recs-atjs/notify-target.md) に進みます。
