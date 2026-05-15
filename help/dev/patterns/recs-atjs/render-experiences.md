---
title: エクスペリエンスをレンダリング
description: エクスペリエンスのレンダリングに必要なすべての手順が正しい順序で実行されていることを確認します。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 7cf0c70b-a4bc-46f4-9b33-099bdb7dd9a9
TQID: https://experienceleague.adobe.com/uHFbc8JEhjjGYIulJUvhkH7cXXht6Rht9rY43HjuNqg
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1060
ht-degree: 3%

---

# エクスペリエンスをレンダリング

*レンダーエクスペリエンス*&#x200B;図の手順に従って、エクスペリエンスのレンダリングに必要なすべてのタスクが正しい順序で実行されるようにします。

>[!NOTE]
>
>*Initialize SDKS*&#x200B;の[自動ページ読み込み要求の設定手順](/help/dev/patterns/recs-atjs/initialize-sdk.md#automatic)中に自動ページ読み込み要求を有効にした場合、Adobe Target SDKを呼び出して地域の位置情報リクエストを使用して追加のエクスペリエンスをレンダリングしない限り、このアクティビティをスキップできます。

>[!TIP]
>
>このトピックの画像をクリックして、フルスクリーンに展開します。

## エクスペリエンスのレンダリング図 {#diagram}

at.jsで利用できる、すぐに使える自動フリッカー処理は、[!UICONTROL Automatic Page Load Request]が有効になっている場合にのみ有効です。 このオプションは、[!DNL Target]からエクスペリエンスを取得する際に、HTMLの本文全体を非表示にします。 この場合、フリッカーを処理するのは自分の責任です。 ガイダンスのちらつき処理に使用できる実装パターンを検索します。

>[!NOTE]
>
>次の図の手順の番号は、以下の節に対応しています。 ステップ番号は特定の順序ではなく、アクティビティの作成中に[!DNL Target] UIで実行されたステップの順序は反映されません。

![&#x200B; エクスペリエンス図をレンダリング &#x200B;](/help/dev/patterns/recs-atjs/assets/diagram-render-experiences-new.png){width="600" zoomable="yes"}

次のリンクをクリックして、目的のセクションに移動します。

* [3.1: プロモーション](#promotion)
* [3.2：カートベースの基準](#cart)
* [3.3：人気ベースの基準](#popularity)
* [3.4：アイテムベースの基準](#item)
* [3.5: ユーザーベースの基準](#user)
* [3.6: カスタム基準](#custom)
* [3.7：包含ルールで使用される属性を指定する](#inclusion)
* [3.8:excludedIdを指定する](#exclude)
* [3.9: Recommendationsの製品カタログを更新するためのエンティティ属性を指定します](#entity-attributes)
* [3.10：包含ルールのキーとして使用されるプロファイル属性を提供する](#keys)
* [3.11: ページ読み込みリクエストを実行する](#fire)
* [3.12：地域の位置情報のリクエストを送信](#location)

## 3.1: プロモーション {#promotion}

アクティビティの作成中に[!DNL Target] UIで「前面」または「背面」のプロモーションを選択して、プロモーションされたアイテムを追加し、レコメンデーションデザインでの配置を制御します。

+++詳細を見る

**使用可能なオプション**

* IDによるプロモーション
* [コレクションで宣伝](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html?lang=ja){target=_blank}
* [属性で昇格](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=ja){target=_blank}

**エンティティ パラメーターが必要**

* 「属性で昇格」オプションを使用する場合は、プロモーションの項目属性を渡す必要があります。

**読み取り**

* [プロモーションの追加](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/adding-promotions.html?lang=ja){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.2：カートベースの基準 {#cart}

ユーザーのカートの内容に基づいてレコメンデーションを行います。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL People Who Viewed These, Viewed Those]
* [!UICONTROL People Who Viewed These, Bought Those]
* [!UICONTROL People Who Bought These, Bought Those]

**エンティティ パラメーターが必要**

* cartIds

**読み取り**

* [カートベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=ja#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.3：人気ベースの基準 {#popularity}

サイト全体でのアイテムの人気度や、訪問者が好むカテゴリーや最も閲覧したカテゴリー、ブランド、ジャンルなどの中でのアイテムの人気度にもとづいて、レコメンデーションを作成できます。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL Most Viewed Across the Site]
* [!UICONTROL Most Viewed by Category]
* [!UICONTROL Most Viewed by Item Attribute]
* [!UICONTROL Top Sellers Across the Site]
* [!UICONTROL Top Sellers by Category]
* [!UICONTROL Top Sellers by Item Attribute]
* [!UICONTROL Top by Analytics Metric]

**エンティティ パラメーターが必要**

* 条件が現在またはitem属性に基づいている場合は、人気のitem属性またはitem属性に基づく。`entity.categoryId`
* サイト全体で閲覧数が最も多い/売れた上位に対しては、何も渡す必要はありません。

**読み取り**

* [人気ベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=ja#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.4：アイテムベースの基準 {#item}

利用者が閲覧中の項目や最近閲覧した項目と類似する項目を見つけることで、レコメンデーションを行うことができます。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL People Who Viewed This, Viewed That]
* [!UICONTROL People Who Viewed This, Bought That]
* [!UICONTROL People Who Bought This, Bought That]
* [!UICONTROL Items with Similar Attributes]

**エンティティ パラメーターが必要**

* `entity.id`
* 任意のプロファイル属性がキーとして使用される場合

**読み取り**

* [アイテムベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=ja#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.5: ユーザーベースの基準 {#user}

利用者の行動にもとづいてレコメンデーションする：

+++詳細を見る

**使用可能な条件**

* [!UICONTROL Recently Viewed Items]
* [!UICONTROL Recommended for You]

**エンティティ パラメーターが必要**

* `entity.id`

**読み取り**

* [ユーザーベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=ja#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.6: カスタム基準 {#custom}

アップロードしたカスタムファイルにもとづいて、レコメンデーションを作成できます。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL Custom algorithm]

**エンティティ パラメーターが必要**

`entity.id`またはカスタムアルゴリズムのキーとして使用される属性

**読み取り**

* [カスタム条件](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=ja#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.7：包含ルールで使用される属性を指定する {#inclusion}

+++詳細を見る

**読み取り**

* [動的および静的インクルージョンルールの使用](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html?lang=ja){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.8:excludedIdを指定する {#exclude}

レコメンデーションから除外するエンティティのエンティティ IDを渡します。 例えば、既に買い物かごにある項目は除外した方がよいでしょう。

+++詳細を見る

**読み取り**

* [エンティティを動的に除外できますか。](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=ja#exclude){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.9: [!DNL Recommendations]の製品カタログを更新するためのエンティティ属性を指定します {#entity-attributes}

+++詳細を見る

**読み取り**

* [エンティティの属性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=ja){target=_blank}

この手順を実行するには、[!DNL Target] UIを使用して[製品フィード &#x200B;](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html?lang=ja){target=_blank}を作成し、[!DNL Recommendations]の製品カタログを更新します。

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.10：包含ルールのキーとして使用されるプロファイル属性を提供する {#keys}

上記の推奨事項の条件で、包含ルールのキーとして使用されるプロファイル属性を指定します。

+++ 詳細を見る

**読み取り**

* [プロファイル属性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html?lang=ja){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.11: ページ読み込みリクエストを実行する {#fire}

この手順では、リクエストで`execute` > `pageLoad` ペイロードを含む[!DNL Delivery API]呼び出しをトリガーします。 `getOffers()` メソッドはエクスペリエンスを取得し、`applyOffers()`はページ上のエクスペリエンスをレンダリングします。 `pageLoad` リクエストは、[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=ja){target=_blank} （VEC）で作成されたエクスペリエンスのレンダリングに必要です。

+++詳細を見る

![&#x200B; ページ読み込みリクエスト図を作成](/help/dev/patterns/recs-atjs/assets/fire-page-load-request-combined.png){width="400" zoomable="yes"}

**前提条件**

* すべてのデータマッピングは、`targetPageParams`関数を使用して実行する必要があります。

**読み取り**

* [adobe.target.getOffers （）](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers （）](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**アクション**

* `getOffers`および`applyOffers` メソッドを使用して、ページ読み込み要求API呼び出しを使用してエクスペリエンスを取得します。

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.12：火災地域の位置情報の要求（#location）

この手順では、リクエストに`execute` > `mboxes` ペイロードを含む[!DNL Delivery API]呼び出しをトリガーします。 `getOffers` メソッドはエクスペリエンスを取得し、`applyOffers`はエクスペリエンスをページにレンダリングします。 `execute` > `mboxes` ペイロードの下に複数のmboxを送信できます。

+++詳細を見る

![地域の場所のリクエスト図を作成](/help/dev/patterns/recs-atjs/assets/fire-regional-location-request-combined.png){width="400" zoomable="yes"}

**前提条件**

* すべてのデータマッピングは、`targetPageParams`関数を使用して実行する必要があります。

**読み取り**

* [adobe.target.getOffers （）](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers （）](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**アクション**

* `getOffers`および`applyOffers` メソッドを使用して、ページ読み込み要求API呼び出しを使用してエクスペリエンスを取得します。

+++

[このページの上部にある図に戻ります。](#diagram)

手順4に進みます：[&#x200B; ターゲットに通知](/help/dev/patterns/recs-atjs/notify-target.md)。
