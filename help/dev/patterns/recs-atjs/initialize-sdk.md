---
title: SDK の初期化
description: の読み込みに必要なすべての手順を確認します。 [!DNL Adobe Target] at.js JavaScript ライブラリは、正しい順序で実行されます。
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 266a8670a906e5be70f11bb05301b708f61a57d6
workflow-type: tm+mt
source-wordcount: '1814'
ht-degree: 7%

---

# SDK の初期化

次の手順に従います： *SDK を初期化* の読み込みに必要なすべてのタスクを確認する図 [!DNL Adobe Target] at.js JavaScript ライブラリは、正しい順序で実行されます。

>[!TIP]
>
>このトピックの画像をクリックすると、全画面表示に展開されます。

## SDK ダイアグラムの初期化 {#diagram}

複数ページのアプリの場合、このフローは、ページがリロードされるたびに、または訪問者が Web サイトの新しいページに移動するたびに発生します。

>[!NOTE]
>
>次の図に示すステップ番号は、以下のセクションに対応しています。 ステップ番号は特定の順序ではなく、 [!DNL Target] アクティビティの作成中に UI が表示されます。

![SDK ダイアグラムの初期化](/help/dev/patterns/recs-atjs/assets/diagram-initiaze-sdk.png){width="600" zoomable="yes"}

次のリンクをクリックして、目的のセクションに移動します。

* [1.1：訪問者 API SDK を読み込む](#load)
* [1.2：顧客 ID の設定](#set)
* [1.3：自動ページ読み込みリクエストの設定](#automatic)
* [1.4：ちらつきの処理を設定する](#flicker)
* [1.5：データマッピングの設定](#data-mapping)
* [1.6：昇格](#promotion)
* [1.7：買い物かごに基づく条件](#cart)
* [1.8：人気度ベースの条件](#popularity)
* [1.9：項目ベースの条件](#item)
* [1.10：ユーザーベースの条件](#user)
* [1.11：カスタム条件](#custom)
* [1.12：インクルージョンルールで使用する属性を提供します。](#inclusion)
* [1.13: excludedIds の提供](#exclude)
* [1.14: entity.event.detailsOnly=true パラメーターを渡します。](#true)
* [1.15：リモートデータマッピングの設定](#remote)
* [1.16: at.js を読み込む](#web)

## 1.1：訪問者 API SDK を読み込む {#load}

この手順は、 `VisitorAPI.js` ライブラリが正しく読み込まれ、設定され、初期化されている。

+++詳細を見る

![訪問者 API SDK の図を読み込み](/help/dev/patterns/recs-atjs/assets/load-visitor-combined.png){width="400" zoomable="yes"}

**前提条件**

* 訪問者 ID/API サービスを使用するには、会社で [!DNL Adobe Experience Cloud] また、 [!UICONTROL 組織 ID]. 詳しくは、 [Experience Cloud要件：組織 ID](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html?){target=_blank} （内） *ID サービスヘルプ* ガイド。
* 必要なのは `VisitorAPI.js` ファイル。 次の場合は、既にこのファイルが存在するはずです。 [!DNL Adobe Analytics] 実装済み。 このファイルは、 [[!DNL Adobe Experience Platform] タグ拡張子](https://experienceleague.adobe.com/docs/tags.html){target=_blank} or can be downloaded from the [Adobe Analytics Code Manager](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-analytics.html){target=_blank}.

**VisitorAPI.js を設定および参照してください。**

詳しくは、 [TargetExperience Cloudサービスの実装](https://experienceleague.adobe.com/docs/id-service/using/implementation/setup-target.html){target=_blank}.

**読み取り**

* [Experience CloudID サービスの概要](https://experienceleague.adobe.com/docs/id-service/using/intro/overview.html){target=_blank}
* [ID サービスについて](https://experienceleague.adobe.com/docs/id-service/using/intro/about-id-service.html){target=_blank}
* [Cookies および Experience Cloud ID サービス](https://experienceleague.adobe.com/docs/id-service/using/intro/cookies.html){target=_blank}
* [Experience CloudID サービスによる ID のリクエスト方法と設定方法](https://experienceleague.adobe.com/docs/id-service/using/intro/id-request.html){target=_blank}
* [ID 同期と一致率について](https://experienceleague.adobe.com/docs/id-service/using/intro/match-rates.html){target=_blank}

**アクション**

* 埋め込む `VisitorAPI.js` ファイルを Web ページに貼り付けます。
* 詳しくは、 [訪問者 ID/API サービスで利用可能な設定](https://experienceleague.adobe.com/docs/id-service/using/reference/requirements.html){target=_blank}.
* 次の期間の後に `VisitorAPI.js` ファイルが読み込まれた場合は、 `Visitor.getInstance` メソッドを使用して、必要な設定を使用して初期化します。
* の使用に慣れてください。 [使用可能なメソッド](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/get-set.html){target=_blank}.

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.2：顧客 ID の設定 {#set}

この手順を使用すると、訪問者の既知の ID（CRM ID、ユーザー ID など）を、 [!DNL Adobe] クロスデバイスパーソナライゼーション用。

+++詳細を見る

![顧客 ID を設定](/help/dev/patterns/recs-atjs/assets/set-customer-id-combined.png){width="400" zoomable="yes"}

**前提条件**

* 訪問者の既知の ID は、データレイヤーで使用できる必要があります。

**顧客 ID を設定**
詳しくは、 [setCustomerIDs](https://experienceleague.adobe.com/docs/id-service/using/id-service-api/methods/setcustomerids.html){target=_blank}.

**読み取り**

* [mbox3rdPartyId のリアルタイムプロファイル同期](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html){target=_blank}

**アクション**

* 用途 `visitor.setCustomerIDs` 訪問者の既知 ID を設定する。

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.3：自動ページ読み込みリクエストの設定 {#automatic}

この手順により、at.js で、at.js JavaScript ライブラリファイルの読み込み中に、ページでレンダリングする必要があるすべてのエクスペリエンスを取得できます。

+++詳細を見る

![自動ページ読み込みリクエストの設定](/help/dev/patterns/recs-atjs/assets/configure-automatic-page-request-combined.png){width="400" zoomable="yes"}

**前提条件**

* データレイヤー内のすべてのデータをに送信する必要はありません [!DNL Target]. 実験、最適化、パーソナライゼーションに役立つデータを判断するには、ビジネスチーム（デジタルマーケティングチーム）に相談します。 このデータのみがに送信されます [!DNL Target].
* に個人識別情報 (PII) データを送信しないようにする [!DNL Target].

**自動ページ読み込みリクエストの設定**

詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。

**読み取り**

詳しくは、 `pageLoadEnabled` 設定 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**アクション**

* を変更します。 `window.targetGlobalSettings` オブジェクトを使用して、自動ページ読み込みリクエストを有効にします。

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.4：ちらつきの処理を設定する {#flicker}

この手順を使用すると、エクスペリエンスを配信する際に、ページのちらつきが発生しないようにできます。

+++詳細を見る

![ちらつき処理図の設定](/help/dev/patterns/recs-atjs/assets/flicker-handling-combined.png){width="400" zoomable="yes"}

**前提条件**

* at.js で使用されるデフォルトの方法を使用してちらつきを制御することの長所と短所に関する Web ページのパフォーマンスを担当するチームとディスカッションします。 ローダアニメーションなど、カスタムのちらつき処理ソリューションを使用できるデザインパターンを検索できます。 パターンが見つからない場合は、新しいパターンを要求できます。

**ちらつき処理の設定**

詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。

設定 `bodyHidingEnabled` から `true` は、ページ読み込み要求の処理中に、ページ本文全体を非表示にします。 何らかの理由で自動ページ読み込みリクエストを有効にしていない場合（例えば、後でデータの準備ができない場合）、この設定をに設定することをお勧めします。 `false`.

を無効にしている場合 `bodyHidingEnabled` APLR を実行せず、後でページリクエストを実行したい場合、またはちらつき処理が不要な場合は、独自のちらつき処理を実装する必要があります。 ちらつきには、2 つの方法（テスト中のセクションを非表示にする方法と、テスト中のセクションにスローバーを表示する方法）を使用できます。

**読み取り**

* [at.js によるちらつきの制御方法](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
* bodyHiddenStyle オブジェクトと bodyHiddingEnabled オブジェクトについては、 [targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md).

**アクション**

* を変更します。 `window.targetGlobalSettings` 設定するオブジェクト `bodyHiddenStyle` および `bodyHidingEnabled`.

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.5：データマッピングの設定 {#data-mapping}

この手順は、に送信する必要のあるすべてのデータを確実に送信するのに役立ちます。 [!DNL Target] が設定されている。

+++詳細を見る

![データマッピング図](/help/dev/patterns/recs-atjs/assets/data-mapping-combined.png){width="400" zoomable="yes"}

**前提条件**

* データレイヤーには、に送信する必要のあるすべてのデータが準備されている必要があります。 [!DNL Target].
* Recommendations：プロファイルを強化します。
   * 合格 `entity.id` 最近表示された製品に基づく条件に基づいて、最近表示された製品のデータを取り込む。
   * 合格 `entity.id` お気に入りのカテゴリに基づいて、人気度の条件に関するデータをキャプチャする。
   * カスタム条件が基づいている場合や、任意の条件でのインクルージョンルールのフィルタリングで使用されている場合は、プロファイル属性を渡します。
* Recommendations：製品データを取り込みます。
   * その他のエンティティパラメーター（予約済みおよびカスタム）は、 [!DNL Recommendations].
   * また、 [!DNL Target] UI または API。

**データのマッピング先[!DNL Target]**

詳しくは、 [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md).

**読み取り**

* [targetPageParams()](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)
* [Recommendations の計画と実装](/help/dev/implement/recommendations/recommendations.md)
* [Recommendationsカタログの設定](/help/dev/implement/recommendations/recommendations.md)

**アクション**

* 以下を使用します。 `targetPageParams()` 関数を使用して、 [!DNL Target].

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.6：昇格 {#promotion}

プロモーション項目を追加して、 [!DNL Target Recommendations] [デザイン](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html){target=_blank}.

+++詳細を見る

**使用可能なオプション**

* ID 別に昇格
* [コレクション別に昇格](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/collections.html){target=_blank}
* [属性別にプロモート](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html){target=_blank}

**エンティティのパラメーターが必要です**

* 「属性別にプロモート」オプションを使用する場合は、プロモーションの項目属性を渡す必要があります。

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.7：買い物かごに基づく条件 {#cart}

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

## 1.8：人気度ベースの条件 {#popularity}

サイト全体での品目の全体的な人気度に基づいて、またはユーザーのお気に入りまたは最も多く閲覧されたカテゴリ、ブランド、ジャンルなど内の品目の人気度に基づいてレコメンデーションをおこないます。

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

* `entity.categoryId` または、現在の品目または品目属性に基づく条件の場合は、人気度に基づく品目属性。
* サイト全体で最も多く閲覧された/販売されたトップに対しては、何も渡さないでください。

**読み取り**

* [人気度ベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.9：項目ベースの条件 {#item}

ユーザーが閲覧中または最近閲覧した品目に類似した品目を見つけることに基づいてレコメンデーションをおこないます。

+++詳細を見る

**使用可能な条件**

* [!UICONTROL これを閲覧した人が他に閲覧したもの]
* [!UICONTROL これを閲覧した人が購入したもの]
* [!UICONTROL これを購入した人が他に購入したもの]
* [!UICONTROL 類似の属性を持つ品目]

**エンティティのパラメーターが必要です**

* `entity.id` またはキーとして使用される任意のプロファイル属性

**読み取り**

* [項目ベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/algorithms.html?lang=en#section_885B3BB1B43048A88A8926F6B76FC482){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.10：ユーザーベースの条件 {#user}

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

## 1.11：カスタム条件 {#custom}

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

## 1.12：インクルージョンルールで使用する属性を提供します。 {#inclusion}

+++詳細を見る

**読み取り**

* [動的および静的インクルージョンルールの使用](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/dynamic-static/use-dynamic-and-static-inclusion-rules.html){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.13: excludedIds の提供 {#exclude}

レコメンデーションから除外するエンティティのエンティティ ID を渡します。 例えば、既に買い物かごにある項目は除外した方がよいでしょう。

+++詳細を見る

**読み取り**

* [エンティティを動的に除外することはできますか？ ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=en#exclude){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.14: `entity.event.detailsOnly=true` パラメーター {#true}

エンティティの属性を使用して、製品やコンテンツの情報をに渡します。 [!DNL Target Recommendations].

+++詳細を見る

**読み取り**

* [エンティティの属性](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/entity-attributes.html?lang=en){target=_blank}

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.15：リモートデータマッピングの設定（リモート）

この手順により、に送信する必要のあるすべてのデータが [!DNL Target] が設定されている。

+++詳細を見る

![リモートデータマッピング図](/help/dev/patterns/recs-atjs/assets/remote-data-mapping-combined.png){width="400" zoomable="yes"}

**前提条件**

* データレイヤーには、に送信する必要のあるすべてのデータの準備が整っている必要があります。 [!DNL Target].

**データプロバイダーの設定**

詳しくは、 [データプロバイダー](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers).

**読み取り**

[targetPageParams 関数 [targetPageParams かんすう ]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)

**アクション**

以下を使用します。 `targetPageParams()` 関数を使用して、 [!DNL Target].

+++

[このページの上部にある図に戻ります。](#diagram)

## 1.16: at.js を読み込む {#web}

この手順では、at.js JavaScript ライブラリが読み込まれ、初期化されます。

+++詳細を見る

![Adobe Target at.js 図を読み込む](/help/dev/patterns/recs-atjs/assets/load-atjs-combined.png){width="400" zoomable="yes"}

**前提条件**

* をダウンロードするか、デジタルマーケティングチームに `at.js 2.*x*` JavaScript ライブラリファイル。

*読み取り*

* [Target の仕組み](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html){target=_blank}
* [at.js の仕組み](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
* [タグマネージャーを使用しない Target の実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

**アクション**

at.js ファイルは、実験、最適化、パーソナライゼーション、データ収集をおこなう必要があるすべての Web ページに埋め込みます。

+++

[このページの上部にある図に戻ります。](#diagram)





























