---
title: SDKの初期化
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリを読み込むのに必要なすべての手順が、正しい順序で実行されていることを確認します。'
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 250a8382-1fdd-4a70-b712-a25af5adad71
TQID: https://experienceleague.adobe.com/PxAKvxntUCdacBLopvANAI7-8OWe-ELQqFRJu-n3RWo
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: adee20bd-51f4-461d-b9db-d215f8756eebid: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: bcc5edb5-84c3-4940-9f84-ed88b6c16274id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: d3cdead0-685a-4489-9250-4bb709942f66id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1797
ht-degree: 5%

---

# SDKの初期化

*SDKの初期化*&#x200B;図の手順に従って、[!DNL Adobe Target] at.js JavaScript ライブラリの読み込みに必要なすべてのタスクが正しい順序で実行されるようにします。

>[!TIP]
>
>このトピックの画像をクリックして、フルスクリーンに展開します。

## SDK ダイアグラムの初期化 {#diagram}

複数ページアプリケーションの場合、このフローは、ページが再読み込みされるか、訪問者がweb サイト上の新しいページに移動するたびに発生します。

>[!NOTE]
>
>次の図の手順の番号は、以下の節に対応しています。 ステップ番号は特定の順序ではなく、アクティビティの作成中に[!DNL Target] UIで実行されたステップの順序は反映されません。

![SDK ダイアグラムの初期化](/help/dev/patterns/recs-atjs/assets/diagram-initiaze-sdk.png){width="600" zoomable="yes"}

次のリンクをクリックして、目的のセクションに移動します。

* [1.1：訪問者API SDKの読み込み](#load)
* [1.2：顧客IDの設定](#set)
* [1.3：自動ページ読み込みリクエストの設定](#automatic)
* [1.4：ちらつき処理の設定](#flicker)
* [1.5: データマッピングの設定](#data-mapping)
* [1.6: プロモーション](#promotion)
* [1.7：カートベースの基準](#cart)
* [1.8：人気ベースの基準](#popularity)
* [1.9：アイテムベースの基準](#item)
* [1.10: ユーザーベースの基準](#user)
* [1.11: カスタム基準](#custom)
* [1.12：包含ルールで使用される属性を指定する](#inclusion)
* [1.13：除外Idを指定する](#exclude)
* [1.14: entity.event.detailsOnly=true パラメーターを渡します](#true)
* [1.15: リモートデータマッピングの設定](#remote)
* [1.16: at.jsの読み込み](#web)

## 1.1：訪問者API SDKの読み込み {#load}

この手順は、`VisitorAPI.js` ライブラリが正しく読み込まれ、設定され、初期化されていることを確認するのに役立ちます。

+++詳細を見る

![訪問者API SDK ダイアグラムの読み込み](/help/dev/patterns/recs-atjs/assets/load-visitor-combined.png){width="400" zoomable="yes"}

**前提条件**

* 訪問者ID/API サービスを使用するには、会社が[!DNL Adobe Experience Cloud]に対して有効になっており、[!UICONTROL Organization ID]が設定されている必要があります。 詳しくは、*Identity Service ヘルプ* ガイドの「[Experience Cloud要件：組織ID](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html?){target=_blank}」を参照してください。
* `VisitorAPI.js` ファイルが必要です。 [!DNL Adobe Analytics]が実装されている場合は、このファイルを既に用意しておく必要があります。 このファイルは、[[!DNL Adobe Experience Platform] tags拡張機能](https://experienceleague.adobe.com/docs/tags.html){target=_blank}を通じて追加することも、[Adobe Analytics Code Manager](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-analytics.html){target=_blank}からダウンロードすることもできます。

**VisitorAPI.jsの設定と参照**

詳しくは、[Target用Experience Cloud サービスの実装](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-target.html){target=_blank}を参照してください。

**読み取り**

* [Experience Cloud Identity Serviceの概要](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html){target=_blank}
* [ID サービスについて](https://experienceleague.adobe.com/docs/id-service/using/intro/about-id-service.html){target=_blank}
* [Cookies および Experience Cloud ID サービス](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html){target=_blank}
* [Experience Cloud Identity ServiceがIDをリクエストおよび設定する方法](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html){target=_blank}
* [IDの同期と一致率について](https://experienceleague.adobe.com/docs/id-service/using/intro/match-rates.html){target=_blank}

**アクション**

* Web ページに`VisitorAPI.js` ファイルを埋め込みます。
* 訪問者ID/API サービス ](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html){target=_blank}で使用できる[構成について説明します。
* `VisitorAPI.js` ファイルが読み込まれたら、`Visitor.getInstance` メソッドを使用して、必要な構成を使用して初期化します。
* [使用可能なメソッド ](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/get-set.html){target=_blank}について説明します。

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.2：顧客IDの設定 {#set}

この手順は、訪問者の既知のID （CRM ID、ユーザーIDなど）をクロスデバイスのパーソナライゼーション用に[!DNL Adobe]の匿名IDに関連付けるのに役立ちます。

+++詳細を見る

![顧客IDを設定](/help/dev/patterns/recs-atjs/assets/set-customer-id-combined.png){width="400" zoomable="yes"}

**前提条件**

* 訪問者の既知のIDは、データレイヤーで利用できる必要があります。

**顧客IDを設定**
詳しくは、[setCustomerIDs](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html){target=_blank}を参照してください。

**読み取り**

* [mbox3rdPartyId のリアルタイムプロファイル同期](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html){target=_blank}

**アクション**

* `visitor.setCustomerIDs`を使用して、訪問者の既知のIDを設定します。

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.3：自動ページ読み込みリクエストの設定 {#automatic}

この手順により、at.jsは、at.js JavaScript ライブラリファイルの読み込み中にページ上でレンダリングする必要があるすべてのエクスペリエンスを取得できます。

+++詳細を見る

![自動ページ読み込み要求の設定](/help/dev/patterns/recs-atjs/assets/configure-automatic-page-request-combined.png){width="400" zoomable="yes"}

**前提条件**

* データ レイヤー内のすべてのデータを[!DNL Target]に送信する必要はありません。 ビジネスチーム（デジタルマーケティングチーム）と相談して、どのデータが実験、最適化、パーソナライゼーションに役立つかを判断してください。 このデータのみを[!DNL Target]に送信する必要があります。
* 個人を特定できる情報（PII） データを[!DNL Target]に送信しないでください。

**自動ページ読み込み要求の設定**

詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。

**読み取り**

[targetGlobalSettings （） ](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)の`pageLoadEnabled`設定について説明します。

**アクション**

* 自動ページ読み込み要求を有効にするには、`window.targetGlobalSettings` オブジェクトを変更します。

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.4：ちらつき処理の設定 {#flicker}

このステップは、エクスペリエンスの配信時にページのちらつきが発生しないようにするのに役立ちます。

+++詳細を見る

![ちらつき処理図の設定](/help/dev/patterns/recs-atjs/assets/flicker-handling-combined.png){width="400" zoomable="yes"}

**前提条件**

* at.jsで使用されるデフォルトのメソッドを使用してフリッカーを制御する長所と短所について、web ページのパフォーマンスを担当するチームと話し合います。 ローダーアニメーションなどのカスタムフリッカー処理ソリューションを使用できるデザインパターンを検索できます。 パターンが見つからない場合は、新しいパターンをリクエストできます。

**ちらつき処理の設定**

詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。

`bodyHidingEnabled`を`true`に設定すると、ページ読み込み要求の処理中にページ本文全体が非表示になります。 何らかの理由で自動ページ読み込み要求を有効にしていない場合（データの準備が整っていない場合など）、この設定を`false`に設定することをお勧めします。

APLRを実行したくなく、後でページリクエストを実行する必要がある場合、またはフリッカー処理が不要な場合は、独自のフリッカー処理を実装する必要があります。 `bodyHidingEnabled`フリッカーは、テスト中のセクションを非表示にするか、テスト中のセクションにスロバーを表示することで、2つの方法で処理できます。

**読み取り**

* [at.js によるちらつきの制御方法](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
* [targetGlobalSettings （） ](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)のbodyHiddenStyle オブジェクトとbodyHidingEnabled オブジェクトについて説明します。

**アクション**

* `window.targetGlobalSettings` オブジェクトを変更して`bodyHiddenStyle`と`bodyHidingEnabled`を設定します。

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.5: データマッピングの設定 {#data-mapping}

この手順を実行すると、[!DNL Target]に送信する必要があるすべてのデータが設定されます。

+++詳細を見る

![ データマッピング図](/help/dev/patterns/recs-atjs/assets/data-mapping-combined.png){width="400" zoomable="yes"}

**前提条件**

* データ レイヤーは、[!DNL Target]に送信する必要があるすべてのデータで準備できなければなりません。
* 推奨事項：プロファイルの強化。
   * `entity.id`を渡すと、最近閲覧した商品に基づいて、最近閲覧した基準と項目のデータが取得されます。
   * `entity.id`を渡して、お気に入りのカテゴリに基づく人気条件のデータを取得します。
   * カスタム基準が基準に基づいている場合、または任意の基準での包含ルールのフィルタリングで使用される場合は、プロファイル属性を渡します。
* 推奨事項：商品データを取り込みます。
   * その他のエンティティ パラメーター（予約済みおよびカスタム）を渡して、製品カタログを[!DNL Recommendations]に取り込むか更新できます。
   * 製品カタログは、[!DNL Target] UIまたはAPIを使用してエンティティフィードを使用して更新することもできます。

**データを[!DNL Target]**&#x200B;にマッピング

詳しくは、[targetPageParams （） ](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)を参照してください。

**読み取り**

* [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)
* [レコメンデーションの計画と実装](/help/dev/implement/recommendations/recommendations.md)
* [レコメンデーションカタログの設定](/help/dev/implement/recommendations/recommendations.md)

**アクション**

* `targetPageParams()`関数を使用して、[!DNL Target]に送信する必要があるすべての必須データを設定します。

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.6: プロモーション {#promotion}

[!DNL Target Recommendations] [ デザイン ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html){target=_blank}で、プロモーションされたアイテムを追加し、その配置を制御します。

+++詳細を見る

**使用可能なオプション**

* IDによるプロモーション
* [コレクションで宣伝](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [属性で昇格](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**エンティティ パラメーターが必要**

* 「属性で昇格」オプションを使用する場合は、プロモーションの項目属性を渡す必要があります。

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.7：カートベースの基準 {#cart}

ユーザーのカートの内容に基づいてレコメンデーションを行います。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL People Who Viewed These, Viewed Those]
* [!UICONTROL People Who Viewed These, Bought Those]
* [!UICONTROL People Who Bought These, Bought Those]

**エンティティ パラメーターが必要**

* cartIds

**読み取り**

* [カートベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.8：人気ベースの基準 {#popularity}

サイト全体でのアイテムの人気度や、ユーザーが好むカテゴリーや最も閲覧されたカテゴリー、ブランド、ジャンルなどの中でのアイテムの人気度にもとづいて、レコメンデーションを行うことができます。

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

* 条件が現在の項目または項目属性に基づいている場合は、人気の項目属性`entity.categoryId`または項目属性。
* サイト全体で最も閲覧されたページや上位の販売数に対しては、何も渡す必要はありません。

**読み取り**

* [人気ベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.9：アイテムベースの基準 {#item}

利用者が閲覧中の項目や最近閲覧した項目と類似する項目を見つけることで、レコメンデーションを行うことができます。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL People Who Viewed This, Viewed That]
* [!UICONTROL People Who Viewed This, Bought That]
* [!UICONTROL People Who Bought This, Bought That]
* [!UICONTROL Items with Similar Attributes]

**エンティティ パラメーターが必要**

* `entity.id`またはキーとして使用されるプロファイル属性

**読み取り**

* [アイテムベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.10: ユーザーベースの基準 {#user}

利用者の行動にもとづいてレコメンデーションする：

+++詳細を見る

**使用可能な条件**

* [!UICONTROL Recently Viewed Items]
* [!UICONTROL Recommended for You]

**エンティティ パラメーターが必要**

* `entity.id`

**読み取り**

* [ユーザーベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.11: カスタム基準 {#custom}

アップロードしたカスタムファイルにもとづいて、レコメンデーションを作成できます。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL Custom algorithm]

**エンティティ パラメーターが必要**

`entity.id`またはカスタムアルゴリズムのキーとして使用される属性

**読み取り**

* [カスタム条件](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.12：包含ルールで使用される属性を指定する {#inclusion}

+++詳細を見る

**読み取り**

* [動的および静的インクルージョンルールの使用](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.13：除外Idを指定する {#exclude}

レコメンデーションから除外するエンティティのエンティティ IDを渡します。 例えば、既に買い物かごにある項目は除外した方がよいでしょう。

+++詳細を見る

**読み取り**

* [エンティティを動的に除外できますか。](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.14: `entity.event.detailsOnly=true` パラメーターを渡します {#true}

エンティティ属性を使用して、製品情報またはコンテンツ情報を [!DNL Target Recommendations] に渡します。

+++詳細を見る

**読み取り**

* [エンティティの属性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=en){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.15: リモートデータマッピングの設定（リモート）

この手順により、[!DNL Target]に送信する必要のあるすべてのデータが設定されます。

+++詳細を見る

![ リモートデータマッピング図](/help/dev/patterns/recs-atjs/assets/remote-data-mapping-combined.png){width="400" zoomable="yes"}

**前提条件**

* データ層は、[!DNL Target]に送信する必要があるすべてのデータで準備できなければなりません。

**データプロバイダーの設定**

詳しくは、[ データプロバイダー](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)を参照してください。

**読み取り**

[targetPageParams関数](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**アクション**

`targetPageParams()`関数を使用して、[!DNL Target]に送信する必要があるすべての必須データを設定します。

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.16: at.jsの読み込み {#web}

この手順により、at.js JavaScript ライブラリが読み込まれ、初期化されます。

+++詳細を見る

![Adobe Target at.js ダイアグラムを読み込む](/help/dev/patterns/recs-atjs/assets/load-atjs-combined.png){width="400" zoomable="yes"}

**前提条件**

* `at.js 2.*x*` JavaScript ライブラリファイルをダウンロードするか、デジタルマーケティング部門に問い合わせます。

*読み取り*

* [Target の仕組み](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html){target=_blank}
* [at.js の仕組み](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
* [タグマネージャーを使用しない Target の実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

**アクション**

テスト、最適化、パーソナライゼーション、データ収集が必要なあらゆるweb ページにat.js ファイルを埋め込むことができます。

+++

[このページの上部にある図に戻ります。](#diagram)

手順2: [ データ収集の設定](/help/dev/patterns/recs-atjs/data-collection.md)に進みます。
