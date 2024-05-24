---
keywords: Recommendations，設定，環境設定，業種，互換性のない条件をフィルター，デフォルトのホストグループ，thumb base url, recommendations api トークン，
description: 実装方法を学ぶ [!UICONTROL Recommendations] でのアクティビティ [!DNL Adobe Target].
title: 実装方法 [!UICONTROL Recommendations] 活動？
feature: Recommendations
hidefromtoc: true
hide: true
source-git-commit: 8d04fe249aafb5ee064dc614ae72f68b8f8459f2
workflow-type: tm+mt
source-wordcount: '1550'
ht-degree: 20%

---

# プランと実装 [!UICONTROL Recommendations]

計画および実装に役立つ情報 [!DNL Adobe Target Recommendations].

>[!NOTE]
>
>この記事に加えて、 [Adobe Target実務担当者ガイド](https://experienceleague.adobe.com/en/docs/target/using/target-home){target=_blank} に関する詳細な情報を含む [Target Recommendations](https://experienceleague.adobe.com/en/docs/target/using/recommendations/recommendations){target=_blank}.

最初のの設定を行う前に [!UICONTROL Recommendations] でのアクティビティ [!DNL Adobe Target]の場合は、次の手順を実行します。

1. [実装方法 [!UICONTROL Target]](#implement-target) ユーザーの行動のキャプチャとレコメンデーションの配信に使用する web およびモバイルアプリのサーフェス上。
1. [の設定 [!UICONTROL Recommendations] カタログ](#set-up-your-recommendations-catalog) ユーザーにお勧めする製品またはコンテンツの。
1. [行動情報とコンテキストの受け渡し](#pass-behavioral-information-and-context) 対象： [!DNL Target Recommendations] パーソナライズされたレコメンデーションを配信できるようにします。
1. [グローバル除外の設定](#configure-global-exclusions).
1. [設定 [!UICONTROL Recommendations] 設定](#configure-recommendations-settings).
1. （オプション） [の管理 [!UICONTROL Recommendations] 管理 API の使用](#administer-recommendations-using-admin-apis).

## 1.実装 [!UICONTROL Target]

[!DNL Target Recommendations] を実装する必要があります [!DNL Adobe Experience Platform Web SDK] または at.js 0.9.2 （またはそれ以降） を参照してください。 [[!UICONTROL Target] クライアントサイド実装ガイド](../client-side/overview.md) を参照してください。

## 2.の設定 [!UICONTROL Recommendations] カタログ

高品質のレコメンデーションを提供するには、 [!UICONTROL Target] 推奨する製品やコンテンツについて知っている必要があります。 カタログには通常、推奨項目に関する 3 種類の情報が含まれています。 映画をレコメンデーションしているとします。 以下を含めます。

1. レコメンデーションを受け取るユーザーに表示したいデータ。例えば、ムービーの名前と、ムービーのポスターのサムネール画像の URL を表示できます。
1. マーケティングおよびマーチャンダイジング制御に便利なデータ。例えば、NC-17 ムービーをお勧めしないように、ムービーのレーティングを表示できます。
1. アイテムと他のアイテムとの類似性を判断するのに役立つデータです。 例えば、映画のジャンルや映画の監督を表示できます。

[!UICONTROL Target] は、カタログに入力するための複数の統合オプションを提供します。 これらのオプションを組み合わせて使用すると、カタログ内の異なる項目を更新したり、異なる頻度で異なる項目属性を更新したりできます。

| メソッド | 概要 | 使用するタイミング | 追加情報 |
| --- | --- | --- | --- |
| カタログフィード | フィードのスケジュール （CSV、 [!DNL Google] 製品 XML、または [!UICONTROL Analytics Product Classifications]）をアップロードし、毎日取り込みます。 | 一度に複数の項目に関する情報を送信する場合。 変更の頻度が低い情報を送信する。 | 参照： [フィード](https://experienceleague.adobe.com/en/docs/target/using/recommendations/entities/feeds). |
| エンティティ API | API を呼び出して、1 つの項目の最新の最新情報を送信します。 | 一度に 1 つの項目に関する更新を送信する場合。 頻繁に変更される情報（価格、在庫/在庫レベルなど）の送信。 | を参照してください。 [エンティティ API 開発者向けドキュメント](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities). |
| ページで更新を渡す | ページ上で JavaScript を使用するか、配信 API を使用して、1 つの項目に関する最新の情報を送信します。 | 一度に 1 つの項目に関する更新を送信する場合。 頻繁に変更される情報（価格、在庫/在庫レベルなど）の送信。 | 参照： [品目表示/製品ページ](#item-views-or-product-pages) 下。 |

ほとんどのお客様は、1 つ以上のフィードを実装する必要があります。 その後、エンティティ API またはページ上で実行するメソッドを使用して、頻繁に変更される属性や項目の更新で、フィードを補完することを選択できます。

## 3.行動情報とコンテキストの受け渡し

に渡す行動情報とコンテキスト [!UICONTROL Target] 訪問者が実行しているアクションに依存します。多くの場合、アクションは、訪問者がやり取りするページのタイプに関連付けられています。

### 品目ビューまたは製品ページ

製品の詳細ページなど、訪問者が 1 つの項目を表示しているページでは、訪問者が表示している項目の ID を渡す必要があります。 また、訪問者が表示している項目の最も詳細なカテゴリを渡して、現在のカテゴリに推奨事項をフィルタリングできるようにします。

また、変化の速い特定の属性を製品ページ自体に渡すこともできます。 例えば、price （`value`）および在庫/在庫レベル。

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

買い物かごページでは、訪問者の現在の買い物かごの内容に基づいて、アイテムをレコメンデーションできます。 これを行うには、特別なパラメーターを使用して、訪問者の現在の買い物かごにあるすべての項目の ID を渡します `cartIds`.

#### 現在カートに入っている商品を渡す

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "cartIds": "352,223,23432,432,553"
      }
}
```

買い物かごベースの推奨事項について詳しくは、を参照してください。 [買い物かごベース](https://experienceleague.adobe.com/en/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key#cart-based) が含まれる *[!DNL Adobe Target]実務担当者ガイド*.

### 訪問者の買い物かごにすでに入っている品目を除く

サイト全体のページで、一部の項目をレコメンデーションから除外できます。 例えば、訪問者の現在の買い物かごに既にある項目をお勧めしない場合があります。 これを行うには、除外するすべての項目の ID を特別なパラメーターを使用して渡します `excludedIds`.

#### 除外する項目を渡す

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "excludedIds": "352,223,23432,432,553"
      }
}
```

### 購入/注文確認ページ

購入イベントが発生したら、購入した品目の ID を渡します。 参照： [コンバージョンの追跡](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions) が含まれる [at.js のデプロイ方法/実装 [!UICONTROL Target] タグマネージャーを持たない](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md) 記事。

## 4. グローバル除外の設定

訪問者に勧めたくない項目をグローバルレベルで除外します。 参照： [除外数](https://experienceleague.adobe.com/en/docs/target/using/recommendations/entities/exclusions) が含まれる *[!DNL Adobe Target]実務担当者ガイド*.

## 5.の設定 [!UICONTROL Recommendations] 設定

設定を使用して [!UICONTROL Recommendations] の実装を管理します。

にアクセスするには **[!UICONTROL Recommendations Settings]** オプション、開く [!DNL Target] が含まれる [!DNL Adobe Experience Cloud]を選択し、 **[!UICONTROL Administration]** > **[!UICONTROL Recommendations]**.

![Recommendationsの設定ページ](/help/dev/implement/recommendations/assets/recs-settings-new.png)

次のオプションを設定します。

### [!UICONTROL Recommendations API Token]

では以下のオプションを使用できます。 [!UICONTROL Recommendations API Token] セクション：

#### [!UICONTROL Client code]

この [!DNL Target] [!UICONTROL client code].

自分を知らない場合 [!UICONTROL client code]、内 [!DNL Target] ユーザ インタフェース、クリック **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**. この [!UICONTROL client code] を次に示します [!UICONTROL Account Details] セクション。

#### 認証トークン

この [!DNL Adobe Target] 管理 API （次を含む） [!DNL Recommendations Admin] API は、許可されたユーザーのみがアクセスに使用できるように、認証で保護されています [!DNL Adobe Target]. の使用 [Adobe Developer コンソール](https://developer.adobe.com/console/home) すべてのこの認証を管理するには [!DNL Adobe Experience Cloud solutions]を含む [!DNL Adobe Target].

詳しくは、を参照してください [Adobe Target API の認証の設定](/help/dev/before-administer/configure-authentication.md).

>[!WARNING]
>
>新しい認証トークンを生成する際は注意が必要です。 新しいトークンを生成すると、現在のトークンを使用した API 呼び出しが失敗します。 新しく生成された認証トークンでスクリプトまたはアプリケーションを更新します。

### 条件

サイトの業種を把握することは、Target がレコメンデーションの条件を選択するのに役立ちます。

の条件 [!DNL Recommendations] は、事前に定義されている訪問者の行動に基づいて、どの製品またはコンテンツをレコメンデーションするかを決定するルールです。 条件は、人気のあるトレンド、訪問者の現在および過去の行動、類似の製品およびコンテンツに基づくことができます。 複数の条件を追加することで、複数のレコメンデーションタイプを相互にテストすることができます。

詳しくは、を参照してください [条件](https://experienceleague.adobe.com/en/docs/target/using/recommendations/criteria/algorithms){target=_blank} が含まれる *Adobe Target実務担当者ガイド。*

では以下の設定を使用できます [!UICONTROL Criteria] セクション：

#### [!UICONTROL Industry/Vertical]

業種は、レコメンデーション条件の分類に使用されます。この情報は、チームのメンバーが特定のページに適した条件（買い物かごページやメディアページに最適な条件など）を見つけるのに役立ちます。

ドロップダウンリストでは、以下のカテゴリを使用できます。

* パーソナライゼーションなし
* 小売／e コマース
* リードジェネレーション／B2B／金融サービス
* メディア／投稿

#### [!UICONTROL Filter Incompatible Criteria]

このオプションを選択すると、選択されたページが必要なデータを渡す条件のみが表示されます。すべての条件がすべてのページで正しく実行されるわけではありません。 ページまたは mbox が渡される必要があります。 `entity.id` または `entity.categoryId` 現在の項目/現在のカテゴリのレコメンデーションに互換性を持たせるために。

通常は、互換性のある条件のみを表示するようにします。ただし、互換性のない条件をアクティビティで使用できるようにする場合は、このオプションを有効にしないでください。

Adobeでは、タグ管理ソリューションを使用する場合、このオプションを無効にすることをお勧めします。

このオプションの詳細については、を参照してください [[!UICONTROL Recommendations] FAQ](https://experienceleague.adobe.com/en/docs/target/using/recommendations/recommendations-faq/recommendations-faq){target=_blank} が含まれる *[!DNL Adobe Target]実務担当者ガイド*.

### [!UICONTROL Product Catalog]

では以下のオプションを使用できます。 [!UICONTROL Product Catalog] セクション：

#### [!UICONTROL Default Host Group]

デフォルトのホストグループを選択します。

ホストグループを使用して、カタログの利用可能な項目をさまざまな用途に分割できます。例えば、ホストグループは開発環境と実稼動環境、さまざまなブランド、またはさまざまな地域に使用できます。デフォルトでは、カタログ検索、コレクションおよび除外のプレビュー結果はデフォルトのホストグループに基づいています。（環境フィルターを使用して、結果をプレビューする別のホストグループを選択することもできます）デフォルトでは、項目の作成または更新時に環境 ID が指定されている場合を除き、新しく追加された項目はすべてのホストグループで使用できます。配信される Recommendations は、リクエストで指定したホストグループによって異なります。

商品が表示されていない場合は、適切なホストグループが使用されていることを確認してください。例えば、ステージング環境を使用するようにレコメンデーションを設定し、ホストグループをステージングに設定した場合、商品を表示するために、ステージング環境のコレクションを再作成する必要がある可能性があります。各環境でどの商品が利用できるかを確認するには、各環境でカタログ検索を利用します。のコンテンツをプレビューすることもできます。 [!UICONTROL Recommendations] 選択した環境（ホストグループ）のコレクションと除外。

>[!NOTE]
>
>選択した環境を変更した後、 **[!UICONTROL Search]** 返された結果を更新します。

この **[!UICONTROL Environment]** フィルターは、Target UI の次の場所から使用できます。

* カタログ検索（**[!UICONTROL Recommendations]** > **[!UICONTROL Catalog Search]**）
* コレクション （**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]**）
* コレクションを作成ダイアログボックス（**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Create collection]**）
* コレクションを更新ダイアログボックス（**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Edit]**）
* 除外を作成ダイアログボックス（**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Create exclusion]**）
* 除外を更新ダイアログボックス（**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Edit]**）

詳しくは、を参照してください [ホスト](https://experienceleague.adobe.com/en/docs/target/using/administer/hosts){target=_blank} が含まれる *[!DNL Adobe Target]実務担当者ガイド*.

#### [!UICONTROL Thumbnail Base]

商品カタログのベース URL を設定すると、商品のサムネールの指定でサムネール URL を渡す場合に、相対 URL を使用できます。

次に例を示します。

`"entity.thumbnailURL=/Images/Homepage/product1.jpg"`

サムネールのベース URL に対する相対 URL を設定します。

### [!UICONTROL Custom Attribute Key Configuration]

訪問者のプロファイルに保存された項目に基づいてお勧めを紹介します。 例えば、「最後に買い物かごに追加されたアイテム」、「最後に視聴したビデオが 90% 以上表示されました」などです。

クリック **[!UICONTROL Add]** 新しい設定を作成するには、設定の名前を指定し、目的のプロファイル属性を選択して、をクリックします **[!UICONTROL Save]**.

## 6. （任意）の管理 [!UICONTROL Recommendations] 管理 API の使用

を参照してください。 [使用方法 [!UICONTROL Recommendations] API](../../before-administer/recs-api/overview.md) を設定して使用する方法を説明する実践ガイド [!UICONTROL Target] の管理および配信 API [!UICONTROL Recommendations].