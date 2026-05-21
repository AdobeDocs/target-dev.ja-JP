---
title: 配信APIでレコメンデーションを取得する方法
description: この記事では、Adobe Target Delivery APIを使用してレコメンデーションコンテンツを取得するために必要な手順を開発者に説明します。
feature: APIs/SDKs, Recommendations, Administration & Configuration
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 9b391f42-2922-48e0-ad7e-10edd6125be6
TQID: https://experienceleague.adobe.com/K94vITD8ZSDXLkC42Vm02eC5RmHudBvukXNcdPFVjzk
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 129298289889a3b133eb07d0caeade2fd0b5568e
workflow-type: tm+mt
source-wordcount: 1366
ht-degree: 1%

---

# 配信APIを使用したRecommendationsの取得

Adobe TargetおよびAdobe Target Recommendations APIは、web ページへのレスポンスを配信するために使用できますが、アプリ、スクリーン、コンソール、電子メール、キオスクおよびその他のディスプレイデバイスなど、HTML ベース以外のエクスペリエンスでも使用できます。 つまり、Target ライブラリとJavaScriptを使用できない場合でも、[Target Delivery API](/help/dev/implement/delivery-api/overview.md)を使用すると、Targetのすべての機能にアクセスして、パーソナライズされたエクスペリエンスを配信できます。

>[!NOTE]
>
>実際のレコメンデーション（推奨製品またはアイテム）を含むコンテンツをリクエストする場合は、Target Delivery APIを使用します。

レコメンデーションを取得するには、適切なコンテキスト情報を含むAdobe Target Delivery API POST呼び出しを送信します。これには、ユーザーID （ユーザーの最近閲覧した項目などのプロファイル固有のレコメンデーションで使用するため）、関連するmbox名、mbox パラメーター、プロファイルパラメーター、またはその他の属性が含まれます。 応答には、推奨されるentity.id （およびその他のエンティティデータを含む場合があります）がJSONまたはHTML形式で含まれ、デバイスに表示されます。

Adobe Target用[配信API](/help/dev/implement/delivery-api/overview.md)は、標準のTarget リクエストが提供するすべての既存の機能を公開します。

配信API:

* 場所とオーディエンスのエクスペリエンスやオファーをRESTfulな方法で取得できます。
* 認証は必要ありません。
* 投稿のみ。
* Cookieやリダイレクト呼び出しは処理しません。
* 「ユーザーの役割」を必要としない、または認識しない。 Target エッジサーバーにコンテンツやレポートイベントを取得するだけです。

レコメンデーションを含むTarget エクスペリエンスを配信するためにDelivery APIを使用するには、次の手順に従います。

1. フォームベースのコンポーザー（Visual Experience Composerではなく）を使用して、Target アクティビティ（A/B、XT、AP、またはRecommendations）を作成します。
1. 配信APIを使用して、作成したTarget アクティビティによって生成されたリクエストに対する応答を取得します。

<!-- Q: Why are BOTH steps necessary for this? If you have a Form-based recommendation defined for an mbox, what's the point/benefit of ALSO having the Delivery API step in to retrieve results? Why can't you just have the Form-based Rec deliver the results in the destination device...?? A: See use case below... it's when you want to "intercept" the pending results in order to do more stuff prior to displaying the results. Things like real-time comparisons to inventory levels. -->

## フォームベースのExperience Composerを使用したレコメンデーションの作成

配信APIで使用できるレコメンデーションを作成するには、[ フォームベースのコンポーザー](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)を使用します。

1. まず、レコメンデーションで使用するJSON ベースのデザインを作成して保存します。 サンプル JSONと、フォームベースのアクティビティを設定する際にJSON応答を返す方法に関する背景情報については、[ レコメンデーションデザインの作成](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-design/create-design.html)に関するドキュメントを参照してください。 この例では、デザインの名前は&#x200B;*Simple JSONです。*
   ![server-side-create-recs-json-design.png](assets/server-side-create-recs-json-design.png)

1. Targetで、**[!UICONTROL Activities]** > **[!UICONTROL Create Activity]** > **[!UICONTROL Recommendations]**&#x200B;に移動し、**[!UICONTROL Form]**&#x200B;を選択します。

   ![server-side-create-recs.png](assets/server-side-create-recs.png)

1. プロパティを選択し、**[!UICONTROL Next]**&#x200B;をクリックします。
1. レコメンデーションの応答をユーザーに受け取ってもらう場所を定義します。 次の例では、*api_charter*&#x200B;という名前の場所を使用しています。 *Simple JSONという名前で以前に作成したJSON ベースのデザインを選択します。*
   ![server-side-create-recs-form.png](assets/server-side-create-recs-form1.png)
1. レコメンデーションを保存してアクティブ化します。 結果が生まれます。 [結果の準備ができたら](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-activity/previewing-and-launching-your-recommendations-activity.html)、配信APIを使用してそれらを取得できます。

## 配信APIの使用

[配信API](/help/dev/implement/delivery-api/overview.md)の構文は次のとおりです。

`POST https://{{CLIENT_CODE}}.tt.omtrdc.net/rest/v1/delivery`

1. クライアントコードは必須です。 リマインダーとして、**[!UICONTROL Recommendations]** > **[!UICONTROL Settings]**&#x200B;に移動すると、Adobe Targetにクライアントコードが表示される場合があります。 「**Recommendation API トークン**」セクションの「**クライアントコード**」の値に注意してください。
   ![client-code.png](assets/client-code.png)
1. クライアントコードを取得したら、配信API呼び出しを作成します。 以下の例は、[Delivery API Postman コレクション ](../../implement/delivery-api/overview.md#section/Getting-Started/Postman-Collection)で提供されている&#x200B;**[!UICONTROL Web Batched Mboxes Delivery API Call]**&#x200B;から始まり、関連する変更を行います。 次に例を示します。
   * **browser**&#x200B;および&#x200B;**address** オブジェクトは、HTML以外のユースケースでは必要ではないため、**Body**&#x200B;から削除されました
   * この例では、*api_charter*&#x200B;が場所の名前としてリストされています
   * entity.idは指定されています。このレコメンデーションはコンテンツの類似性に基づいているため、現在のアイテムキーをTargetに渡す必要があります。
     ![server-side-Delivery-API-call.png](assets/server-side-delivery-api-call2.png)
クエリパラメーターを正しく設定することを忘れないでください。 例えば、必要に応じて`{{CLIENT_CODE}}`を指定してください。 <!-- Q: In the updated call syntax, entity.id is listed as a profileParameter instead of an mboxParameter as in older versions. Q: Old image ![server-side-create-recs-post.png](assets/server-side-create-recs-post.png) Old accompanying text: "Note this recommendation is based on Content Similar products based on the entity.id sent via mboxParameters." -->
     ![client-code3](assets/client-code3.png)
1. リクエストを送信します。 これは、アクティブな推奨事項が実行されている&#x200B;*api_charter*&#x200B;の場所に対して実行され、推奨されるエンティティのリストを出力するJSON デザインで定義されます。
1. JSON デザインに基づく応答を受け取ります。
   ![server-side-create-recs-json-response2.png](assets/server-side-create-recs-json-response2.png)
応答には、キーIDと、推奨されるエンティティのエンティティ IDが含まれます。

この方法でDelivery APIとRecommendationsを使用すると、HTML以外のデバイスで訪問者にレコメンデーションを表示する前に、追加の手順を実行できます。 例えば、Delivery APIからの応答を利用して、別のシステム（CMS、PIM、e コマースプラットフォームなど）からエンティティ属性の詳細（在庫、価格、評価など）をリアルタイムで追加して検索し、最終的な結果を表示できます。

このガイドで説明したアプローチを使用すると、Targetからの応答を活用して、パーソナライズされたレコメンデーションを提供する任意のアプリケーションを取得できます。

## 実装例

以下の資料は、HTML以外に焦点を当てた様々な実装の例を示しています。 システムやデバイスが異なるため、実装はそれぞれ異なることに留意してください。

| リソース | 詳細 |
| --- | --- |
| [Experience Platform LaunchでのTarget拡張機能の設定とTarget APIの実装](https://developer.adobe.com/client-sdks/documentation/adobe-target/) | Experience Platform LaunchでTarget拡張機能を設定し、Target拡張機能をアプリに追加し、アクティビティのリクエスト、オファーの先行取得、ビジュアルプレビューモードの開始を行うためのTarget APIを実装する手順。 |
| [Adobe Target Node Client](https://www.npmjs.com/package/@adobe/target-nodejs-sdk) | オープンソースのTarget Node.js SDK v1.0 |
| [ サーバーサイドの概要](../../implement/server-side/server-side-overview.md) | Adobe Target Server Side Delivery API、Server Side Batch Delivery API、Node.js SDK、Adobe Target Recommendations APIに関する情報。 |
| 電子メールの[Adobe Campaign コンテンツのレコメンデーション ](https://medium.com/adobetech/adobe-campaign-content-recommendations-in-email-b51ced771d7f) | Adobe TargetとAdobe CampaignのAdobe I/O Runtimeを使用して、メールでコンテンツレコメンデーションを活用する方法を説明したブログです。 |

## APIを使用したレコメンデーション設定の管理

多くの場合、レコメンデーションはAdobe Target UIで設定され、上記のセクションで説明した理由から、Target APIを介して使用またはアクセスされます。 このUIとAPIの連携は一般的です。 しかし、ユーザーがAPIを介してすべてのアクション（設定と結果の使用）を実行したい場合があります。 あまり一般的ではありませんが、ユーザーはAPIを使用してレコメンデーションの結果を完全に活用して、完全に設定、実行、*および*&#x200B;することができます。

Adobe Target Recommendations エンティティを管理し、サーバーサイドで配信する方法については、[前のセクション ](manage-catalog.md)で説明しました。 同様に、[Adobe Developer Console](https://developer.adobe.com/console/home)では、Adobe Targetにログインしなくても、条件、プロモーション、コレクション、デザインテンプレートを管理できます。 すべてのRecommendations APIの完全なリストは[ここ](https://developer.adobe.com/target/administer/recommendations-api/)にありますが、参照用の概要はここにあります。

| リソース | 詳細 |
| --- | --- |
| [コレクション](https://developer.adobe.com/target/administer/recommendations-api/#tag/Collections) | コレクションを一覧表示、作成、取得、編集、削除します。 |
| [条件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Criteria) | 条件のリストと取得。 |
| [ デザイン ](https://developer.adobe.com/target/administer/recommendations-api/#tag/Designs) | デザインのリスト、作成、取得、編集、削除、検証。 |
| [ エンティティ ](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities) | エンティティの保存、削除、取得。 |
| [ プロモーション ](https://developer.adobe.com/target/administer/recommendations-api/#tag/Promotions) | プロモーションのリスト、作成、取得、編集、削除。 |
| [ カテゴリ条件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Category-Criteria) | カテゴリ条件をリスト、作成、取得、編集、および削除します。 |
| [ カスタム条件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Custom-Criteria) | カスタム条件をリスト、作成、取得、編集、削除します。 |
| [項目条件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Item-Criteria) | 項目条件をリスト、作成、取得、編集、および削除します。 |
| [人気条件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Popularity-Criteria) | 人気条件のリスト、作成、取得、編集、削除。 |
| [ プロファイル属性条件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Profile-Attribute-Criteria) | プロファイル属性条件をリスト、作成、取得、編集、削除します。 |
| [最近の条件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Recent-Criteria) | 最近の条件をリスト、作成、取得、編集、および削除します。 |
| [ シーケンス条件](https://developer.adobe.com/target/administer/recommendations-api/#tag/Sequence-Criteria) | シーケンスの条件をリスト、作成、取得、編集、削除します。 |

## リファレンスドキュメント

* [Adobe Target Delivery API ドキュメント](/help/dev/implement/delivery-api/overview.md)
* [レコメンデーションとメールの統合](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/integrating-recs-email.html)

## 概要とレビュー

おめでとうございます。 このガイドでは、次の方法について解説しました。
* [Recommendations APIを使用したカタログの管理](manage-catalog.md)
* [Recommendations APIを使用したカスタム条件の管理](manage-custom-criteria.md)
* [Recommendationsでの配信APIの使用](fetch-recs-server-side-delivery-api.md)
