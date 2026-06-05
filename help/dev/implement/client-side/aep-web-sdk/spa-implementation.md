---
title: '[!DNL Adobe Experience Platform Web SDK]のシングルページアプリケーションの実装'
description: ' [!DNL Target]を使用して [!DNL Adobe Experience Platform Web SDK]のシングルページアプリケーション （SPA）実装を作成する方法について説明します。'
keywords: target;adobe target;xdm ビュー；ビュー；シングルページアプリケーション；SPA;SPA;SPA ライフサイクル；クライアントサイド；AB テスト；AB；エクスペリエンスターゲティング；XT;VEC
feature: AEP Web SDK
exl-id: 17e71e47-c7cc-421a-bc9c-53f45f587449
TQID: https://experienceleague.adobe.com/Kp5fxEhLaXUNi6GOXXnET-1ueGQVLC0tPFhYzShk0cQ
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: bcc5edb5-84c3-4940-9f84-ed88b6c16274
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1836
ht-degree: 2%

---

# シングルページアプリケーションの導入

[!DNL Adobe Experience Platform Web SDK]には、シングルページアプリケーション （SPA）などの次世代のクライアントサイド技術でパーソナライゼーションを実行するための豊富な機能が用意されています。

従来のweb サイトでは、「ページ間」ナビゲーションモデルを使用していました。これは、マルチページアプリケーションとも呼ばれます。 これらのweb サイトでは、デザインはURLと密接にリンクされており、ページ間を移動するにはページ全体の読み込みが必要です。

代わりに、シングルページアプリケーションなどの最新のweb アプリケーションでは、ブラウザーUI レンダリングを迅速に使用できるモデルが採用されています。このモデルは、ページのリロードに依存しないことが多いです。 スクロール、クリック、カーソルの移動など、顧客とのやり取りによってトリガーされます。 現代のwebのパラダイムが進化するにつれ、パーソナライゼーションと実験を展開するための、ページ読み込みなどの従来の一般的なイベントの関連性はもはや機能しません。

![従来のページ ライフサイクルとSPA ライフサイクルを比較した図。](/help/dev/implement/client-side/aep-web-sdk/assets/spa-vs-traditional-lifecycle.png)

## SPAの[!DNL Experience Platform Web SDK]の利点

シングルページアプリケーションに[!DNL Adobe Experience Platform Web SDK]を使用するメリットを以下に示します。

* ページ読み込み時にすべてのオファーをキャッシュし、複数のサーバー呼び出しを単一のサーバー呼び出しに減らす機能。
* オファーは従来のサーバーコールによって生じるラグ時間なしにキャッシュを介してすぐに表示されるため、サイトのユーザーエクスペリエンスが向上します。
* 1行のコードと1回限りの開発者セットアップにより、マーケターは、SPAの[!UICONTROL Visual Experience Composer] （VEC）を介して[!UICONTROL A/B テスト &#x200B;]および[!UICONTROL Experience Targeting] （XT）アクティビティを作成して実行できます。

## XDM ビューとシングルページアプリケーション

SPA用[!UICONTROL Adobe Target] VECは、[!UICONTROL &#x200B; ビュー]と呼ばれる概念を利用しています。これは、SPA エクスペリエンスを構成するビジュアル要素の論理グループです。 したがって、単一ページアプリケーションは、ユーザーのインタラクションに基づいて、URLではなくビューを介して移行と見なすことができます。 [!UICONTROL &#x200B; ビュー]は、通常、サイト全体またはサイト内のグループ化されたビジュアル要素を表すことができます。

ビューの詳細については、次の例では、[!DNL React]に実装された仮想的なオンライン e コマースサイトを使用して、例[!UICONTROL &#x200B; ビュー]を調べます。

ホームサイトに移動すると、ヒーロー画像がイースターセールと、サイトで利用可能な最新の製品を宣伝します。 この場合、ホーム画面全体に対して[!UICONTROL &#x200B; ビュー]を定義できます。 この[!UICONTROL &#x200B; ビュー]は、単に「ホーム」と呼ぶことができます。

![&#x200B; ブラウザーウィンドウでのシングルページアプリケーションのサンプル画像。](/help/dev/implement/client-side/aep-web-sdk/assets/example-views.png)

顧客が販売している商品に関心を持つようになると、**商品** リンクをクリックすることにしました。 ホームサイトと同様に、製品サイト全体を[!UICONTROL &#x200B; ビュー]として定義できます。 この[!UICONTROL &#x200B; ビュー]には、「products-all」という名前を付けることができます。

![すべての製品が表示されたブラウザーウィンドウでの単一ページアプリケーションのサンプル画像。](/help/dev/implement/client-side/aep-web-sdk/assets/example-products-all.png)

[!UICONTROL &#x200B; ビュー]は、サイト全体またはサイト上の視覚要素のグループとして定義できます。 製品サイトに表示される4つの製品は、グループ化して[!UICONTROL &#x200B; ビュー]と見なすことができます。 このビューには「products」という名前を付けることができます。

![&#x200B; ブラウザーウィンドウ内の単一ページアプリケーションのサンプル画像。サンプル製品が表示されている](/help/dev/implement/client-side/aep-web-sdk/assets/example-products.png)

お客様が「**さらに読み込む**」ボタンをクリックしてサイト上の他の製品を検索する場合、web サイトのURLは変更されません。 ただし、ここに[!UICONTROL &#x200B; ビュー]を作成して、表示される製品の2行目のみを表すことができます。 [!UICONTROL &#x200B; ビュー]名は「products-page-2」である可能性があります。

![&#x200B; ブラウザーウィンドウ内の単一ページアプリケーションのサンプル画像。追加ページにサンプル製品が表示されている](/help/dev/implement/client-side/aep-web-sdk/assets/example-load-more.png)。

顧客はサイトから商品を購入し、チェックアウト画面に進みます。 チェックアウトサイトでは、通常の配送とエクスプレス配送のどちらかを選択できるオプションが顧客に提供されます。 [!UICONTROL &#x200B; ビュー]は、サイト上の任意のビジュアル要素のグループにすることができるため、[!UICONTROL &#x200B; ビュー]を配信環境設定に使用して作成し、「配信環境設定」と呼ぶことができます。

![&#x200B; ブラウザーウィンドウでのシングルページアプリケーションのチェックアウトページのサンプル画像。](/help/dev/implement/client-side/aep-web-sdk/assets/example-check-out.png)

[!UICONTROL &#x200B; ビュー]の概念は、このシナリオよりもはるかに拡張できます。 これらのシナリオは、サイトで定義できる[!UICONTROL &#x200B; ビュー]の例のほんの一部です。

## [!UICONTROL XDM ビュー]を実装しています

[!UICONTROL XDM ビュー]を[!DNL Target]で利用すると、マーケターは[!UICONTROL Visual Experience Composer]を介してSPAでA/B テストとXT テストを実行できます。 これを行うには、1回限りの開発者セットアップを完了するために、次の手順を実行する必要があります。

1. [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/install/overview)をインストールします。
2. パーソナライズするシングルページアプリケーションのすべての[!UICONTROL XDM ビュー]を決定します。
3. [!UICONTROL XDM ビュー]を定義した後、A/BまたはXT VEC アクティビティを配信するには、`renderDecisions`を`true`に設定し、対応する[!UICONTROL XDM ビュー]をシングルページアプリケーションに`sendEvent()`関数を実装します。 [!UICONTROL XDM ビュー]は`xdm.web.webPageDetails.viewName`で渡す必要があります。 この手順により、マーケターは[!UICONTROL Visual Experience Composer]を活用して、これらのXDMのA/B テストおよびXT テストを開始できます。

   ```javascript
   alloy("sendEvent", { 
     "renderDecisions": true, 
     "xdm": { 
       "web": { 
         "webPageDetails": { 
         "viewName":"home" 
         }
       } 
     } 
   });
   ```

>[!NOTE]
>
>最初の`sendEvent()`呼び出しでは、エンドユーザーにレンダリングされるすべての[!UICONTROL XDM ビュー]が取得され、キャッシュされます。 [!UICONTROL XDM ビュー]が渡された後続の`sendEvent()`呼び出しは、キャッシュから読み取られ、サーバーコールなしでレンダリングされます。

## `sendEvent()`関数の例

この節では、仮想e コマース SPAに対してReactで`sendEvent()`関数を呼び出す方法を示す3つの例の概要を説明します。

### 例1:A/B テストのホームページ

マーケティング部門は、ホームページ全体でA/B テストを実施したいと考えている。

![&#x200B; ブラウザーウィンドウでのシングルページアプリケーションのサンプル画像。](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-1.png)

ホームサイト全体でA/B テストを実行するには、XDM `viewName`を`home`に設定して`sendEvent()`を呼び出す必要があります。

```jsx
function onViewChange() { 
  
  var viewName = window.location.hash; // or use window.location.pathName if router works on path and not hash 

  viewName = viewName || 'home'; // view name cannot be empty 

  // Sanitize viewName to get rid of any trailing symbols derived from URL 

  if (viewName.startsWith('#') || viewName.startsWith('/')) { 
    viewName = viewName.substr(1); 
  }
   
  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName":"home" 
        } 
      } 
    }
  }); 
} 

// react router v4 

const history = syncHistoryWithStore(createBrowserHistory(), store); 

history.listen(onViewChange); 

// react router v3 

<Router history={hashHistory} onUpdate={onViewChange} > 
```

### 例2：パーソナライズされた商品

マーケティングチームは、ユーザーが「**さらに読み込む**」をクリックした後、価格ラベルの色を赤に変更して、2行目の商品をパーソナライズしたいと考えています。

![&#x200B; パーソナライズされたオファーを表示する、ブラウザーウィンドウ内の単一ページアプリケーションのサンプル画像。](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-2.png)

```jsx
function onViewChange(viewName) { 

  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName": viewName
        }
      } 
    } 
  }); 
} 

class Products extends Component { 
  
  render() { 
    return ( 
      <button type="button" onClick={this.handleLoadMoreClicked}>Load more</button> 
    ); 
  } 

  handleLoadMoreClicked() { 
    var page = this.state.page + 1; // assuming page number is derived from component's state 
    this.setState({page: page}); 
    onViewChange('PRODUCTS-PAGE-' + page); 
  } 

} 
```

### 例3:A/B テスト配信の環境設定

マーケティング部門は、**Express Delivery**&#x200B;が選択されている場合に、ボタンの色を青から赤に変更するかどうかを確認するためにA/B テストを実行したいと考えています。 チームは、この変更が（両方の配信オプションでボタンの色を青に保つのではなく）コンバージョンを高めることができると考えています。

![A/B テストを行ったブラウザーウィンドウでの単一ページアプリケーションのサンプル画像。](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-3.png)

選択した配信設定に応じてサイト上のコンテンツをパーソナライズするには、配信設定ごとに[!UICONTROL 表示]を作成できます。 **通常の配信**&#x200B;を選択すると、[!UICONTROL &#x200B; ビュー]に「checkout-normal」という名前を付けることができます。 **Express Delivery**&#x200B;が選択されている場合、[!UICONTROL View]には「checkout-express」という名前を付けることができます。

```jsx
function onViewChange(viewName) { 
  alloy("sendEvent", { 
    "renderDecisions": true, 
    "xdm": { 
      "web": { 
        "webPageDetails": { 
          "viewName": viewName 
        }
      }
    }
  }); 
} 

class Checkout extends Component { 

  render() { 
    return ( 
      <div onChange={this.onDeliveryPreferenceChanged}> 
        <label> 
          <input type="radio" id="normal" name="deliveryPreference" value={"Normal Delivery"} defaultChecked={true}/> 
          <span> Normal Delivery (7-10 business days)</span> 
        </label> 
        <label> 
          <input type="radio" id="express" name="deliveryPreference" value={"Express Delivery"}/> 
          <span> Express Delivery* (2-3 business days)</span> 
        </label> 
      </div> 
    ); 
  } 

  onDeliveryPreferenceChanged(evt) { 
    var selectedPreferenceValue = evt.target.value; 
    onViewChange(selectedPreferenceValue); 
  } 

} 
```

## SPAに[!UICONTROL Visual Experience Composer]を使用する

[!UICONTROL XDM ビュー]の定義が完了し、それらの[!UICONTROL XDM ビュー]が渡された`sendEvent()`を実装すると、VECはこれらの[!UICONTROL &#x200B; ビュー]を検出し、ユーザーがA/BまたはXT アクティビティのアクションと変更を作成できるようにします。

>[!NOTE]
>
>SPAにVECを使用するには、[Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/)または[Chrome VEC Helper Extension](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension)のいずれかをインストールしてアクティベートする必要があります。

### [!UICONTROL 変更] パネル

[!UICONTROL 変更] パネルは、特定の[!UICONTROL &#x200B; ビュー]に対して作成されたアクションをキャプチャします。 [!UICONTROL &#x200B; ビュー]のすべてのアクションは、その[!UICONTROL &#x200B; ビュー]の下にグループ化されます。

### アクション

アクションをクリックすると、このアクションが適用されるサイト上の要素がハイライト表示されます。 [!UICONTROL &#x200B; ビュー]で作成された各VEC アクションには、次のアイコンがあります：**情報**、**編集**、**クローン**、**移動**、**削除**。 これらのアイコンについては、次の表で詳しく説明します。

| アイコン | 説明 |
|---|---|
| 情報 | アクションの詳細を表示します。 |
| 編集 | アクションのプロパティを直接編集できます。 |
| 複製 | アクションを、[!UICONTROL 変更] パネルに存在する1つ以上の[!UICONTROL &#x200B; ビュー]またはVECで参照して移動した1つ以上の[!UICONTROL &#x200B; ビュー]に複製します。 アクションは、[!UICONTROL 変更] パネルに必ずしも存在する必要はありません。<br/><br/>**注：** クローン操作を実行した後、VECの[!UICONTROL 表示]に[!UICONTROL 参照]経由で移動して、クローン操作が有効な操作であったかどうかを確認する必要があります。 アクションを[!UICONTROL &#x200B; ビュー]に適用できない場合は、エラーが表示されます。 |
| 移動 | アクションを[!UICONTROL &#x200B; ページ読み込みイベント &#x200B;]または[!UICONTROL 変更] パネルに既に存在するその他の[!UICONTROL &#x200B; ビュー]に移動します。<br/><br/>**ページ読み込みイベント：** ページ読み込みイベントに対応するすべてのアクションは、web アプリケーションの最初のページ読み込み時に適用されます。 <br/><br/>**メモ：**&#x200B;移動操作を行った後、[!UICONTROL 参照]経由でVECの[!UICONTROL &#x200B; ビュー]に移動して、移動が有効な操作であったかどうかを確認する必要があります。 アクションを[!UICONTROL &#x200B; ビュー]に適用できない場合は、エラーを参照してください。 |
| 削除 | アクションを削除します。 |

## SPAの例にVECを使用する

この節では、[!UICONTROL Visual Experience Composer]を使用してA/BまたはXT アクティビティのアクションと変更を作成する3つの例について説明します。

### 例1: 「ホーム」ビューの更新

この記事の前に、「ホーム」という名前の[!UICONTROL &#x200B; ビュー]がホームサイト全体に対して定義されました。 さて、マーケティング部門は次の方法で「ホーム」ビューを更新したいと考えています。

* **買い物かごに追加** ボタンと&#x200B;**いいね** ボタンを青色に変更します。 この変更は、ヘッダーのコンポーネントを変更する必要があるため、ページの読み込み中に行う必要があります。
* 「**2026**」ラベルの最新製品を「**2026**」の最新製品に変更し、テキストの色を紫色に変更します。

VECでこれらの更新を行うには、**作成**&#x200B;を選択し、これらの変更を「ホーム」ビューに適用します。

![Visual Experience Composer サンプル ページ。](/help/dev/implement/client-side/aep-web-sdk/assets/vec-home.png)

### 例2：製品ラベルの変更

「products-page-2」 [!UICONTROL &#x200B; ビュー]の場合、マーケティング部門は&#x200B;**価格** ラベルを&#x200B;**販売価格**&#x200B;に変更し、ラベルの色を赤に変更したいと考えています。

VECでこれらの更新を行うには、次の手順が必要です。

1. VECで「**参照**」を選択します。
2. サイトの上部ナビゲーションで「**製品**」を選択します。
3. 「**さらに読み込む**」を1回選択すると、2行目の商品が表示されます。
4. VECで「**作成**」を選択します。
5. アクションを適用して、テキストラベルを&#x200B;**販売価格**&#x200B;に変更し、色を赤に変更します。

製品ラベルを含む![Visual Experience Composer サンプルページ。](/help/dev/implement/client-side/aep-web-sdk/assets/vec-products-page-2.png)

### 例3：配信設定スタイルのパーソナライズ

[!UICONTROL &#x200B; ビュー]は、状態やラジオボタンのオプションなど、詳細レベルで定義できます。 この記事の前の記事では、[!UICONTROL &#x200B; ビュー]は、配信設定、「checkout-normal」および「checkout-express」に対して定義されていました。 「checkout-express」ビューのボタンの色を赤に変更したいと考えています。

VECでこれらの更新を行うには、次の手順が必要です。

1. VECで「**参照**」を選択します。
2. サイトのカートに商品を追加します。
3. サイトの右上隅にあるカートアイコンを選択します。
4. **ご注文のチェックアウト**&#x200B;を選択します。
5. **配信設定**&#x200B;の下の&#x200B;**Express配信** ラジオボタンを選択します。
6. VECで「**作成**」を選択します。
7. **支払い** ボタンの色を赤に変更します。

>[!NOTE]
>
>「**Express Delivery**」ラジオボタンが選択されるまで、「checkout-express」 [!UICONTROL &#x200B; ビュー]が[!UICONTROL 変更] パネルに表示されません。 これは、**Express Delivery** ラジオボタンが選択されている場合に`sendEvent()`関数が実行されるため、ラジオボタンが選択されるまで、VECは「チェックアウトエクスプレス」 [!UICONTROL View]を認識しません。

![配信設定セレクターを表示するVisual Experience Composer。](/help/dev/implement/client-side/aep-web-sdk/assets/vec-delivery-preference.png)
