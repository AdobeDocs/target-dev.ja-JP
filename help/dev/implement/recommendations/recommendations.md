---
keywords: Recommendations、設定、環境設定、業界の垂直方向、互換性のない条件をフィルタリング、デフォルトのホストグループ、サムネベース url、recommendations api トークン、9 ドル
description: '[!UICONTROL Recommendations] で  [!DNL Adobe Target] アクティビティを実装する方法を説明します。'
title: '[!UICONTROL Recommendations] アクティビティの実装方法'
feature: Recommendations
exl-id: af1e8b60-6dbb-451b-aa4f-e167d1800d1c
source-git-commit: 94a4122244065384f487ca9a29dfa1b414168cb8
workflow-type: tm+mt
source-wordcount: '1462'
ht-degree: 21%

---

# [!UICONTROL Recommendations] の計画と実装

[!DNL Adobe Target Recommendations] の計画と実装に役立つ情報です。

>[!NOTE]
>
>この記事に加えて、[Adobe Target ビジネス実践者ガイド ](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=ja){target=_blank} には、[Target Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=ja){target=_blank} に関する詳細が含まれています。

[!UICONTROL Recommendations] で最初の [!DNL Adobe Target] アクティビティを設定する前に、次の手順を実行します。

1. [ ユーザーの行動のキャプチャとレコメンデーションの配信に使用する [!UICONTROL Target]](#implement-target) を web およびモバイルアプリのサーフェスに実装します。
1. ユーザーにレコメンデーションする製品またはコンテンツの [[!UICONTROL Recommendations] カタログを設定 ](#set-up-your-recommendations-catalog) します。
1. [ 行動情報とコンテキストを渡す ](#pass-behavioral-information-and-context) を [!DNL Target Recommendations] に渡して、パーソナライズされたレコメンデーションを提供できるようにします。
1. [ グローバル除外を設定 ](#configure-global-exclusions) します。
1. [[!UICONTROL Recommendations] 設定を構成します ](#configure-recommendations-settings)。
1. （任意） [ 管理 API を使用して [!UICONTROL Recommendations] を管理します ](#administer-recommendations-using-admin-apis)。

## &#x200B;1. [!UICONTROL Target] の実装

[!DNL Target Recommendations] では、Adobe Experience Platform Web SDKまたは at.js 0.9.2 （以降）を実装する必要があります。 詳しくは、[[!UICONTROL Target] クライアントサイド実装ガイド ](../client-side/overview.md) 参照してください。

## &#x200B;2. [!UICONTROL Recommendations] カタログを設定する

高品質のレコメンデーションを配信 [!UICONTROL Target] るには、レコメンデーションする製品やコンテンツについて知っておく必要があります。 カタログには通常、推奨項目に関する 3 種類の情報が含まれています。 映画をレコメンデーションしているとします。 以下を含めます。

1. レコメンデーションを受け取るユーザーに表示したいデータ。例えば、ムービーの名前と、ムービーのポスターのサムネール画像の URL を表示できます。
1. マーケティングおよびマーチャンダイジング制御に便利なデータ。例えば、NC-17 ムービーをお勧めしないように、ムービーのレーティングを表示できます。
1. アイテムと他のアイテムとの類似性を判断するのに役立つデータです。 例えば、映画のジャンルや映画の監督を表示できます。

[!UICONTROL Target] では、カタログに入力するための複数の統合オプションを提供しています。 これらのオプションを組み合わせて使用すると、カタログ内の異なる項目を更新したり、異なる頻度で異なる項目属性を更新したりできます。

| メソッド | 概要 | 使用するタイミング | 追加情報 |
| --- | --- | --- | --- |
| カタログフィード | アップロードして毎日取り込むフィード（CSV、Google Product XML または Analytics Product Classifications）のスケジュールを設定します。 | 一度に複数の項目に関する情報を送信する場合。 変更の頻度が低い情報を送信する。 | [ フィード ](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/feeds.html?lang=ja) を参照してください。 |
| エンティティ API | API を呼び出して、1 つの項目の最新の最新情報を送信します。 | 一度に 1 つの項目に関する更新を送信する場合。 頻繁に変更される情報（価格、在庫/在庫レベルなど）の送信。 | 詳しくは、[Entities API 開発者向けドキュメント ](https://developer.adobe.com/target/administer/recommendations-api/#tag/Entities) を参照してください。 |
| ページで更新を渡す | ページ上でJavaScriptを使用するか、配信 API を使用して、1 つの項目に関する最新の情報を送信します。 | 一度に 1 つの項目に関する更新を送信する場合。 頻繁に変更される情報（価格、在庫/在庫レベルなど）の送信。 | 以下の [ 項目表示/製品ページ ](#item-views-or-product-pages) を参照してください。 |

>[!IMPORTANT]
>
>[!DNL Recommendations] から [!UICONTROL Catalog] [!DNL Delivery API] を更新する場合は注意が必要です。 [!DNL Delivery API] は公開されているので、Recommendations カタログのクリック可能な項目への入力には使用しないでください。 無効なコンテンツを追加すると、カタログが汚染される可能性があります。
>
>**ベストプラクティス**:[!DNL Delivery API] は、次のようなカタログ属性の更新にのみ使用します。
>
>* 頻繁に変更する（価格、株価など）。
>
>* Web サイトで簡単に検証できる事前定義済みの形式に従います。
>
>* クリック可能な項目やその他の未検証のコンテンツの追加や変更には使用しないでください。
>
>* 必要に応じて、配信 API を使用してカタログの更新を無効にするようカスタマーサポートにリクエストできます。
>
>詳しくは、[[!UICONTROL Adobe Target Delivery API]](https://developer.adobe.com/target/implement/delivery-api/){target=_blank} ドキュメントを参照してください。

ほとんどのお客様は、1 つ以上のフィードを実装する必要があります。 その後、エンティティ API またはページ上で実行するメソッドを使用して、頻繁に変更される属性や項目の更新で、フィードを補完することを選択できます。

## 3.行動情報とコンテキストの受け渡し

[!UICONTROL Target] に渡す必要がある行動情報とコンテキストは、訪問者が実行しているアクションに応じて異なります。多くの場合、このアクションは、訪問者がやり取りしているページのタイプに関連付けられています。

### 品目ビューまたは製品ページ

製品の詳細ページなど、訪問者が 1 つの項目を表示しているページでは、訪問者が表示している項目の ID を渡す必要があります。 また、訪問者が表示している項目の最も詳細なカテゴリを渡して、現在のカテゴリにレコメンデーションをフィルタリングできるようにします。

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

買い物かごベースの Recommendations について詳しくは、『買い物かごベースのビジネス実践者ガイド [ の ](https://experienceleague.adobe.com/docs/target/using/recommendations/criteria/base-the-recommendation-on-a-recommendation-key.html?lang=ja#cart-based) 買い物かごベース *[!DNL Adobe Target]を参照し* ください。

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

購入イベントが発生したら、購入した品目の ID を渡します。 [at.js のデプロイ方法 ](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)/タグマネージャーを使用しない [ の実装 [!UICONTROL Target] 記事の ](../client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md) コンバージョンの追跡を参照してください。

## &#x200B;4. グローバル除外の設定

訪問者に勧めたくない項目をグローバルレベルで除外します。 『 [ Business Practitioner Guide](https://experienceleague.adobe.com/docs/target/using/recommendations/entities/exclusions.html?lang=ja)Exclusions *[!DNL Adobe Target]』を参照してください*。

## &#x200B;5. [!UICONTROL Recommendations] 設定の指定

設定を使用して [!UICONTROL Recommendations] の実装を管理します。

**[!UICONTROL Recommendations Settings]** のオプションにアクセスするには、[!DNL Adobe Experience Cloud] で Target を開き、**[!UICONTROL Recommendations]**/**[!UICONTROL Settings]** をクリックします。

![Recommendations 設定ページ ](/help/dev/implement/recommendations/assets/recs_settings.png)

以下のオプションがあります。

| 設定 | 説明 |
|--- |--- |
| カスタムグローバル mbox | （オプション）[!UICONTROL Target] アクティビティを提供するために使用するカスタムグローバル mbox を指定します。デフォルトでは、[!UICONTROL Target] で使用されるグローバル mbox が [!UICONTROL Recommendations] に使用されます。<P>メモ：このオプションは [!UICONTROL Target] **[!UICONTROL Administration]** ページで設定します。 [!UICONTROL Target] を開き、**[!UICONTROL Administration]**/**[!UICONTROL Visual Experience Composer]** をクリックします。 |
| 業種 | 業種は、レコメンデーション条件の分類に使用されます。この情報は、チームのメンバーが特定のページに適した条件（買い物かごページやメディアページに最適な条件など）を見つけるのに役立ちます。 |
| 非互換の条件をフィルター | このオプションを選択すると、選択されたページが必要なデータを渡す条件のみが表示されます。すべての条件がすべてのページで正しく実行されるわけではありません。 現在の項目/現在のカテゴリのレコメンデーションに互換性を持たせるには、ページまたは mbox が `entity.id` または `entity.categoryId` を渡す必要があります。 通常は、互換性のある条件のみを表示するようにします。ただし、アクティビティで互換性のない条件を有効にしたい場合は、このオプションのチェックを外します。<P>タグ管理ソリューションを使用している場合は、このオプションを無効にすることをお勧めします。<P>このオプションについて詳しくは、『 [[!UICONTROL Recommendations] Business Practitioner Guide 』の ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-faq/recommendations-faq.html?lang=ja) FAQ *[!DNL Adobe Target]を参照してください* |
| デフォルトホストグループ | デフォルトのホストグループを選択します。<P>ホストグループを使用して、カタログの利用可能な項目をさまざまな用途に分割できます。例えば、ホストグループは開発環境と実稼動環境、さまざまなブランド、またはさまざまな地域に使用できます。デフォルトでは、カタログ検索、コレクションおよび除外のプレビュー結果はデフォルトのホストグループに基づいています。（環境フィルターを使用して、別のホストグループを選択して結果をプレビューすることもできます）。 デフォルトでは、項目の作成または更新時に環境 ID を指定しない限り、新しく追加された項目はすべてのホストグループで使用できます。 配信される Recommendations は、リクエストで指定したホストグループによって異なります。<P>商品が表示されていない場合は、適切なホストグループが使用されていることを確認してください。例えば、ステージング環境を使用するようにレコメンデーションを設定し、ホストグループをステージングに設定した場合、商品を表示するために、ステージング環境のコレクションを再作成する必要がある可能性があります。各環境でどの商品が利用できるかを確認するには、各環境でカタログ検索を利用します。選択した環境（ホストグループ）のコレクション [!UICONTROL Recommendations] 除外のコンテンツをプレビューすることもできます。<P>**注意：** 選択した環境を変更した後、「検索」をクリックして返された結果を更新する必要があります。<P> フィ **[!UICONTROL The Environment]** ターは、Target UI の次の場所から使用できます。<ul><li>カタログ検索（**[!UICONTROL Recommendations]**/**[!UICONTROL Catalog Search]**）</li><li>コレクションを作成ダイアログボックス（**[!UICONTROL Recommendations]**/**[!UICONTROL Collections]**/**[!UICONTROL Create New]**）</li><li>コレクションを更新ダイアログボックス（**[!UICONTROL Recommendations]**/**[!UICONTROL Collections]**/**[!UICONTROL Edit]**）</li><li>除外を作成ダイアログボックス（**[!UICONTROL Recommendations]**/**[!UICONTROL Exclusions]**/**[!UICONTROL Create New]**）</li><li>除外を更新ダイアログボックス（**[!UICONTROL Recommendations]**/**[!UICONTROL Exclusions]**/**[!UICONTROL Edit]**）</li></ul>詳しくは、『 [ Business Practitioner Guide](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html?lang=ja) の *[!DNL Adobe Target]ホスト* を参照してください。 |
| サムネールのベース URL | 商品カタログのベース URL を設定すると、商品のサムネールの指定でサムネール URL を渡す場合に、相対 URL を使用できます。<P>次に例を示します。<P>`"entity.thumbnailURL=/Images/Homepage/product1.jpg"`<P>サムネールのベース URL に対する相対 URL を設定します。 |
| [!UICONTROL Recommendations] API トークン | Download API など、[!UICONTROL Recommendations] の API 呼び出しでこのトークンを使用します。 |

## &#x200B;6. （任意）管理 API を使用した [!UICONTROL Recommendations] の管理

[ 用の [!UICONTROL Recommendations] 管理 API と配信 API を設定および使用する方法については、](../../before-administer/recs-api/overview.md) [!UICONTROL Target] API の使用 [!UICONTROL Recommendations] 実践ガイドを参照してください。
