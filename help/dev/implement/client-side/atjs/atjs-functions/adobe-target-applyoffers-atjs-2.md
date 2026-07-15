---
keywords: adobe.target.applyOffers, applyOffers, applyoffers, apply offers, at.js, functions,
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリの[!UICONTROL adobe.target.applyOffers （） &#x200B;]関数を使用して、レスポンスに複数のオファーを適用します。 (at.js 2.x)'
title: '[!UICONTROL adobe.target.applyOffers （） &#x200B;]関数の使用方法を教えてください。'
feature: at.js
exl-id: c391e3f4-fdf1-4e33-8dcb-6bf46e390538
TQID: https://experienceleague.adobe.com/9WIJvPZIlrtLkv-vv-HRkctgwHn3nX-jrE4-4usXW0Y
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
source-git-commit: a1af9d2c909d9b3d506dd4875d1bd75149dbf636
workflow-type: tm+mt
source-wordcount: 825
ht-degree: 80%

---

# [!UICONTROL adobe.target.applyOffers （options） &#x200B;] - at.js 2.x

この関数を使用すると、`adobe.target.getOffers()` で取得した複数のオファーを適用できます。

>[!NOTE]
>
>この関数は、at.js 2.*x*&#x200B;で導入されました。 この関数は、at.js バージョン 1.*x*&#x200B;では使用できません。

| キー | タイプ | 必須？ | 説明 |
| --- | --- | --- | --- |
| selector | 文字列 | × | [!DNL Target] がオファーコンテンツを配置する必要がある HTML 要素を特定するために使用される HTML 要素または CSS セレクター。 セレクターが指定されていない場合、[!DNL Target]は、使用するHTML要素がHTML HEADであると仮定します。 |
| 応答 | オブジェクト | ○ | `getOffers()`からの応答オブジェクト。<br />以下のリクエストの表を参照してください。 |

## 応答

>[!NOTE]
>
>以下に示すすべてのフィールドで使用可能なタイプについて詳しくは、[&#x200B; 配信 API ドキュメント &#x200B;](/help/dev/implement/delivery-api/overview.md) を参照してください。

| フィールド名 | 説明 |
| --- | --- |
| response > prefetch > views > options > content | 注意：「オプション」の内容は明確に定義されておらず、オプションのタイプ／テンプレートの構造に直接依存しています。 |
| response > prefetch > views > options > type | オプションのタイプ。 「コンテンツ」フィールドのタイプを反映します。 サポートされているタイプはアクションです。 |
| response > prefetch > views > state | ビューの表示通知で転送する必要がある不透明なビューステートトークン。 |
| response > prefetch > views > options > responseTokens | 現在のオプションの処理時に収集された `responseTokens` のマップが含まれています。 |
| response > prefetch > views > analytics > payload | ビューが適用された後に[!DNL Analytics]に送信される、クライアントサイド統合用の[!DNL Analytics] ペイロード。 |
| response > prefetch > views > trace | ビューごとのプリフェッチコールの全トレースデータを含むオブジェクト。<br />トレースオブジェクトにはトレースのバージョンも含まれます。<br />トレースオブジェクトには、現在のビューの詳細も含まれます。 |
| response > prefetch > views > options > eventToken | イベントログはオプションごとに実行されます。 適用されるオプションごとに、それぞれのイベントトークンを通知トークンのリストに追加する必要があります。 注意： ビューは複数のオプションで構成されています。 すべてのオプションの適用および表示されている場合、通知にすべての `eventTokens` を含める必要があります。 |
| response > prefetch > views > name | 人間が解読可能な形式のビュー名。 |
| response > prefetch > views > metrics | 監視してから [!DNL Target] に通知する必要のあるレポート指標。 現在、クリック指標のみがサポートされています。 要素がクリックされると、適切な `eventTokens` が収集され、通知が送信されます。 |
| response > prefetch > views > key | ビューを識別するキーまたはフィンガープリント。 |
| response > prefetch > views > id | ビューの ID。 |
| response > notifications > id | 通知 ID。 |
| response > notifications > events > type | 通知、クリックまたは表示のタイプ。 |
| response > notifications > events > trace | 通知イベントのトレース。 |
| response > notifications > events > token | 通知イベントで送信されたトークン。 |
| response > notifications > events > timestamp | 通知イベントで送信されたタイムスタンプ。 |
| response > notifications > events > errorCode | 通知が失敗した場合、コードは失敗の理由を示します。 |
| response > notifications > events | 現在の通知について記録されたか、記録が失敗したイベント。 |
| response > notifications | 通知が記録されたか、記録が失敗したかを示します。 |
| response > execute > mboxes > mbox > trace | 個々の mbox リクエストの全トレースデータを含むオブジェクト。 |
| response > execute > mboxes > mbox > responseTokens | 特定の mbox リクエストの実行のための `responseTokens` のマップが含まれています。 |
| response > execute > mboxes > mbox > option > content | 注意：「オプション」の内容は明確に定義されておらず、オプションのタイプ／テンプレートの構造に直接依存しています。 |
| response > execute > mboxes > mbox > option > type | オプションのタイプ。 「コンテンツ」フィールドのタイプを反映します。 サポートされているタイプは、html、redirect、JSON、および dynamic です。 |
| response > execute > mboxes > mbox > options | レスポンスオプション。 |
| response > execute > mboxes > mbox > metrics > eventToken | クリックイベントのトークン。 |
| response > execute > mboxes > mbox > metrics > type | &quot;click&quot; |
| response > execute > mboxes > mbox > metrics | `clickThrough` 指標のリストが含まれています。 |
| response > execute > mboxes > mbox > mbox | mbox の名前。 |
| response > execute > mboxes > mbox >index | レスポンスが、リクエストからのこのインデックスを持つ mbox に対するものであることを示します。 |
| response > execute > mboxes > mbox > analytics > payload | mboxが適用された後に[!DNL Analytics]に送信するクライアントサイド統合用の[!DNL Analytics] ペイロード。 （「A4T が有効なキャンペーン」セクションを参照してください） |
| response > execute > mboxes | 実行された mbox のリスト。 |
| response > execute > pageLoad > options > content | 注意：「オプション」の内容は明確に定義されておらず、オプションのタイプ／テンプレートの構造に直接依存しています。 |
| response > execute > pageLoad > options > type | オプションのタイプ。 「コンテンツ」フィールドのタイプを反映します。 サポートされているタイプは、html、redirect、JSON、dynamic、および action です。 |
| response > execute > pageLoad > options | ビューでグループ化されていないオプション（target-global-mbox およびビューでグループ化されていないビューを持つアクティビティのオプション）。 |
| response > execute > pageLoad > metrics | 特定のビューに属していないクリック指標。 |
| response > execute > pageLoad > trace | pageLoad リクエストのすべてのトレースデータを含むオブジェクト。 |
| response > execute > pageLoad > analytics > payload | ページ読み込みコンテンツが適用された後に[!DNL Analytics]に送信される、クライアントサイド統合用の[!DNL Analytics] ペイロード。 （「A4T が有効なキャンペーン」セクションを参照してください） |

## 例[!UICONTROL applyOffers （） &#x200B;]呼び出し

```javascript {line-numbers="true"}
adobe.target.applyOffers({response:{
  "execute": {
    "pageLoad": {
      "options": [{
        "type": "html",
        "content": "page-load"
      },
      {
        "type": "actions",
        "content": [{
          "type": "setHtml",
          "content": "<h1>Container 1</h1>",
          "selector": "#container1",
          "cssSelector": "#container1"
        },
        {
          "type": "setHtml",
          "content": "<h3>Container 3</h3>",
          "selector": "#container3",
          "cssSelector": "#container3"
        }]
      }],
 
 
      "metrics": [{
        "type": "click",
        "selector": "#container1",
        "eventToken": "page-load-click-metric" 
      }]
    }
  }
}});
```

## これらの関数はPromise ベースであるため、`[!UICONTROL getOffers()]`および`[!UICONTROL applyOffers()]`を使用したPromise チェーンの呼び出しの例

```javascript {line-numbers="true"}
adobe.target.getOffers({...})
.then(response => adobe.target.applyOffers({ response: response }))
.then(() => console.log("Success"))
.catch(error => console.log("Error", error));
```

getOffers （）の使用例については、getOffers [&#x200B; ドキュメント &#x200B;](adobe-target-getoffers-atjs-2.md)を参照してください

### ページ読み込みリクエストの例


```javascript {line-numbers="true"}
adobe.target.getOffers({
    request: {
        execute: {
            pageLoad: {}
        }
    }
}).
then(response => adobe.target.applyOffers({ response: response }))
.then(() => console.log("Success"))
.catch(error => console.log("Error", error));
```



