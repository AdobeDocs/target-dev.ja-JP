---
keywords: Recommendations，設定，環境設定，業種，非互換の条件をフィルター，デフォルトホストグループ，サムネールのベース URL，レコメンデーション API トークン， $9
description: の実装方法を学ぶ [!UICONTROL Recommendations] アクティビティ [!DNL Adobe Target].
title: 実装方法 [!UICONTROL Recommendations] アクティビティ？
feature: Recommendations
exl-id: af1e8b60-6dbb-451b-aa4f-e167d1800d1c
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '1461'
ht-degree: 29%

---

# 計画と実装 [!UICONTROL Recommendations]

計画および実装に役立つ情報 [!DNL Adobe Target Recommendations].

>[!NOTE]
>
>この記事に加えて、 [Adobe Target Business Practioner ガイド](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=ja){target=_blank} contains in-depth information about [Target Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html){target=_blank}.

最初の [!UICONTROL Recommendations] アクティビティ [!DNL Adobe Target]、次の手順を実行します。

1. [実装方法 [!UICONTROL Target]](#implement-target) Web およびモバイルアプリ上で、ユーザーの行動をキャプチャし、レコメンデーションを配信する際に使用するを設定できます。
1. [を設定します。 [!UICONTROL Recommendations] カタログ](#set-up-your-recommendations-catalog) ユーザーにレコメンデーションする製品またはコンテンツの数を指定します。
1. [行動情報とコンテキストを渡す](#pass-behavioral-information-and-context) から [!DNL Target Recommendations] パーソナライズされたレコメンデーションを配信できるようにします。
1. [グローバル除外の設定](#configure-global-exclusions).
1. [設定 [!UICONTROL Recommendations] 設定](#configure-recommendations-settings).
1. （オプション） [管理 [!UICONTROL Recommendations] Admin API の使用](#administer-recommendations-using-admin-apis).

## 1.実装 [!UICONTROL Target]

[!DNL Target Recommendations] では、Adobe Experience Platform Web SDK または at.js 0.9.2（以降）を実装する必要があります。 詳しくは、 [[!UICONTROL Target] クライアント側実装ガイド](../client-side/overview.md) を参照してください。

## 2. [!UICONTROL Recommendations] カタログ

高品質のレコメンデーションを提供するには、 [!UICONTROL Target] は、レコメンデーションする製品またはコンテンツについて把握している必要があります。 通常、カタログには、レコメンデーション品目に関する 3 つのタイプの情報が含まれます。 映画をレコメンデーションするとします。 以下を含めます。

1. レコメンデーションを受け取るユーザーに表示したいデータ。例えば、映画の名前と、映画のポスターのサムネール画像の URL を表示できます。
1. マーケティングおよびマーチャンダイジング制御に便利なデータ。例えば、映画の評価を表示して、NC-17 映画をレコメンデーションしないようにすることができます。
1. 他の品目との品目の類似性を判断するのに役立つデータ。 例えば、映画のジャンルや映画の監督を表示できます。

[!UICONTROL Target] は、カタログに入力するための複数の統合オプションを提供します。 これらのオプションを組み合わせて使用して、カタログ内の異なる項目を更新したり、異なる頻度で異なる項目属性を更新したりできます。

| メソッド | 内容 | 使用するタイミング | 追加情報 |
| --- | --- | --- | --- |
| カタログフィード | フィード (CSV、Google Product XML、Analytics Product Classifications) を毎日アップロードおよび取り込むようにスケジュールします。 | 一度に複数の項目に関する情報を送信する場合。 頻繁に変更されない情報を送信する場合。 | 詳しくは、 [フィード](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html). |
| エンティティ API | API を呼び出して、単一アイテムの最新の更新を送信します。 | 一度に 1 つの項目に関する更新が発生した場合に送信するためのものです。 頻繁に変更される情報（価格、在庫/在庫レベルなど）を送信する場合。 | 詳しくは、 [エンティティ API 開発者向けドキュメント](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities). |
| ページ上で更新を渡す | ページ上の JavaScript を使用するか Delivery API を使用して、単一のアイテムに関する最新の更新を送信します。 | 一度に 1 つの項目に関する更新が発生した場合に送信するためのものです。 頻繁に変更される情報（価格、在庫/在庫レベルなど）を送信する場合。 | 詳しくは、 [品目ビュー/製品ページ](#item-views-or-product-pages) 下 |

ほとんどのお客様は、少なくとも 1 つのフィードを実装する必要があります。 その後、Entities API またはページ上の方法を使用して、頻繁に変更される属性や項目の更新でフィードを補完するよう選択できます。

## 3.行動情報とコンテキストを渡す

に渡す必要のある行動情報とコンテキスト [!UICONTROL Target] は、訪問者がおこなうアクションに依存します。これは、多くの場合、訪問者が操作しているページのタイプに関連しています。

### 品目表示または製品ページ

訪問者が製品の詳細ページなど、単一の品目を表示しているページでは、訪問者が表示している品目の ID を渡す必要があります。 また、現在のカテゴリに対するレコメンデーションをフィルタリングできるように、訪問者が表示する品目の最も詳細なカテゴリを渡す必要があります。

また、製品ページ自体で、すばやく変更された特定の属性を渡すこともできます。 例えば、`value`) および在庫/在庫レベル。

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

カテゴリページでは、レコメンデーションをそのカテゴリ内の製品やコンテンツに制限したい場合が多いでしょう。 これをおこなうには、現在表示されているカテゴリの ID を渡す必要があります。

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

買い物かごページでは、訪問者の現在の買い物かごの内容に基づいて品目をレコメンデーションできます。 そのためには、特別なパラメーターを使用して、訪問者の現在の買い物かごにあるすべての項目の ID を渡します `cartIds`.

#### 現在買い物かごに入っている項目を渡す

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "cartIds": "352,223,23432,432,553"
      }
}
```

買い物かごベースのレコメンデーションについて詳しくは、 [買い物かごベース](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=en#cart-based) （内） *[!DNL Adobe Target]実務者ガイド*.

### 訪問者の買い物かごにすでに入っている品目を除く

サイト全体のページで、レコメンデーションから一部の品目を除外できます。 例えば、訪問者の現在の買い物かごに既に含まれている品目をレコメンデーションしたくない場合があります。 それには、特別なパラメーターを使用して、除外するすべての項目の ID を渡します `excludedIds`.

#### 除外する項目を渡す

```js {line-numbers="true"}
function targetPageParams() {
   return {
      "excludedIds": "352,223,23432,432,553"
      }
}
```

### 購入/注文確認ページ

購入イベントが発生したら、購入した品目の ID を渡します。 詳しくは、 [コンバージョンの追跡](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions) ( [at.js のデプロイ方法/実装 [!UICONTROL Target] タグマネージャーなし](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md) 記事。

## 4.グローバル除外を設定する

訪問者にレコメンデーションしたくないグローバルレベルの品目を除外します。 詳しくは、 [除外](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/exclusions.html) （内） *[!DNL Adobe Target]実務者ガイド*.

## 5.設定 [!UICONTROL Recommendations] 設定

設定を使用して [!UICONTROL Recommendations] の実装を管理します。

**[!UICONTROL Recommendations 設定]**&#x200B;オプションにアクセスするには、 で [!DNL Adobe Experience Cloud]Target を開き、**[!UICONTROL レコメンデーション]**／**[!UICONTROL 設定]**&#x200B;をクリックします。

![代替画像](assets/recs_settings.png)

以下のオプションがあります。

| 設定 | 説明 |
|--- |--- |
| カスタムグローバル mbox | （オプション）[!UICONTROL Target] アクティビティを提供するために使用するカスタムグローバル mbox を指定します。デフォルトでは、[!UICONTROL Target] によって使用されるグローバル mbox が [!UICONTROL Recommendations] に使用されます。<P>注意：このオプションは、 [!UICONTROL Target] **[!UICONTROL 管理]** ページに貼り付けます。 開く [!UICONTROL Target]を選択し、次に **[!UICONTROL 管理]** > **[!UICONTROL Visual Experience Composer]**. |
| 業種 | 業種は、レコメンデーション条件の分類に使用されます。この情報は、買い物かごページやメディアページに最適な条件など、特定のページに適した条件をチームのメンバーが見つけるのに役立ちます。 |
| 非互換の条件をフィルター | このオプションを選択すると、選択されたページが必要なデータを渡す条件のみが表示されます。すべてのページですべての条件が正しく実行されるわけではありません。 ページまたは mbox にが渡される必要がある `entity.id` または `entity.categoryId` 現在の品目/現在のカテゴリのレコメンデーションと互換性を持たせるため。 通常は、互換性のある条件のみを表示するようにします。ただし、アクティビティで互換性のない条件を有効にしたい場合は、このオプションのチェックを外します。<P>タグ管理ソリューションを使用している場合は、このオプションを無効にすることをお勧めします。<P>このオプションについて詳しくは、 [[!UICONTROL Recommendations] FAQ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html) （内） *[!DNL Adobe Target]実務者ガイド*. |
| デフォルトホストグループ | デフォルトホストグループを選択します。<P>ホストグループを使用して、カタログの利用可能な項目をさまざまな用途に分割できます。例えば、ホストグループは開発環境と実稼動環境、さまざまなブランド、またはさまざまな地域に使用できます。デフォルトでは、カタログ検索、コレクションおよび除外のプレビュー結果はデフォルトのホストグループに基づいています。（環境フィルターを使用して、結果をプレビューする別のホストグループを選択することもできます）デフォルトでは、項目の作成または更新時に環境 ID が指定されている場合を除き、新しく追加された項目はすべてのホストグループで使用できます。配信される Recommendations は、リクエストで指定したホストグループによって異なります。<P>商品が表示されていない場合は、適切なホストグループが使用されていることを確認してください。例えば、ステージング環境を使用するようにレコメンデーションを設定し、ホストグループをステージングに設定した場合、商品を表示するために、ステージング環境のコレクションを再作成する必要がある可能性があります。各環境でどの商品が利用できるかを確認するには、各環境でカタログ検索を利用します。また、 [!UICONTROL Recommendations] 選択した環境（ホストグループ）のコレクションと除外。<P>**注意：** 選択した環境を変更した後、 「検索」をクリックして返された結果を更新する必要があります。<P> **[!UICONTROL 環境フィルターは、Target UI の次の場所から利用可能です。]**<ul><li>カタログ検索 (**[!UICONTROL Recommendations]** > **[!UICONTROL カタログ検索]**)</li><li>コレクションを作成ダイアログボックス (**[!UICONTROL Recommendations]** > **[!UICONTROL コレクション]** > **[!UICONTROL 新規作成]**)</li><li>コレクションを更新ダイアログボックス (**[!UICONTROL Recommendations]** > **[!UICONTROL コレクション]** > **[!UICONTROL 編集]**)</li><li>「除外を作成」ダイアログボックス (**[!UICONTROL Recommendations]** > **[!UICONTROL 除外]** > **[!UICONTROL 新規作成]**)</li><li>「除外を更新」ダイアログボックス (**[!UICONTROL Recommendations]** > **[!UICONTROL 除外]** > **[!UICONTROL 編集]**)</li></ul>詳しくは、 [ホスト](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html) （内） *[!DNL Adobe Target]実務者ガイド*. |
| サムネールのベース URL | 商品カタログのベース URL を設定すると、商品のサムネールの指定でサムネール URL を渡す場合に、相対 URL を使用できます。<P>次に例を示します。<P>`"entity.thumbnailURL=/Images/Homepage/product1.jpg"`<P>サムネールのベース URL に対する相対 URL を設定します。 |
| [!UICONTROL Recommendations API トークン] | このトークンをで使用 [!UICONTROL Recommendations] API 呼び出し（ダウンロード API など）。 |

## 6. （オプション）管理 [!UICONTROL Recommendations] Admin API の使用

詳しくは、 [用途 [!UICONTROL Recommendations] API](../../before-administer/recs-api/overview.md) 設定方法と使用方法を学ぶための実践ガイド [!UICONTROL Target] の管理 API と配信 API [!UICONTROL Recommendations].
