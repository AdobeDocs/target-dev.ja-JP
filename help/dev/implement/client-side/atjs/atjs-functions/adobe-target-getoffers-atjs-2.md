---
keywords: adobe.target.getOffers, getOffers, getoffers, get offers, at.js, functions, function, $8
description: '[!UICONTROL adobe.target.getOffers （） &#x200B;]関数とその [!DNL Adobe Target] at.js ライブラリのオプションを使用して、複数の [!DNL Target]  オファーを取得するリクエストを実行します。 (at.js 2.x)'
title: '[!UICONTROL adobe.target.getOffers （） &#x200B;]関数の使用方法を教えてください。'
feature: at.js
exl-id: b96a3018-93eb-49e7-9aed-b27bd9ae073a
TQID: https://experienceleague.adobe.com/jJXcWyQzJ48GNCNcOT165vxcO-CLExTj-t-3kbR2FZ0
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c2be0313-b3ae-45e0-b454-d20bf54b23f2
source-git-commit: 4d0e7f9f2887db71229061fa64b2633a84c6d054
workflow-type: tm+mt
source-wordcount: 1357
ht-degree: 88%

---

# [!UICONTROL adobe.target.getOffers （） &#x200B;] - at.js 2.x

この関数を使用すると、複数の mbox を渡すことで複数のオファーを取得できます。 さらに、アクティブなアクティビティのすべてのビュー向けに複数のオファーを取得できます。

>[!NOTE]
>
>この関数は、at.js 2.xで導入されました。 この関数は、at.js バージョン 1.*x*&#x200B;では使用できません。

| キー | タイプ | 必須？ | 説明 |
| --- | --- | --- | --- |
| `consumerId` | 文字列 | × | 指定しない場合、デフォルト値はクライアントのグローバル mbox です。 このキーは、A4T 統合に使用される追加のデータ ID （SDID）を生成するために使用されます。<P>`getOffers()` を使用する場合、各呼び出しで新しい SDID が生成されます。 同じページに複数の mbox リクエストがあり、SDID を保持する（target-global-mbox と [!DNL Adobe Analytics] の SDID と一致するように）場合は、`consumerId` パラメーターを使用します。<P>`getOffers()` に 3 つの mbox （「mbox1」、「mbox2」、「mbox3」という名前）が含まれている場合は、`getOffers()` 呼び出しに `consumerId: "mbox1, mbox2, mbox3"` を含めます。 |
| `decisioningMethod` | 文字列 | × | &quot;server-side&quot;、&quot;on-device&quot;、&quot;hybrid&quot; |
| `request` | オブジェクト | ○ | 下の「リクエスト」の表を参照してください。 |
| `timeout` | 数値 | × | リクエストのタイムアウト。 指定しない場合、at.js のデフォルトのタイムアウトが使用されます。 |

## リクエスト

>[!NOTE]
>
>以下に示すすべてのフィールドで使用可能なタイプについて詳しくは、[&#x200B; 配信 API ドキュメント &#x200B;](/help/dev/implement/delivery-api/overview.md) を参照してください。

| フィールド名 | 必須？ | 制限事項 | 説明 |
| --- | --- | --- | --- |
| request > id | × |  | `tntId`、`thirdPartyId`、または `marketingCloudVisitorId` のいずれか 1 つが必須です。 |
| Request > id > thirdPartyId | × | 最大サイズ = 128。 |  |
| Request > experienceCloud | × |  |  |
| Request > experienceCloud > analytics | × |  | Adobe Analytics の統合 |
| Request > experienceCloud > analytics > logging | × | 以下をページに実装する必要があります。<ul><li>訪問者 ID サービス</li><li>Appmeasurement.js</li></ul> | 次の値がサポートされています。<P>**client_side**：指定すると、Analytics ペイロードが呼び出し元に返されます。このペイロードは、[!UICONTROL Data Insertion API]を介して[!UICONTROL Adobe Analytics]に送信するために使用されます。<P>**server_side**： これはデフォルト値で、[!DNL Target]と[!DNL Analytics] バックエンドが SDID を使用してレポート目的で呼び出しをステッチします。 |
| Request > prefetch | × |  |  |
| Request > prefetch > views | × | 最大数は 50 です。<P>名前が空白ではありません。<P>名前の長さ `<=`128 です。<P>値の長さ `<=`5000。<P>名前を「profile」で始めないでください。<P>使用できない名前：「orderId」、「orderTotal」、「productPurchasedId」 | アクティブなアクティビティで関連するビューを取得するために使用するパラメーターを渡します。 |
| Request > prefetch > views > profileParameters | × | 最大値は 50 です。<P>名前が空白ではありません。<P>名前の長さ `<=`128 です。<P>値の長さ `<=`5000。<P>文字列値のみを使用できます。<P>名前を「profile」で始めないでください。 | アクティブなアクティビティで関連するビューを取得するために使用するプロファイルパラメーターを渡します。 |
| Request > prefetch > views > product | × |  |  |
| Request > prefetch > views > product -> id | × | 空白ではありません。<P>最大サイズ = 128。 | アクティブなアクティビティで関連するビューを取得するために使用する製品 ID を渡します。 |
| Request > prefetch > views > product > categoryId | × | 空白ではありません。<P>最大サイズ = 128。 | アクティビティ内の関連するビューを取得するために使用する製品カテゴリ ID を渡します。 |
| Request > prefetch > views > order | × |  |  |
| Request > prefetch > views > order > id | × | 最大長= 250。 | アクティブなアクティビティで関連するビューを取得するために使用する注文 ID を渡します。 |
| Request > prefetch > views > order > total | × | 合計 `>=` 0。 | アクティブなアクティビティで関連するビューを取得するために使用する注文の合計を渡します。 |
| Request > prefetch > views > order > purchasedProductIds | × | 空白の値はありません。<P>各値の最大長は 50 です。<P>コンマで連結および区切ります。<P>商品 ID の長さ `<=` 合計 250。 | アクティブなアクティビティで関連するビューを取得するために使用する 購入製品の ID を渡します。 |
| Request > execute | × |  |  |
| Request > execute > pageLoad | × |  |  |
| Request > execute > pageLoad > parameters | × | 最大数は 50 です。<P>名前が空白ではありません。<P>名前の長さ `<=`128 です。<P>値の長さ `<=`5000。<P>文字列値のみを使用できます。<P>名前を「profile」で始めないでください。<P>使用できない名前：「orderId」、「orderTotal」、「productPurchasedId」。 | ページ読み込み時に、指定されたパラメーターを使用してオファーを取得します。 |
| Request > execute > pageLoad > profileParameters | × | 最大数は 50 です。<P>名前が空白ではありません。<P>名前の長さ `<=`128 です。<P>値の長さ `<=`256。<P>名前を「profile」で始めないでください。<P>文字列値のみを使用できます。 | ページ読み込み時に、指定されたプロファイルパラメーターを使用してオファーを取得します。 |
| Request > execute > pageLoad > product | × |  |  |
| Request > execute > pageLoad > product -> id | × | 空白ではありません。<P>最大サイズ = 128。 | ページ読み込み時に、指定された製品 ID を使用してオファーを取得します。 |
| Request > execute > pageLoad > product > categoryId | × | 空白ではありません。<P>最大サイズ = 128。 | ページ読み込み時に、指定された製品カテゴリー ID を使用してオファーを取得します。 |
| Request > execute > pageLoad > order | × |  |  |
| Request > execute > pageLoad > order > id | × | 最大長= 250。 | ページ読み込み時に、指定された注文 ID を使用してオファーを取得します。 |
| Request > execute > pageLoad > order > total | × | 0`>=`。 | ページ読み込み時に、指定された注文の合計を使用してオファーを取得します。 |
| Request > execute > pageLoad > order > purchasedProductIds | × | 空白の値はありません。<P>各値の最大長は 50 です。<P>コンマで連結および区切ります。<P>商品 ID の長さ `<=` 合計 250。 | ページ読み込み時に、指定された購入 ID を使用してオファーを取得します。 |
| Request > execute > mboxes | × | 最大サイズ = 50。<P>null 要素がありません。 |  |
| Request > execute > mboxes>mbox | ○ | 空白ではありません。<P>「– クリック」サフィックスはありません。<P>最大サイズ = 250。<P>使用可能な文字：`'-, ._\/=:;&!@#$%^&*()_+|?~[]{}'`\|mboxの名前。 |
| Request > execute > mboxes>mbox>index | ○ | null ではありません。<P>一意。<P>0`>=`。 | 注意： インデックスは、mbox が処理される順序を表すものではありません。 複数のリージョナル mbox を持つ Web ページと同様、mbox が処理される順序は指定できません。 |
| Request > execute > mboxes > mbox > parameters | × | 最大数= 50。<P>名前が空白ではありません。<P>名前の長さ `<=`128 です。<P>文字列値のみを使用できます。<P>値の長さ `<=`5000。<P>名前を「profile」で始めないでください。<P>使用できない名前：「orderId」、「orderTotal」、「productPurchasedId」。 | 指定されたパラメーターを使用して特定の mbox のオファーを取得します。 |
| Request > execute > mboxes>mbox>profileParameters | × | 最大数= 50。<P>名前が空白ではありません。<P>名前の長さ `<=`128 です。<P>文字列値のみを使用できます。<P>値の長さ `<=`256。<P>名前を「profile」で始めないでください。 | 指定されたプロファイルパラメーターを使用して特定の mbox のオファーを取得します。 |
| Request > execute > mboxes>mbox > product | × |  |  |
| Request > execute > mboxes > mbox > product > id | × | 空白ではありません。<P>最大サイズ = 128。 | 指定された製品 ID を使用して特定の mbox のオファーを取得します。 |
| Request > execute > mboxes > mbox > product > categoryId | × | 空白ではありません。<P>最大サイズ = 128。 | 指定された製品カテゴリー ID を使用して特定の mbox のオファーを取得します。 |
| Request > execute > mboxes > mbox > order | × |  |  |
| Request > execute > mboxes>mbox > order > id | × | 最大長= 250。 | 指定された注文 ID を持つ特定の mbox のオファーを取得します。 |
| Request > execute > mboxes > mbox > order > total | × | 0`>=`。 | 指定された注文合計を持つ特定の mbox のオファーを取得します。 |
| Request > execute > mboxes > mbox > order > purchasedProductIds | × | 空白の値はありません。<P>各値の最大長= 50。<P>コンマで連結および区切ります。<P>製品 ID の全長 `<=`250 です。 | 指定された注文で購入された製品 ID を持つ特定の mbox のオファーを取得します。 |

## すべてのビューについて[!UICONTROL getOffers （） &#x200B;]を呼び出します

```javascript {line-numbers="true"}
adobe.target.getOffers({
    request: {
      prefetch: {
        views: [{}]
    }
  }
});
```

## [!UICONTROL getOffers （） &#x200B;]を呼び出して、デバイス上で決定します

```javascript {line-numbers="true"}
adobe.target.getOffers({ 

  decisioningMethod:"on-device", 
  request: { 
    execute: { 
      mboxes: [ 
        { 
          index: 0, 
          name: "homepage" 
        } 
      ] 
    } 
 } 
}); 
```

## [!UICONTROL getOffers （） &#x200B;]を呼び出して、渡されたパラメーターとプロファイル パラメーターを使用して最新のビューを取得します

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    "prefetch": {
      "views": [
        {
          "parameters": {
            "ad": "1"
          },
          "profileParameters": {
            "age": 23
          }
        }
      ]
    }
  }
});
```

## [!UICONTROL getOffers （） &#x200B;]を呼び出して、パラメーターとプロファイルパラメーターが渡されたmboxを取得します。

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    execute: {
      mboxes: [
        {
          index: 0,
          name: "first-mbox"
        },
        {
          index: 1,
          name: "second-mbox",
          parameters: {
            a: 1
          },
          profileParameters: {
            b: 2
          }
        }
      ]
    }
  }
});
```

## [!UICONTROL getOffers （） &#x200B;]を呼び出して、クライアントサイドから分析ペイロードを取得します

```javascript {line-numbers="true"}
adobe.target.getOffers({
      request: {
        experienceCloud: {
          analytics: {
            logging: "client_side"
          }
        },
        prefetch: {
          mboxes: [{
            index: 0,
            name: "a1-serverside-xt"
          }]
        }
      }
    })
    .then(console.log)
```

**応答**：

```javascript {line-numbers="true"}
{
  "prefetch": {
    "mboxes": [{
      "index": 0,
      "name": "a1-serverside-xt",
      "options": [{
        "content": "<img src=\"http://s7d2.scene7.com/is/image/TargetAdobeTargetMobile/L4242-xt-usa?tm=1490025518668&fit=constrain&hei=491&wid=980&fmt=png-alpha\"/>",
        "type": "html",
        "eventToken": "n/K05qdH0MxsiyH4gX05/2qipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
        "responseTokens": {
          "profile.memberlevel": "0",
          "geo.city": "bucharest",
          "activity.id": "167169",
          "experience.name": "USA Experience",
          "geo.country": "romania"
        }
      }],
      "analytics": {
        "payload": {
          "pe": "tnt",
          "tnta": "167169:0:0|0|100,167169:0:0|2|100,167169:0:0|1|100"
        }
      }
    }]
  }
}
```

その後、ペイロードは [&#x200B; Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) を介して [!DNL Adobe Analytics] に転送できます。

## [!UICONTROL getOffers （） &#x200B;]および[!UICONTROL applyOffers （） &#x200B;]を介して、複数のmboxからデータを取得してレンダリングします

at.js 2.x を使用すると、`[!UICONTROL getOffers()]` API を使用して複数の mbox を取得できます。 複数の mbox のデータを取得して、`[!UICONTROL applyOffers()]` CSS セレクターによって識別されるさまざまな場所で、データをレンダリングすることもできます。

次の例は、at.js 2.x を実装した単純な HTML ページを示しています。

```html {line-numbers="true"}
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>at.js 2.x, multiple selectors and multiple mboxes</title>
  <script src="at.js"></script>
</head>
<body>
  <div id="container1">Default content 1</div>
  
  <div id="container2">Default content 2</div>

  <div id="container3">Default content 3</div>
</body>
</html>
```

[!DNL Target] から受け取ったコンテンツを介して変更したい 3 つのコンテナがあるとします。 3 つの mbox に対して 1 つのリクエストを作成し、それぞれの mbox に、それぞれのコンテナにレンダリングするためのコンテンツを含めることができます。

リクエストおよびレンダリングコードは次の例のようになります。

```javascript {line-numbers="true"}
adobe.target.getOffers({
  request: {
    prefetch: {
      mboxes: [
        {
          index: 0,
          name: "mbox1"
        },
        {
          index: 1,
          name: "mbox2"
        },
        {
          index: 2,
          name: "mbox3"
        }
      ]
    }
  }
})
.then(response => {
  // get all mboxes from response
  const mboxes = response.prefetch.mboxes;
  let count = 1;

  mboxes.forEach(el => {
    adobe.target.applyOffers({
      selector: "#container" + count,
      response: {
        prefetch: {
          mboxes: [el]
        }
      }
    });

    count += 1;
  });
});
```

`request > prefetch > mboxes` セクションでは、3 つの異なる mbox があります。 リクエストが正常に完了した場合は、それぞれの mbox に対する応答を `response > prefetch > mboxes` から受信します。 レスポンスとレンダリングに使用する場所が決まったら、`applyOffers()` を呼び出して [!DNL Target] から取得したコンテンツをレンダリングできます。 この例では、次のマッピングがあります。

* mbox1／CSS セレクター# container1
* mbox2／CSS セレクター# container2
* mbox3／CSS セレクター# container3

この例では、count 変数を使用して CSS セレクターを作成します。 実際のシナリオでは、CSS セレクターと mbox との異なるマッピングを使用できます。

この例では `prefetch > mboxes` を使用していますが、`execute > mboxes` を使用することもできます。 `getOffers()` でプリフェッチを使用する場合は、`applyOffers()` 呼び出しでもプリフェッチを使用する必要があります。

## [!UICONTROL getOffers （） &#x200B;]を呼び出してpageLoadを実行します

次の例は、at.js 2.*x*&#x200B;で[!UICONTROL getOffers （） &#x200B;]を使用してpageLoadを実行する方法を示しています

```javascript {line-numbers="true"}
adobe.target.getOffers({
    request: {
        execute: {
            pageLoad: {
                parameters: {}
            }
        }
    }
});
```

