---
title: エクスペリエンスをレンダリング
description: エクスペリエンスのレンダリングに必要なすべての手順が正しい順序で実行されていることを確認します。
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 85af1bad244dc0aa7665e2fbb627d82f6fabbf88
workflow-type: tm+mt
source-wordcount: '1080'
ht-degree: 7%

---

# エクスペリエンスをレンダリング

次の手順に従います： *エクスペリエンスをレンダリング* ダイアグラムを使用して、エクスペリエンスのレンダリングに必要なすべてのタスクが正しい順序で実行されていることを確認します。

>[!NOTE]
>
>ページの自動読み込み要求を [自動ページ読み込み要求ステップの設定](/help/dev/patterns/recs-atjs/initialize-sdk.md#automatic) in *SDK を初期化* を呼び出す場合を除き、Adobe Target SDK を呼び出して地域 — 場所のリクエストを使用して追加のエクスペリエンスをレンダリングする場合を除き、このアクティビティをスキップできます。

>[!TIP]
>
>このトピックの画像をクリックすると、全画面表示に展開されます。

## エクスペリエンス図をレンダリング {#diagram}

at.js で利用可能な、あらかじめ用意されている自動ちらつき処理は、 [!UICONTROL 自動ページ読み込み要求] 有効。 このオプションを選択すると、HTMLの本文全体が非表示になり、次のエクスペリエンスが取得されます。 [!DNL Target]. この場合、ちらつきを処理するのはユーザーの責任です。 ガイダンスについて、ちらつき処理に使用できる実装パターンを検索します。

次の図に示すステップ番号は、以下のセクションに対応しています。

![エクスペリエンス図をレンダリング](/help/dev/patterns/recs-atjs/assets/diagram-render-experiences-new.png){width="600" zoomable="yes"}

次のリンクをクリックして、目的のセクションに移動します。

* [3.1：昇格](#promotion)
* [3.2：買い物かごに基づく条件](#cart)
* [3.3：人気度に基づく条件](#popularity)
* [3.4：項目に基づく基準](#item)
* [3.5：ユーザーベースの条件](#user)
* [3.6：カスタム条件](#custom)
* [3.7：インクルージョンルールで使用する属性の提供](#inclusion)
* [3.8: excludedIds の提供](#exclude)
* [3.9: Recommendationsの製品カタログを更新するエンティティ属性を指定します](#entity-attributes)
* [3.10：インクルージョンルールのキーとして使用するプロファイル属性を提供します](#keys)
* [3.11：ページ読み込み要求の実行](#fire)
* [3.12：地域ロケーションリクエストの実行](#location)

## 3.1：昇格 {#promotion}

プロモーション項目の追加と Target Recommendationsでの配置の制御 [デザイン](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html){target=_blank}.

+++詳細を見る

**使用可能なオプション**

* ID 別に昇格
* [コレクション別に昇格](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [属性別にプロモート](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**エンティティのパラメーターが必要です**

* 「属性別にプロモート」オプションを使用する場合は、プロモーションの項目属性を渡す必要があります。

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.2：買い物かごに基づく条件 {#cart}

ユーザーの買い物かごの内容に基づいてレコメンデーションをおこないます。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL これらを閲覧した人がそれらを閲覧しました]
* [!UICONTROL これらを閲覧した人が購入したもの]
* [!UICONTROL これらを購入した人が購入したもの]

**エンティティのパラメーターが必要です**

* cartIds

**読み取り**

* [買い物かごベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.3：人気度に基づく条件 {#popularity}

サイト全体での品目の全体的な人気度に基づいて、または訪問者のお気に入りまたは最も多く閲覧されたカテゴリ、ブランド、ジャンルなど内の品目の人気度に基づいてレコメンデーションをおこないます。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL サイト全体で最も多く閲覧された]
* [!UICONTROL カテゴリ別の最も多く閲覧された項目]
* [!UICONTROL 品目属性別最も多く閲覧された品目]
* [!UICONTROL サイト全体のトップセラー]
* [!UICONTROL カテゴリ別のトップセラー]
* [!UICONTROL 品目属性別トップセラー]
* [!UICONTROL Analytics 指標別の上位]

**エンティティのパラメーターが必要です**

* `entity.categoryId` または、現在の属性または品目属性に基づく条件の場合は、人気度に基づく品目属性。
* サイト全体で最も多く閲覧された/販売されたトップに対して渡す必要はありません。

**読み取り**

* [人気度ベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.4：項目に基づく基準 {#item}

ユーザーが閲覧中または最近閲覧した品目に類似した品目を見つけることに基づいてレコメンデーションをおこないます。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL これを閲覧した人が他に閲覧したもの]
* [!UICONTROL これを閲覧した人が購入したもの]
* [!UICONTROL これを購入した人が他に購入したもの]
* [!UICONTROL 類似の属性を持つ品目]

**エンティティのパラメーターが必要です**

* `entity.id`
* いずれかのプロファイル属性がキーとして使用されている場合

**読み取り**

* [項目ベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.5：ユーザーベースの条件 {#user}

ユーザーの行動に基づいてレコメンデーションをおこなう。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL 最近表示された項目]
* [!UICONTROL お勧め]

**エンティティのパラメーターが必要です**

* `entity.id`

**読み取り**

* [ユーザーベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.6：カスタム条件 {#custom}

アップロードしたカスタムファイルに基づいてレコメンデーションをおこないます。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL カスタムアルゴリズム]

**エンティティのパラメーターが必要です**

`entity.id` またはカスタムアルゴリズムのキーとして使用される属性

**読み取り**

* [カスタム条件](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.7：インクルージョンルールで使用する属性の提供 {#inclusion}

+++詳細を見る

**読み取り**

* [動的および静的インクルージョンルールの使用](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.8: excludedIds の提供 {#exclude}

レコメンデーションから除外するエンティティのエンティティ ID を渡します。 例えば、既に買い物かごにある項目は除外した方がよいでしょう。

+++詳細を見る

**読み取り**

* [エンティティを動的に除外することはできますか？ ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.9：商品カタログを更新するエンティティ属性を指定します。 [!DNL Recommendations] {#entity-attributes}

+++詳細を見る

**読み取り**

* [エンティティの属性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

この手順は、 [製品フィード](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html){target=_blank} の使用 [!DNL Target] の商品カタログを更新するための UI [!DNL Recommendations].

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.10：インクルージョンルールのキーとして使用するプロファイル属性を提供します {#keys}

上記の任意のRecommendations条件のインクルージョンルールのキーとして使用するプロファイル属性を指定します。

+++ 詳細を見る

**読み取り**

* [プロファイル属性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.11：ページ読み込み要求の実行 {#fire}

この手順では、トリガーa [!DNL Delivery API] ～に電話する `execute` > `pageLoad` ペイロードを含める必要があります。 The `getOffers()` メソッドはエクスペリエンスを取得し、 `applyOffers()` は、ページ上のエクスペリエンスをレンダリングします。 The `pageLoad` リクエストは、 [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html){target=_blank} (VEC) を参照してください。

+++詳細を見る

![ページ読み込み要求の図を実行する](/help/dev/patterns/recs-atjs/assets/fire-page-load-request-combined.png){width="400" zoomable="yes"}

**前提条件**

* すべてのデータマッピングは、 `targetPageParams` 関数に置き換えます。

**読み取り**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**アクション**

* 以下を使用します。 `getOffers` および `applyOffers` ページ読み込み要求 API 呼び出しを使用してエクスペリエンスを取得するためのメソッド。

+++

[このページの上部にある図に戻ります。](#diagram)

## 3.12：地域ロケーションリクエストの実行 (#location)

この手順では、トリガーa [!DNL Delivery API] ～に電話する `execute` > `mboxes` ペイロードを含める必要があります。 The `getOffers` メソッドはエクスペリエンスを取得し、 `applyOffers` はエクスペリエンスをページにレンダリングします。 複数の mbox を `execute` > `mboxes` ペイロード。

+++詳細を見る

![地域の位置リクエスト図を実行します](/help/dev/patterns/recs-atjs/assets/fire-regional-location-request-combined.png){width="400" zoomable="yes"}

**前提条件**

* すべてのデータマッピングは、 `targetPageParams` 関数に置き換えます。

**読み取り**

* [adobe.target.getOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
* [adobe.target.applyOffers()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)

**アクション**

* 以下を使用します。 `getOffers` および `applyOffers` ページ読み込み要求 API 呼び出しを使用してエクスペリエンスを取得するためのメソッド。

+++

[このページの上部にある図に戻ります。](#diagram)