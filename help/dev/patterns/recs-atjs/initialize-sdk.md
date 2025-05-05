---
title: SDK の初期化
description: at [!DNL Adobe Target] js JavaScript ライブラリを読み込むために必要なすべての手順が、正しい順序で実行されていることを確認します。
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: 250a8382-1fdd-4a70-b712-a25af5adad71
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '1558'
ht-degree: 5%

---

# SDK の初期化

*SDK の初期化* の図に示す手順に従って、[!DNL Adobe Target] at.js JavaScript ライブラリの読み込みに必要なすべてのタスクが正しい順序で実行されていることを確認します。

>[!TIP]
>
>全画面表示するには、このトピックの画像をクリックします。

## SDK 図の初期化 {#diagram}

複数ページアプリケーションの場合、このフローは、ページがリロードされるか、訪問者が web サイト上の新しいページに移動するたびに発生します。

>[!NOTE]
>
>次の図のステップ番号は、以下のセクションに対応しています。 ステップ番号は特定の順序ではなく、アクティビティの作成時に [!DNL Target] UI で実行されるステップの順序は反映されません。

![SDK 図の初期化 ](/help/dev/patterns/recs-atjs/assets/diagram-initiaze-sdk.png){width="600" zoomable="yes"}

次のリンクをクリックして、目的のセクションに移動します。

* [1.1：訪問者 API SDK の読み込み](#load)
* [1.2：顧客 ID の設定](#set)
* [1.3：自動ページ読み込みリクエストの設定](#automatic)
* [1.4：ちらつき処理の設定](#flicker)
* [1.5：データマッピングの設定](#data-mapping)
* [1.6：プロモーション](#promotion)
* [1.7：買い物かごベースの条件](#cart)
* [1.8：人気に基づく基準](#popularity)
* [1.9：品目ベースの基準](#item)
* [1.10：ユーザーベースの条件](#user)
* [1.11：カスタム条件](#custom)
* [1.12：インクルージョンルールで使用する属性を指定する](#inclusion)
* [1.13:excludedIds の指定](#exclude)
* [1.14: entity.event.detailsOnly=true パラメーターを渡す](#true)
* [1.15: リモートデータマッピングの設定](#remote)
* [1.16: at.js の読み込み](#web)

## 1.1：訪問者 API SDK の読み込み {#load}

この手順は、`VisitorAPI.js` ライブラリが正しく読み込まれ、設定および初期化されていることを確認するのに役立ちます。

+++詳細を見る

![Visitor API SDK の読み込み図 ](/help/dev/patterns/recs-atjs/assets/load-visitor-combined.png){width="400" zoomable="yes"}

**前提条件**

* 訪問者 ID/API サービスを使用するには、[!DNL Adobe Experience Cloud] に対して会社が有効になっており、[!UICONTROL Organization ID] が付与されている必要があります。 詳しくは、&lbrace;ID サービスのヘルプ [&#128279;](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html?){target=_blank} ガイドの &lbrace;0 *Experience Cloud要件：組織 ID* を参照してください。
* `VisitorAPI.js` ファイルが必要です。 既に実装されている場合は、このファイルが既に存在しているはず [!DNL Adobe Analytics] す。 このファイルは、[[!DNL Adobe Experience Platform] tags extension](https://experienceleague.adobe.com/docs/tags.html){target=_blank} を使用して追加することも、[Adobe Analytics Code Manager](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-analytics.html){target=_blank} からダウンロードすることもできます。

**VisitorAPI.js の設定と参照**

詳しくは、[Target のExperience Cloudサービスを実装する ](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-target.html){target=_blank} を参照してください。

**読み取り**

* [Experience Cloud ID サービスの概要 ](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html){target=_blank}
* [ID サービスについて ](https://experienceleague.adobe.com/docs/id-service/using/intro/about-id-service.html){target=_blank}
* [Cookie とExperience CloudID サービス ](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html){target=_blank}
* [Experience CloudID サービスによる ID のリクエスト方法と設定方法 ](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html){target=_blank}
* [ID 同期と一致率について ](https://experienceleague.adobe.com/docs/id-service/using/intro/match-rates.html){target=_blank}

**アクション**

* `VisitorAPI.js` ファイルを Web ページに埋め込みます。
* 詳しくは、[ 訪問者 ID/API サービスで使用可能な設定 ](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html){target=_blank} を参照してください。
* `VisitorAPI.js` ファイルが読み込まれたら、`Visitor.getInstance` メソッドを使用して、必要な設定を使用して初期化します。
* [ 使用可能なメソッド ](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/get-set.html){target=_blank} を理解します。

+++

[このページの上部の図に戻ります。](#diagram)

## 1.2：顧客 ID の設定 {#set}

この手順は、訪問者の既知の ID （CRM ID、ユーザー ID など）を [!DNL Adobe] の匿名 ID に結び付けて、クロスデバイスパーソナライゼーションを確実に行うのに役立ちます。

+++詳細を見る

![ 顧客 ID を設定 ](/help/dev/patterns/recs-atjs/assets/set-customer-id-combined.png){width="400" zoomable="yes"}

**前提条件**

* 訪問者の既知の ID は、データレイヤーで利用できる必要があります。

**顧客 ID を設定**
詳しくは、「[setCustomerIDs](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html){target=_blank}」を参照してください。

**読み取り**

* [mbox3rdPartyId のリアルタイムプロファイル同期 ](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html){target=_blank}

**アクション**

* `visitor.setCustomerIDs` を使用して、訪問者の既知の ID を設定します。

+++

[このページの上部の図に戻ります。](#diagram)

## 1.3：自動ページ読み込みリクエストの設定 {#automatic}

この手順により、at.js は、at.js JavaScript ライブラリファイルの読み込み中に、ページ上でレンダリングする必要があるすべてのエクスペリエンスを取得できます。

+++詳細を見る

![ 自動ページ読み込みリクエストを設定 ](/help/dev/patterns/recs-atjs/assets/configure-automatic-page-request-combined.png){width="400" zoomable="yes"}

**前提条件**

* データレイヤー内のすべてのデータを [!DNL Target] に送信する必要があるわけではありません。 ビジネスチーム（デジタルマーケティングチーム）に問い合わせて、実験、最適化、パーソナライゼーションにとって価値のあるデータを判断します。 このデータのみを [!DNL Target] に送信する必要があります。
* 個人を特定できる情報（PII）のデータを [!DNL Target] に送信しないようにしてください。

**自動ページ読み込みリクエストを設定**

詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。

**読み取り**

[targetGlobalSettings （） ](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) の `pageLoadEnabled` 設定について説明します。

**アクション**

* `window.targetGlobalSettings` オブジェクトを変更して、自動ページ読み込みリクエストを有効にします。

+++

[このページの上部の図に戻ります。](#diagram)

## 1.4：ちらつき処理の設定 {#flicker}

この手順は、エクスペリエンスの配信時にページのちらつきを回避するのに役立ちます。

+++詳細を見る

![ ちらつき処理図の設定 ](/help/dev/patterns/recs-atjs/assets/flicker-handling-combined.png){width="400" zoomable="yes"}

**前提条件**

* at.js で使用されるデフォルトの方法を使用してちらつきの制御に関するメリットとデメリットについて、web ページのパフォーマンスを担当するチームと話し合います。 ローダーアニメーションなど、カスタムのちらつき処理ソリューションを使用できるデザインパターンを検索できます。 パターンが見つからない場合は、新しいパターンをリクエストできます。

**ちらつき処理の設定**

詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。

`bodyHidingEnabled` を `true` に設定すると、ページ読み込みリクエストの処理中に、ページ本文全体が非表示になります。 何らかの理由で自動ページ読み込みリクエストを有効にしていない場合（後でデータの準備が整っていない場合など）、この設定を `false` に設定することをお勧めします。

APLR を実行せずに後でページ リクエストを実行するために `bodyHidingEnabled` を無効にした場合、またはちらつき処理を必要としない場合は、独自のちらつき処理を実装する必要があります。 ちらつきの処理には、テスト中のセクションを非表示にする方法と、テスト中のセクションに絞り込みを表示する方法の 2 つがあります。

**読み取り**

* [at.js によるちらつきの制御方法](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
* [targetGlobalSettings （） ](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) の bodyHiddenStyle オブジェクトと bodyHidingEnabled オブジェクトについて説明します。

**アクション**

* `bodyHiddenStyle` および `bodyHidingEnabled` を設定するように `window.targetGlobalSettings` オブジェクトを修正します。

+++

[このページの上部の図に戻ります。](#diagram)

## 1.5：データマッピングの設定 {#data-mapping}

この手順は、[!DNL Target] に送信する必要があるすべてのデータが設定されていることを確認するのに役立ちます。

+++詳細を見る

![ データマッピング図 ](/help/dev/patterns/recs-atjs/assets/data-mapping-combined.png){width="400" zoomable="yes"}

**前提条件**

* データレイヤーには、[!DNL Target] に送信する必要のあるすべてのデータが準備されているはずです。
* Recommendations: プロファイルを強化します。
   * 最後に閲覧された製品に基づく条件に基づいて、最近閲覧された条件および項目のデータをキャプチャする `entity.id` を渡します。
   * お気に入りのカテゴリに基づいて、人気度条件のデータをキャプチャする `entity.id` を渡します。
   * カスタム条件がプロファイル属性に基づいている場合、または任意の条件の包含ルールフィルタリングで使用されている場合は、プロファイル属性を渡します。
* Recommendations：商品データの取り込み。
   * その他のエンティティパラメーター（予約済みおよびカスタム）を渡して、[!DNL Recommendations] で商品カタログを取得または更新できます。
   * 製品カタログは、[!DNL Target] UI または API を使用したエンティティフィードを使用して更新することもできます。

**データを[!DNL Target]** にマッピングする

詳しくは、[targetPageParams （） ](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) を参照してください。

**読み取り**

* [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)
* [Recommendations の計画と実装](/help/dev/implement/recommendations/recommendations.md)
* [Recommendations カタログの設定](/help/dev/implement/recommendations/recommendations.md)

**アクション**

* `targetPageParams()` 関数を使用して、[!DNL Target] に送信する必要のあるすべての必要なデータを設定します。

+++

[このページの上部の図に戻ります。](#diagram)

## 1.6：プロモーション {#promotion}

昇格された項目を追加し、[!DNL Target Recommendations] ージでの配置を制御します [ デザイン ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html){target=_blank}。

+++詳細を見る

**使用可能なオプション**

* ID で昇格
* [ コレクションで昇格 ](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [ 属性別に昇格 ](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**必要なエンティティ パラメーター**

* 「属性で昇格」オプションを使用する場合は、昇格時に項目属性を渡す必要があります。

+++

[このページの上部の図に戻ります。](#diagram)

## 1.7：買い物かごベースの条件 {#cart}

ユーザーの買い物かごの中身に基づいてお勧めを紹介します。

+++詳細を見る

**利用可能な条件**

* [!UICONTROL People Who Viewed These, Viewed Those]
* [!UICONTROL People Who Viewed These, Bought Those]
* [!UICONTROL People Who Bought These, Bought Those]

**必要なエンティティ パラメーター**

* cartIds

**読み取り**

* [ 買い物かごベース ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 1.8：人気に基づく基準 {#popularity}

サイト全体でのアイテムの全体的な人気度に基づいて、またはユーザーのお気に入りのカテゴリや最も多く閲覧されたカテゴリ、ブランド、ジャンルなどのアイテムの人気度に基づいて、レコメンデーションを作成します。

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

* 基準が現在の項目または項目属性に基づいている場合は、人気度の `entity.categoryId` または項目属性。
* サイト全体で最も多く閲覧/トップの販売に対して何も渡さない

**読み取り**

* [ 人気度ベース ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 1.9：品目ベースの基準 {#item}

ユーザーが表示している項目または最近表示した項目に類似した項目を見つけることに基づいてお勧めを紹介します。

+++詳細を見る

**利用可能な条件**

* [!UICONTROL People Who Viewed This, Viewed That]
* [!UICONTROL People Who Viewed This, Bought That]
* [!UICONTROL People Who Bought This, Bought That]
* [!UICONTROL Items with Similar Attributes]

**必要なエンティティ パラメーター**

* キーとして使用される `entity.id` または任意のプロファイル属性

**読み取り**

* [ 項目ベース ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 1.10：ユーザーベースの条件 {#user}

ユーザーの行動に基づいてお勧めを紹介します。

+++詳細を見る

**利用可能な条件**

* [!UICONTROL Recently Viewed Items]
* [!UICONTROL Recommended for You]

**必要なエンティティ パラメーター**

* `entity.id`

**読み取り**

* [ ユーザーベース ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 1.11：カスタム条件 {#custom}

アップロードしたカスタムファイルに基づいてお勧めを紹介します。

+++詳細を見る

**利用可能な条件**

* [!UICONTROL Custom algorithm]

**必要なエンティティ パラメーター**

`entity.id` または、カスタムアルゴリズムのキーとして使用される属性

**読み取り**

* [ カスタム条件 ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 1.12：インクルージョンルールで使用する属性を指定する {#inclusion}

+++詳細を見る

**読み取り**

* [ 動的および静的インクルージョンルールの使用 ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 1.13:excludedIds の指定 {#exclude}

レコメンデーションから除外したいエンティティのエンティティ ID を渡します。 例えば、既に買い物かごにある項目は除外した方がよいでしょう。

+++詳細を見る

**読み取り**

* [ エンティティを動的に除外することはできますか？](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 1.14: `entity.event.detailsOnly=true` パラメーターを渡す {#true}

エンティティ属性を使用して、製品またはコンテンツの情報を [!DNL Target Recommendations] に渡します。

+++詳細を見る

**読み取り**

* [ エンティティの属性 ](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=en){target=_blank}

+++

[このページの上部の図に戻ります。](#diagram)

## 1.15: リモートデータマッピングの設定（remote）

この手順により、[!DNL Target] に送信する必要があるすべてのデータが設定されます。

+++詳細を見る

![ リモートデータマッピングの図 ](/help/dev/patterns/recs-atjs/assets/remote-data-mapping-combined.png){width="400" zoomable="yes"}

**前提条件**

* データレイヤーは、[!DNL Target] に送信する必要があるすべてのデータで準備が整っている必要があります。

**データプロバイダーの設定**

詳しくは、[ データプロバイダー ](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers) を参照してください。

**読み取り**

[targetPageParams 関数](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**アクション**

`targetPageParams()` 関数を使用して、[!DNL Target] に送信する必要のあるすべての必要なデータを設定します。

+++

[このページの上部の図に戻ります。](#diagram)

## 1.16: at.js の読み込み {#web}

この手順では、at.js JavaScript ライブラリが読み込まれ、初期化されるようにします。

+++詳細を見る

![Adobe Target at.js の読み込み図 ](/help/dev/patterns/recs-atjs/assets/load-atjs-combined.png){width="400" zoomable="yes"}

**前提条件**

* `at.js 2.*x*` JavaScript ライブラリファイルをダウンロードするか、デジタルマーケティングチームに問い合わせます。

*読み取り*

* [Target の仕組み ](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html){target=_blank}
* [at.js の仕組み](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
* [タグマネージャーを使用しない Target の実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

**アクション**

at.js ファイルは、実験、最適化、パーソナライゼーション、データ収集が行われる必要があるすべての web ページに埋め込みます。

+++

[このページの上部の図に戻ります。](#diagram)

手順 2:[ データ収集の設定 ](/help/dev/patterns/recs-atjs/data-collection.md) に進みます。
