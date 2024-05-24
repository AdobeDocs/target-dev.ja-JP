---
keywords: Recommendations，設定，環境設定，業界の垂直性，互換性のない条件をフィルター，デフォルトのホストグループ，thumb base url, recommendations api トークン，9 ドル
description: 実装方法を学ぶ [!UICONTROL Recommendations] でのアクティビティ [!DNL Adobe Target].
title: 実装方法 [!UICONTROL Recommendations] 活動？
feature: Recommendations
exl-id: af1e8b60-6dbb-451b-aa4f-e167d1800d1c
source-git-commit: 777c8b9aeb866e1b20ec24e85532a99f57d1fe72
workflow-type: tm+mt
source-wordcount: '1365'
ht-degree: 25%

---

# プランと実装 [!UICONTROL Recommendations]

計画および実装に役立つ情報 [!DNL Adobe Target Recommendations].

>[!NOTE]
>
>この記事に加えて、 [Adobe Target実務担当者ガイド](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=ja){target=_blank} に関する詳細な情報を含む [Target Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html){target=_blank}.

最初のの設定を行う前に [!UICONTROL Recommendations] でのアクティビティ [!DNL Adobe Target]の場合は、次の手順を実行します。

1. [実装方法 [!UICONTROL Target]](#implement-target) ユーザーの行動のキャプチャとレコメンデーションの配信に使用する web およびモバイルアプリのサーフェス上。
1. [の設定 [!UICONTROL Recommendations] カタログ](#set-up-your-recommendations-catalog) ユーザーにお勧めする製品またはコンテンツの。
1. [行動情報とコンテキストの受け渡し](#pass-behavioral-information-and-context) 対象： [!DNL Target Recommendations] パーソナライズされたレコメンデーションを配信できるようにします。
1. [グローバル除外の設定](#configure-global-exclusions).
1. [設定 [!UICONTROL Recommendations] 設定](#configure-recommendations-settings).
1. （オプション） [の管理 [!UICONTROL Recommendations] 管理 API の使用](#administer-recommendations-using-admin-apis).

## 1.実装 [!UICONTROL Target]

[!DNL Target Recommendations] Adobe Experience Platform Web SDK または at.js 0.9.2 （以降）を実装する必要があります。 を参照してください。 [[!UICONTROL Target] クライアントサイド実装ガイド](../client-side/overview.md) を参照してください。

## 2.の設定 [!UICONTROL Recommendations] カタログ

高品質のレコメンデーションを提供するには、 [!UICONTROL Target] 推奨する製品やコンテンツについて知っている必要があります。 カタログには通常、推奨項目に関する 3 種類の情報が含まれています。 映画をレコメンデーションしているとします。 以下を含めます。

1. レコメンデーションを受け取るユーザーに表示したいデータ。例えば、ムービーの名前と、ムービーのポスターのサムネール画像の URL を表示できます。
1. マーケティングおよびマーチャンダイジング制御に便利なデータ。例えば、NC-17 ムービーをお勧めしないように、ムービーのレーティングを表示できます。
1. アイテムと他のアイテムとの類似性を判断するのに役立つデータです。 例えば、映画のジャンルや映画の監督を表示できます。

[!UICONTROL Target] は、カタログに入力するための複数の統合オプションを提供します。 これらのオプションを組み合わせて使用すると、カタログ内の異なる項目を更新したり、異なる頻度で異なる項目属性を更新したりできます。

| メソッド | 概要 | 使用するタイミング | 追加情報 |
| --- | --- | --- | --- |
| カタログフィード | アップロードして毎日取り込むフィード（CSV、Google Product XML または Analytics Product Classifications）のスケジュールを設定します。 | 一度に複数の項目に関する情報を送信する場合。 変更の頻度が低い情報を送信する。 | 参照： [フィード](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html). |
| エンティティ API | API を呼び出して、1 つの項目の最新の最新情報を送信します。 | 一度に 1 つの項目に関する更新を送信する場合。 頻繁に変更される情報（価格、在庫/在庫レベルなど）の送信。 | を参照してください。 [エンティティ API 開発者向けドキュメント](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities). |
| ページで更新を渡す | ページ上で JavaScript を使用するか、配信 API を使用して、1 つの項目に関する最新の情報を送信します。 | 一度に 1 つの項目に関する更新を送信する場合。 頻繁に変更される情報（価格、在庫/在庫レベルなど）の送信。 | 参照： [品目表示/製品ページ](#item-views-or-product-pages) 下。 |

ほとんどのお客様は、1 つ以上のフィードを実装する必要があります。 その後、エンティティ API またはページ上で実行するメソッドを使用して、頻繁に変更される属性や項目の更新で、フィードを補完することを選択できます。

## 3.行動情報とコンテキストの受け渡し

に渡す行動情報とコンテキスト [!UICONTROL Target] 訪問者が実行しているアクションに依存します。多くの場合、アクションは、訪問者がやり取りするページのタイプに関連付けられています。

### 品目ビューまたは製品ページ

製品の詳細ページなど、訪問者が 1 つの項目を表示しているページでは、訪問者が表示している項目の ID を渡す必要があります。 また、訪問者が表示している項目の最も詳細なカテゴリを渡して、現在のカテゴリにレコメンデーションをフィルタリングできるようにします。

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

買い物かごベースの推奨事項について詳しくは、を参照してください。 [買い物かごベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=en#cart-based) が含まれる *[!DNL Adobe Target]実務担当者ガイド*.

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

訪問者に勧めたくない項目をグローバルレベルで除外します。 参照： [除外数](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/exclusions.html) が含まれる *[!DNL Adobe Target]実務担当者ガイド*.

## 5.の設定 [!UICONTROL Recommendations] 設定

設定を使用して [!UICONTROL Recommendations] の実装を管理します。

にアクセスするには **[!UICONTROL Recommendations Settings]** オプションを選択し、Target を [!DNL Adobe Experience Cloud]を選択し、 **[!UICONTROL Recommendations]** > **[!UICONTROL Settings]**.

![Recommendationsの設定ページ](/help/dev/implement/recommendations/assets/recs_settings.png)

以下のオプションがあります。

| 設定 | 説明 |
|--- |--- |
| カスタムグローバル mbox | （オプション）[!UICONTROL Target] アクティビティを提供するために使用するカスタムグローバル mbox を指定します。デフォルトでは、によって使用されるグローバル mbox は [!UICONTROL Target] は、次の場合に使用されます [!UICONTROL Recommendations].<P>メモ：このオプションは [!UICONTROL Target] **[!UICONTROL Administration]** ページ。 開く [!UICONTROL Target]を選択し、 **[!UICONTROL Administration]** > **[!UICONTROL Visual Experience Composer]**. |
| 業種 | 業種は、レコメンデーション条件の分類に使用されます。この情報は、チームのメンバーが特定のページに適した条件（買い物かごページやメディアページに最適な条件など）を見つけるのに役立ちます。 |
| 非互換の条件をフィルター | このオプションを選択すると、選択されたページが必要なデータを渡す条件のみが表示されます。すべての条件がすべてのページで正しく実行されるわけではありません。 ページまたは mbox が渡される必要があります。 `entity.id` または `entity.categoryId` 現在の項目/現在のカテゴリのレコメンデーションに互換性を持たせるために。 通常は、互換性のある条件のみを表示するようにします。ただし、アクティビティで互換性のない条件を有効にしたい場合は、このオプションのチェックを外します。<P>タグ管理ソリューションを使用している場合は、このオプションを無効にすることをお勧めします。<P>このオプションの詳細については、を参照してください [[!UICONTROL Recommendations] FAQ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html) が含まれる *[!DNL Adobe Target]実務担当者ガイド*. |
| デフォルトホストグループ | デフォルトのホストグループを選択します。<P>ホストグループを使用して、カタログの利用可能な項目をさまざまな用途に分割できます。例えば、ホストグループは開発環境と実稼動環境、さまざまなブランド、またはさまざまな地域に使用できます。デフォルトでは、カタログ検索、コレクションおよび除外のプレビュー結果はデフォルトのホストグループに基づいています。（環境フィルターを使用して、結果をプレビューする別のホストグループを選択することもできます）デフォルトでは、項目の作成または更新時に環境 ID が指定されている場合を除き、新しく追加された項目はすべてのホストグループで使用できます。配信される Recommendations は、リクエストで指定したホストグループによって異なります。<P>商品が表示されていない場合は、適切なホストグループが使用されていることを確認してください。例えば、ステージング環境を使用するようにレコメンデーションを設定し、ホストグループをステージングに設定した場合、商品を表示するために、ステージング環境のコレクションを再作成する必要がある可能性があります。各環境でどの商品が利用できるかを確認するには、各環境でカタログ検索を利用します。のコンテンツをプレビューすることもできます。 [!UICONTROL Recommendations] 選択した環境（ホストグループ）のコレクションと除外。<P>**注意：** 選択した環境を変更した後、「検索」をクリックして返された結果を更新する必要があります。<P> **[!UICONTROL The Environment]** フィルターは、Target UI の次の場所から使用できます。<ul><li>カタログ検索（**[!UICONTROL Recommendations]** > **[!UICONTROL Catalog Search]**）</li><li>コレクションを作成ダイアログボックス（**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Create New]**）</li><li>コレクションを更新ダイアログボックス（**[!UICONTROL Recommendations]** > **[!UICONTROL Collections]** > **[!UICONTROL Edit]**）</li><li>除外を作成ダイアログボックス（**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Create New]**）</li><li>除外を更新ダイアログボックス（**[!UICONTROL Recommendations]** > **[!UICONTROL Exclusions]** > **[!UICONTROL Edit]**）</li></ul>詳しくは、を参照してください [ホスト](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html) が含まれる *[!DNL Adobe Target]実務担当者ガイド*. |
| サムネールのベース URL | 商品カタログのベース URL を設定すると、商品のサムネールの指定でサムネール URL を渡す場合に、相対 URL を使用できます。<P>次に例を示します。<P>`"entity.thumbnailURL=/Images/Homepage/product1.jpg"`<P>サムネールのベース URL に対する相対 URL を設定します。 |
| [!UICONTROL Recommendations] API トークン | このトークンを次で使用 [!UICONTROL Recommendations] API 呼び出し（Download API など）。 |

## 6. （任意）の管理 [!UICONTROL Recommendations] 管理 API の使用

を参照してください。 [使用方法 [!UICONTROL Recommendations] API](../../before-administer/recs-api/overview.md) を設定して使用する方法を説明する実践ガイド [!UICONTROL Target] の管理および配信 API [!UICONTROL Recommendations].
