---
keywords: Recommendations，設定，環境設定，業種，互換性のない条件をフィルター，デフォルトのホストグループ，thumb base url, recommendations api トークン，
description: ' [!DNL Adobe Target] で [!UICONTROL Recommendations] アクティビティを実装する方法を説明します。'
title: '[!UICONTROL Recommendations] アクティビティの実装方法'
feature: Recommendations
hidefromtoc: true
hide: true
exl-id: 0a9c9649-195b-44e2-987e-d02eaf98cc54
source-git-commit: aa032255222d92aeddd7238922eb450f1b6b93a0
workflow-type: tm+mt
source-wordcount: '1550'
ht-degree: 20%

---

# [!UICONTROL Recommendations] の計画と実装

[!DNL Adobe Target Recommendations] の計画と実装に役立つ情報です。

>[!NOTE]
>
>この記事に加えて、[Adobe Target ビジネス実践者ガイド ](https://experienceleague.adobe.com/ja/docs/target/using/target-home){target=_blank} には [Target Recommendations](https://experienceleague.adobe.com/ja/docs/target/using/recommendations/recommendations){target=_blank} に関する詳細が含まれています。

[!DNL Adobe Target] で最初の [!UICONTROL Recommendations] アクティビティを設定する前に、次の手順を実行します。

1. [ ユーザーの行動のキャプチャとレコメンデーションの配信に使用する [!UICONTROL Target]](#implement-target) を web およびモバイルアプリのサーフェスに実装します。
1. ユーザーにレコメンデーションする製品またはコンテンツの [[!UICONTROL Recommendations] カタログを設定 ](#set-up-your-recommendations-catalog) します。
1. [ 行動情報とコンテキストを渡す ](#pass-behavioral-information-and-context) を [!DNL Target Recommendations] に渡して、パーソナライズされたレコメンデーションを提供できるようにします。
1. [ グローバル除外を設定 ](#configure-global-exclusions) します。
1. [[!UICONTROL Recommendations] 設定を構成します ](#configure-recommendations-settings)。
1. （任意） [ 管理 API を使用して [!UICONTROL Recommendations] を管理します ](#administer-recommendations-using-admin-apis)。

## 1. [!UICONTROL Target] の実装

[!DNL Target Recommendations] では、[!DNL Adobe Experience Platform Web SDK] または at.js 0.9.2 （以降）を実装する必要があります。 詳しくは、[[!UICONTROL Target] クライアントサイド実装ガイド ](../client-side/overview.md) 参照してください。

## 2. [!UICONTROL Recommendations] カタログを設定する

高品質のレコメンデーションを配信 [!UICONTROL Target] るには、レコメンデーションする製品やコンテンツについて知っておく必要があります。 カタログには通常、推奨項目に関する 3 種類の情報が含まれています。 映画をレコメンデーションしているとします。 以下を含めます。

1. レコメンデーションを受け取るユーザーに表示したいデータ。例えば、ムービーの名前と、ムービーのポスターのサムネール画像の URL を表示できます。
1. マーケティングおよびマーチャンダイジング制御に便利なデータ。例えば、NC-17 ムービーをお勧めしないように、ムービーのレーティングを表示できます。
1. アイテムと他のアイテムとの類似性を判断するのに役立つデータです。 例えば、映画のジャンルや映画の監督を表示できます。

[!UICONTROL Target] では、カタログに入力するための複数の統合オプションを提供しています。 これらのオプションを組み合わせて使用すると、カタログ内の異なる項目を更新したり、異なる頻度で異なる項目属性を更新したりできます。

| メソッド | 概要 | 使用するタイミング | 追加情報 |
| --- | --- | --- | --- |
| カタログフィード | アップロードして毎日取り込むフィード（CSV、[!DNL Google] Product XML または [!UICONTROL Analytics Product Classifications]）のスケジュールを設定します。 | 一度に複数の項目に関する情報を送信する場合。 変更の頻度が低い情報を送信する。 | [ フィード ](https://experienceleague.adobe.com/ja/docs/target/using/recommendations/entities/feeds) を参照してください。 |
| エンティティ API | API を呼び出して、1 つの項目の最新の最新情報を送信します。 | 一度に 1 つの項目に関する更新を送信する場合。 頻繁に変更される情報（価格、在庫/在庫レベルなど）の送信。 | 詳しくは、[Entities API 開発者向けドキュメント ](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities) を参照してください。 |
| ページで更新を渡す | ページ上でJavaScriptを使用するか、配信 API を使用して、1 つの項目に関する最新の情報を送信します。 | 一度に 1 つの項目に関する更新を送信する場合。 頻繁に変更される情報（価格、在庫/在庫レベルなど）の送信。 | 以下の [ 項目表示/製品ページ ](#item-views-or-product-pages) を参照してください。 |

ほとんどのお客様は、1 つ以上のフィードを実装する必要があります。 その後、エンティティ API またはページ上で実行するメソッドを使用して、頻繁に変更される属性や項目の更新で、フィードを補完することを選択できます。

## 3.行動情報とコンテキストの受け渡し

[!UICONTROL Target] に渡す必要がある行動情報とコンテキストは、訪問者が実行しているアクションに応じて異なります。多くの場合、このアクションは、訪問者がやり取りしているページのタイプに関連付けられています。

### 品目ビューまたは製品ページ

製品の詳細ページなど、訪問者が 1 つの項目を表示しているページでは、訪問者が表示している項目の ID を渡す必要があります。 また、訪問者が表示している項目の最も詳細なカテゴリを渡して、現在のカテゴリに推奨事項をフィルタリングできるようにします。

また、変化の速い特定の属性を製品ページ自体に渡すこともできます。 例えば、価格（`value`）と在庫/在庫レベルを渡すことができます。

#### 価格と在庫を渡す

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

カテゴリページでは、レコメンデーションをそのカテゴリ内の製品やコンテンツに制限する必要が生じる場合があります。 そのためには、現在表示されているカテゴリの ID を渡す必要があります。

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

買い物かごページでは、訪問者の現在の買い物かごの内容に基づいて、アイテムをレコメンデーションできます。 それには、特別なパラメーター `cartIds` を使用して、訪問者の現在の買い物かごにあるすべての項目の ID を渡します。

#### 現在カートに入っている商品を渡す

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "cartIds": "352,223,23432,432,553"
      }
}
```

買い物かごベースの Recommendations について詳しくは、『買い物かごベースのビジネス実践者ガイド [&#128279;](https://experienceleague.adobe.com/ja/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key#cart-based) の  買い物かごベース *を参照し*[!DNL Adobe Target] ください。

### 訪問者の買い物かごにすでに入っている品目を除く

サイト全体のページで、一部の項目をレコメンデーションから除外できます。 例えば、訪問者の現在の買い物かごに既にある項目をお勧めしない場合があります。 それには、特別なパラメーター `excludedIds` を使用して、除外するすべての項目の ID を渡します。

#### 除外する項目を渡す

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "excludedIds": "352,223,23432,432,553"
      }
}
```

### 購入/注文確認ページ

購入イベントが発生したら、購入した品目の ID を渡します。 [at.js のデプロイ方法 ](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)/タグマネージャーを使用しない [!UICONTROL Target] の実装 [&#128279;](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md) 記事の  コンバージョンの追跡を参照してください。

## 4. グローバル除外の設定

訪問者に勧めたくない項目をグローバルレベルで除外します。 『 *[!DNL Adobe Target]Business Practitioner Guide[Exclusions](https://experienceleague.adobe.com/ja/docs/target/using/recommendations/entities/exclusions) 』を参照してください*。

## 5. [!UICONTROL Recommendations] 設定の指定

設定を使用して [!UICONTROL Recommendations] の実装を管理します。

**[!UICONTROL Recommendations Settings]** のオプションにアクセスするには、[!DNL Adobe Experience Cloud] で [!DNL Target] を開き、**[!UICONTROL Administration]** > **[!UICONTROL Recommendations]** をクリックします。

![Recommendations設定ページ ](/help/dev/implement/recommendations/assets/recs-settings-new.png)

次のオプションを設定します。

### [!UICONTROL Recommendations API Token]

[!UICONTROL Recommendations API Token] のセクションでは、次のオプションを使用できます。

#### [!UICONTROL Client code]

[!DNL Target][!UICONTROL client code]。

[!UICONTROL client code] が不明な場合は、[!DNL Target] ユーザーインターフェイスで **[!UICONTROL Administration]**/**[!UICONTROL Implementation]** をクリックします。 この [!UICONTROL client code] については、[!UICONTROL Account Details] の節を参照してください。

#### 認証トークン

[!DNL Recommendations Admin] API を含む [!DNL Adobe Target] 管理 API は、許可されたユーザーのみが [!DNL Adobe Target] へのアクセスに使用できるように、認証で保護されています。 [Adobe Developer Console](https://developer.adobe.com/console/home) を使用して、[!DNL Adobe Target] を含むすべての [!DNL Adobe Experience Cloud solutions] に対してこの認証を管理します。

詳しくは、[Adobe Target API の認証の設定 ](/help/dev/before-administer/configure-authentication.md) を参照してください。

>[!WARNING]
>
>新しい認証トークンを生成する際は注意が必要です。 新しいトークンを生成すると、現在のトークンを使用した API 呼び出しが失敗します。 新しく生成された認証トークンでスクリプトまたはアプリケーションを更新します。

### 条件

サイトの業種を把握することは、Target がレコメンデーションの条件を選択するのに役立ちます。

[!DNL Recommendations] の条件は、事前に定義された訪問者の行動に基づいて、どの製品やコンテンツをレコメンデーションするかを決定するルールです。 条件は、人気のあるトレンド、訪問者の現在および過去の行動、類似の製品およびコンテンツに基づくことができます。 複数の条件を追加することで、複数のレコメンデーションタイプを相互にテストすることができます。

詳しくは、*Adobe Target ビジネス実践者ガイドの [ 条件 ](https://experienceleague.adobe.com/ja/docs/target/using/recommendations/criteria/algorithms){target=_blank} を参照してください。*

[!UICONTROL Criteria] のセクションでは、次の設定を使用できます。

#### [!UICONTROL Industry/Vertical]

業種は、レコメンデーション条件の分類に使用されます。この情報は、チームのメンバーが特定のページに適した条件（買い物かごページやメディアページに最適な条件など）を見つけるのに役立ちます。

ドロップダウンリストでは、以下のカテゴリを使用できます。

* Personalizationなし
* 小売／e コマース
* リードジェネレーション／B2B／金融サービス
* メディア／投稿

#### [!UICONTROL Filter Incompatible Criteria]

このオプションを選択すると、選択されたページが必要なデータを渡す条件のみが表示されます。すべての条件がすべてのページで正しく実行されるわけではありません。 現在の項目/現在のカテゴリのレコメンデーションに互換性を持たせるには、ページまたは mbox が `entity.id` または `entity.categoryId` を渡す必要があります。

通常は、互換性のある条件のみを表示するようにします。ただし、互換性のない条件をアクティビティで使用できるようにする場合は、このオプションを有効にしないでください。

Adobeでは、タグ管理ソリューションを使用する場合、このオプションを無効にすることをお勧めします。

このオプションについて詳しくは、『 *[!DNL Adobe Target]Business Practitioner Guide 』の [[!UICONTROL Recommendations] FAQ](https://experienceleague.adobe.com/ja/docs/target/using/recommendations/recommendations-faq/recommendations-faq){target=_blank} を参照してください*

### [!UICONTROL Product Catalog]

[!UICONTROL Product Catalog] のセクションでは、次のオプションを使用できます。

#### [!UICONTROL Default Host Group]

デフォルトのホストグループを選択します。

ホストグループを使用して、カタログの利用可能な項目をさまざまな用途に分割できます。例えば、ホストグループは開発環境と実稼動環境、さまざまなブランド、またはさまざまな地域に使用できます。デフォルトでは、カタログ検索、コレクションおよび除外のプレビュー結果はデフォルトのホストグループに基づいています。（環境フィルターを使用して、結果をプレビューする別のホストグループを選択することもできます）デフォルトでは、項目の作成または更新時に環境 ID が指定されている場合を除き、新しく追加された項目はすべてのホストグループで使用できます。配信される Recommendations は、リクエストで指定したホストグループによって異なります。

商品が表示されていない場合は、適切なホストグループが使用されていることを確認してください。例えば、ステージング環境を使用するようにレコメンデーションを設定し、ホストグループをステージングに設定した場合、商品を表示するために、ステージング環境のコレクションを再作成する必要がある可能性があります。各環境でどの商品が利用できるかを確認するには、各環境でカタログ検索を利用します。選択した環境（ホストグループ）のコレクション [!UICONTROL Recommendations] 除外のコンテンツをプレビューすることもできます。

>[!NOTE]
>
>選択した環境を変更した後、「**[!UICONTROL Search]**」をクリックして返された結果を更新する必要があります。

**[!UICONTROL Environment]** フィルターは、Target UI の次の場所から使用できます。

* カタログ検索（**[!UICONTROL Recommendations]**/**[!UICONTROL Catalog Search]**）
* コレクション（**[!UICONTROL Recommendations]**/**[!UICONTROL Collections]**）
* コレクションを作成ダイアログボックス（**[!UICONTROL Recommendations]**/**[!UICONTROL Collections]**/**[!UICONTROL Create collection]**）
* コレクションを更新ダイアログボックス（**[!UICONTROL Recommendations]**/**[!UICONTROL Collections]**/**[!UICONTROL Edit]**）
* 除外を作成ダイアログボックス（**[!UICONTROL Recommendations]**/**[!UICONTROL Exclusions]**/**[!UICONTROL Create exclusion]**）
* 除外を更新ダイアログボックス（**[!UICONTROL Recommendations]**/**[!UICONTROL Exclusions]**/**[!UICONTROL Edit]**）

詳しくは、『 *[!DNL Adobe Target]Business Practitioner Guide[ の ](https://experienceleague.adobe.com/ja/docs/target/using/administer/hosts){target=_blank} ホスト* を参照してください。

#### [!UICONTROL Thumbnail Base]

商品カタログのベース URL を設定すると、商品のサムネールの指定でサムネール URL を渡す場合に、相対 URL を使用できます。

次に例を示します。

`"entity.thumbnailURL=/Images/Homepage/product1.jpg"`

サムネールのベース URL に対する相対 URL を設定します。

### [!UICONTROL Custom Attribute Key Configuration]

訪問者のプロファイルに保存された項目に基づいてお勧めを紹介します。 例えば、「最後に買い物かごに追加されたアイテム」、「最後に視聴したビデオが 90% 以上表示されました」などです。

「**[!UICONTROL Add]**」をクリックして新しい設定を作成し、設定の名前を指定し、目的のプロファイル属性を選択して「**[!UICONTROL Save]**」をクリックします。

## 6. （任意）管理 API を使用した [!UICONTROL Recommendations] の管理

[!UICONTROL Recommendations] 用の [!UICONTROL Target] 管理 API と配信 API を設定および使用する方法については、[[!UICONTROL Recommendations] API の使用 ](../../before-administer/recs-api/overview.md) 実践ガイドを参照してください。
