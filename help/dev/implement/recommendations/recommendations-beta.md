---
keywords: Recommendations、settings、preferences、industry vertical、filter incompatible criteria、default host group、thumb base url、recommendations api token,
description: '[!UICONTROL Recommendations]で [!DNL Adobe Target] アクティビティを実装する方法について説明します。'
title: '[!UICONTROL Recommendations] アクティビティを実装するにはどうすればよいですか？'
feature: Recommendations
hide: true
exl-id: 0a9c9649-195b-44e2-987e-d02eaf98cc54
source-git-commit: 760e6425fc2a32ec5c405094d144f173de1b6c07
workflow-type: tm+mt
source-wordcount: '1550'
ht-degree: 18%

---

# [!UICONTROL Recommendations]の計画と実装

[!DNL Adobe Target Recommendations]の計画と実装に役立つ情報。

>[!NOTE]
>
>この記事に加えて、[Adobe Target Business Practitioner Guide](https://experienceleague.adobe.com/en/docs/target/using/target-home){target=_blank}には、[Target Recommendations](https://experienceleague.adobe.com/en/docs/target/using/recommendations/recommendations){target=_blank}に関する詳細な情報が含まれています。

[!UICONTROL Recommendations]で最初の[!DNL Adobe Target] アクティビティを設定する前に、次の手順を実行します。

1. [&#x200B; ユーザーの行動のキャプチャとレコメンデーションの配信に使用するwebおよびモバイルアプリのサーフェスに[!UICONTROL Target]](#implement-target)を実装します。
1. [&#x200B; ユーザーに推奨する製品またはコンテンツの[!UICONTROL Recommendations] カタログ &#x200B;](#set-up-your-recommendations-catalog)を設定します。
1. [行動情報とコンテキスト &#x200B;](#pass-behavioral-information-and-context)を[!DNL Target Recommendations]に渡して、パーソナライズされたレコメンデーションを配信できるようにします。
1. [&#x200B; グローバル除外の設定](#configure-global-exclusions)。
1. [設定[!UICONTROL Recommendations]を構成](#configure-recommendations-settings)。
1. （オプション） [管理者API[!UICONTROL Recommendations]を使用して](#administer-recommendations-using-admin-apis)を管理します。

## &#x200B;1. [!UICONTROL Target]の実装

[!DNL Target Recommendations]では、[!DNL Adobe Experience Platform Web SDK]またはat.js 0.9.2 （以降）を実装する必要があります。 詳しくは、[[!UICONTROL Target] クライアントサイド実装ガイド &#x200B;](../client-side/overview.md)を参照してください。

## &#x200B;2. [!UICONTROL Recommendations] カタログを設定する

高品質なレコメンデーションを提供するには、[!UICONTROL Target]がレコメンデーションする製品またはコンテンツについて知っている必要があります。 カタログには通常、推奨項目に関する3種類の情報が含まれます。 例えば、映画を推薦しているとします。 次の項目を含めます。

1. レコメンデーションを受け取るユーザーに表示したいデータ。例えば、ムービーの名前と、ムービーポスターのサムネール画像のURLを表示できます。
1. マーケティングおよびマーチャンダイジング制御に便利なデータ。例えば、NC-17 ムービーを推奨しないように、ムービーの評価を表示できます。
1. 項目と他の項目の類似性を判断するのに役立つデータ。 例えば、ムービーのジャンルとムービーの監督を表示できます。

[!UICONTROL Target]には、カタログに入力するための複数の統合オプションが用意されています。 これらのオプションを組み合わせて使用すると、カタログ内の異なる項目を更新したり、異なる頻度で異なる項目属性を更新したりできます。

| メソッド | 現状 | 使用するタイミング | 追加情報 |
| --- | --- | --- | --- |
| カタログフィード | フィード（CSV、[!DNL Google]製品XML、または[!UICONTROL Analytics Product Classifications]）のアップロードと取り込みを毎日スケジュールします。 | 一度に複数の項目に関する情報を送信します。 情報の送信に使用できます。 | [&#x200B; フィード &#x200B;](https://experienceleague.adobe.com/en/docs/target/using/recommendations/entities/feeds)を参照してください。 |
| エンティティ API | APIを呼び出して、1つの項目の最新更新を送信します。 | アップデートを送信する機能です。 頻繁に変更される情報（価格、在庫/在庫レベルなど）の送信用。 | [Entities API開発者ドキュメント &#x200B;](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities)を参照してください。 |
| ページの更新を渡します | ページ上のJavaScriptや配信APIを使用して、1つの商品の最新情報を送信できます。 | アップデートを送信する機能です。 頻繁に変更される情報（価格、在庫/在庫レベルなど）の送信用。 | 以下の[&#x200B; アイテムビュー/製品ページ &#x200B;](#item-views-or-product-pages)を参照してください。 |

ほとんどの顧客は、少なくとも1つのフィードを実装する必要があります。 その後、エンティティ APIまたはページ上のメソッドを使用して、頻繁に変更される属性や項目の更新をフィードに補完するように選択できます。

## &#x200B;3. 行動情報とコンテクストの伝達

[!UICONTROL Target]に渡す行動情報とコンテキストは、訪問者が実行するアクションによって異なります。これは、多くの場合、訪問者が操作するページのタイプに関連付けられます。

### アイテムビューまたは商品ページ

製品詳細ページなど、訪問者が単一の項目を表示しているページでは、訪問者が表示している項目のIDを渡す必要があります。 また、訪問者が表示している項目の最も詳細なカテゴリを渡して、現在のカテゴリに対するレコメンデーションのフィルタリングを許可します。

また、製品ページ自体に、特定のすぐに変更される属性を渡すこともできます。 例えば、価格（`value`）と在庫/在庫レベルを渡すことができます。

#### 価格と在庫の受け渡し

```js {line-numbers="true"}
<script type="text/javascript">
function targetPageParams() { 
   return { 
      "entity": { 
         "id": "32323", 
         "categoryId": "running-shoes", 
         "value": 119.99, 
         "inventory": 329 
      } 
   } 
}
</script>
```

### カテゴリビュー/カテゴリページ

カテゴリーページでは、レコメンデーションをそのカテゴリ内の商品やコンテンツに限定する場合が多くあります。 そのためには、現在表示されているカテゴリのIDを渡します。

#### 現在のカテゴリを渡す

```js {line-numbers="true"}
function targetPageParams() { 
   return { 
      "entity": { 
         "categoryId": "running-shoes" 
      } 
   } 
}
```

### 買い物かごへの追加／買い物かごの表示／チェックアウトページ

カートページでは、訪問者の現在のカートの内容に基づいて商品をレコメンデーションできます。 これには、特別なパラメーター`cartIds`を使用して、訪問者の現在のカート内のすべてのアイテムのIDを渡します。

#### 現在カートにあるアイテムを渡す

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "cartIds": "352,223,23432,432,553"
      }
}
```

カートベースのレコメンデーションについて詳しくは、「[&#x200B; Business Practitioner Guide](https://experienceleague.adobe.com/en/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key#cart-based)」の「*[!DNL Adobe Target]カートベース*」を参照してください。

### 訪問者の買い物かごにすでに入っている品目を除く

サイト全体のページで、レコメンデーションからいくつかの項目を除外できます。 例えば、訪問者の現在のカートにある商品をレコメンデーションしたくないことがあります。 これを行うには、特殊パラメーター`excludedIds`を使用して、除外するすべての項目のIDを渡します。

#### 除外する項目を渡す

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "excludedIds": "352,223,23432,432,553"
      }
}
```

### 購入/注文確認ページ

購入イベントが発生したら、購入したアイテムのIDを渡します。 「[at.jsのデプロイ方法> タグマネージャーを使用せずに](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)を実装する方法[記事の[!UICONTROL Target] コンバージョンを追跡](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)を参照してください。

## &#x200B;4. グローバル除外の設定

訪問者に推奨したくないグローバルレベルの項目を除外します。 [&#x200B; Business Practitioner Guide](https://experienceleague.adobe.com/en/docs/target/using/recommendations/entities/exclusions)の&#x200B;*[!DNL Adobe Target]Exclusions*&#x200B;を参照してください。

## &#x200B;5. [!UICONTROL Recommendations]設定の構成

設定を使用して [!UICONTROL Recommendations] の実装を管理します。

**[!UICONTROL Recommendations Settings]** オプションにアクセスするには、[!DNL Target]で[!DNL Adobe Experience Cloud]を開き、**[!UICONTROL Administration]** > **[!UICONTROL Recommendations]**&#x200B;をクリックします。

![推奨事項の設定ページ &#x200B;](/help/dev/implement/recommendations/assets/recs-settings-new.png)

次のオプションを設定します。

### [!UICONTROL Recommendations API Token]

[!UICONTROL Recommendations API Token] セクションでは、次のオプションを使用できます。

#### [!UICONTROL Client code]

[!DNL Target] [!UICONTROL client code]です。

[!UICONTROL client code]を知らない場合は、[!DNL Target] ユーザーインターフェイスで、**[!UICONTROL Administration]** > **[!UICONTROL Implementation]**&#x200B;をクリックします。 [!UICONTROL client code]が[!UICONTROL Account Details] セクションに表示されます。

#### 認証トークン

[!DNL Adobe Target] APIを含む[!DNL Recommendations Admin]管理APIは、認証によって保護され、承認済みのユーザーのみが[!DNL Adobe Target]にアクセスするために使用できるようになります。 [Adobe Developer Console](https://developer.adobe.com/console/home)を使用して、[!DNL Adobe Experience Cloud solutions]を含むすべての[!DNL Adobe Target]の認証を管理します。

詳しくは、[Adobe Target APIの認証の設定](/help/dev/before-administer/configure-authentication.md)を参照してください。

>[!WARNING]
>
>新しい認証トークンを生成する場合は注意してください。 新しいトークンを生成すると、現在のトークンを使用したAPI呼び出しが失敗します。 新しく生成された認証トークンを使用して、スクリプトやアプリケーションを更新します。

### 条件

サイトの業種を把握することで、Adobe Targetがレコメンデーションの基準を選択することができます。

[!DNL Recommendations]の条件は、事前に決められた訪問者の行動セットに基づいて、推奨する製品またはコンテンツを決定するルールです。 基準は、人気の高いトレンド、訪問者の現在および過去の行動、類似の製品やコンテンツなどにもとづいて設定できます。 複数の条件を追加することで、複数のレコメンデーションタイプを相互にテストすることができます。

詳しくは、[Adobe Target Business Practitioner Guideの](https://experienceleague.adobe.com/en/docs/target/using/recommendations/criteria/algorithms){target=_blank}条件&#x200B;*を参照してください。*

[!UICONTROL Criteria] セクションでは、次の設定を使用できます。

#### [!UICONTROL Industry/Vertical]

業種は、レコメンデーション条件の分類に使用されます。この情報は、ショッピングカートページやメディアページに最適な基準など、特定のページに最適な基準を見つけるのに役立ちます。

ドロップダウンリストから次のカテゴリを使用できます。

* Personalizationなし
* 小売／e コマース
* リードジェネレーション／B2B／金融サービス
* メディア／投稿

#### [!UICONTROL Filter Incompatible Criteria]

このオプションを選択すると、選択されたページが必要なデータを渡す条件のみが表示されます。すべての基準があらゆるページで正しく実行されるわけではありません。 現在のアイテムまたは現在のカテゴリの推奨事項に互換性を持たせるには、ページまたはmboxを`entity.id`または`entity.categoryId`に渡す必要があります。

通常は、互換性のある条件のみを表示するようにします。ただし、アクティビティで互換性のない基準を使用できるようにするには、このオプションを有効にしないでください。

Adobeでは、タグ管理ソリューションを使用する場合は、このオプションを無効にすることをお勧めします。

このオプションについて詳しくは、「[[!UICONTROL Recommendations] Business Practitioner Guide](https://experienceleague.adobe.com/en/docs/target/using/recommendations/recommendations-faq/recommendations-faq){target=_blank}」の「*[!DNL Adobe Target]FAQ*」を参照してください。

### [!UICONTROL Product Catalog]

[!UICONTROL Product Catalog] セクションでは、次のオプションを使用できます。

#### [!UICONTROL Default Host Group]

デフォルトのホストグループを選択します。

ホストグループを使用して、カタログの利用可能な項目をさまざまな用途に分割できます。例えば、ホストグループは開発環境と本番環境、さまざまなブランド、またはさまざまな地域に使用できます。デフォルトでは、カタログ検索、コレクションおよび除外のプレビュー結果はデフォルトのホストグループに基づいています。（環境フィルターを使用して、結果をプレビューする別のホストグループを選択することもできます）。 デフォルトでは、新しく追加された項目は、項目の作成または更新時に環境IDが指定されていない限り、すべてのホストグループで使用できます。 配信されるレコメンデーションは、リクエストで指定したホストグループによって異なります。

商品が表示されていない場合は、適切なホストグループが使用されていることを確認してください。例えば、ステージング環境を使用するようにレコメンデーションを設定し、ホストグループをステージングに設定した場合、商品を表示するために、ステージング環境のコレクションを再作成する必要がある可能性があります。各環境でどの商品が利用できるかを確認するには、各環境でカタログ検索を利用します。選択した環境（ホストグループ）の[!UICONTROL Recommendations] コレクションと除外の内容をプレビューすることもできます。

>[!NOTE]
>
>選択した環境を変更した後、**[!UICONTROL Search]**&#x200B;をクリックして、返された結果を更新する必要があります。

**[!UICONTROL Environment]** フィルターは、Target UIの次の場所から使用できます。

* カタログ検索（**[!UICONTROL Recommendations]** > **[!UICONTROL Catalog Search]**）
* コレクション （**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]**）
* コレクションを作成ダイアログボックス （**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Create collection]**）
* コレクションを更新ダイアログボックス （**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Edit]**）
* 除外を作成ダイアログボックス （**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Create exclusion]**）
* 除外を更新ダイアログボックス （**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Edit]**）

詳しくは、「[&#x200B; Business Practitioner Guide](https://experienceleague.adobe.com/en/docs/target/using/administer/hosts){target=_blank}」の「*[!DNL Adobe Target]Hosts*」を参照してください。

#### [!UICONTROL Thumbnail Base]

商品カタログのベース URL を設定すると、商品のサムネールの指定でサムネール URL を渡す場合に、相対 URL を使用できます。

次に例を示します。

`"entity.thumbnailURL=/Images/Homepage/product1.jpg"`

サムネールのベース URL に対する相対 URL を設定します。

### [!UICONTROL Custom Attribute Key Configuration]

訪問者のプロファイルに保存されている項目にもとづいて、レコメンデーションを決定します。 例えば、「最後にカートに追加された商品」、「最後に視聴した動画が90%以上表示された」などです。

**[!UICONTROL Add]**&#x200B;をクリックして新しい設定を作成し、設定の名前を指定し、目的のプロファイル属性を選択してから、**[!UICONTROL Save]**&#x200B;をクリックします。

## &#x200B;6. （オプション）管理者APIを使用して[!UICONTROL Recommendations]を管理

[の[!UICONTROL Recommendations]管理者および配信APIを設定および使用する方法については、](../../before-administer/recs-api/overview.md)APIs[!UICONTROL Target]の実践ガイドを参照してください。[!UICONTROL Recommendations]
