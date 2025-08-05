---
title: '[!DNL  Adobe Experience Platform Web SDK] のシングルページアプリケーションの実装'
description: ' [!DNL Adobe Experience Platform Web SDK]using [!DNL Target] の単一ページアプリケーション（SPA）実装を作成する方法を説明します。'
keywords: target;adobe target;xdm ビュー；ビュー；単一ページアプリケーション；SPA;SPA ライフサイクル；クライアントサイド；AB テスト；AB；エクスペリエンスのターゲット設定；XT;VEC
feature: AEP Web SDK
source-git-commit: f4e0e1b202863eb9fddb40836cf53d2df7cdbebe
workflow-type: tm+mt
source-wordcount: '1680'
ht-degree: 2%

---


# シングルページアプリケーションの実装

[!DNL Adobe Experience Platform Web SDK] は、単一ページアプリケーション（SPA）などの次世代のクライアントサイドテクノロジーでパーソナライゼーションを実行できる豊富な機能を提供します。

従来の Web サイトでは、複数ページアプリケーションとも呼ばれる「ページからページ」ナビゲーションモデルを使用しました。 これらの web サイトでは、デザインは URL に緊密にリンクされているので、ページ間を移動するにはページ全体を読み込む必要があります。

代わりに、単一ページアプリケーションなどの最新の web アプリケーションは、ブラウザー UI レンダリングの迅速な使用を推進するモデルを採用しています。これは、多くの場合、ページリロードに依存しません。 これらのエクスペリエンスは、スクロール、クリック、カーソル移動など、顧客のインタラクションによってトリガーされます。 最新の web のパラダイムが進化するにつれ、パーソナライゼーションと実験をデプロイするための、ページの読み込みなどの従来の汎用イベントの関連性は機能しなくなりました。

![ 従来のページライフサイクルと比較した SPA のライフサイクルを示す図。](/help/dev/implement/client-side/aep-web-sdk/assets/spa-vs-traditional-lifecycle.png)

## SPA 向け [!DNL Experience Platform Web SDK] のメリット

単一ページアプリケーションで [!DNL Adobe Experience Platform Web SDK] を使用する利点は次のとおりです。

* ページ読み込み時にすべてのオファーをキャッシュし、複数のサーバー呼び出しを単一のサーバー呼び出しに減らす機能。
* 従来のサーバーコールで生じるタイムラグのないキャッシュでオファーが即座に表示されるので、サイトでのユーザーエクスペリエンスが向上します。
* 1 行のコードと 1 回限りのデベロッパー設定を使用すると、マーケターは、SPA の [!UICONTROL A/B Test] （VEC）を介して [!UICONTROL Experience Targeting] and [!UICONTROL Visual Experience Composer] （XT）アクティビティを作成および実行できます。

## XDM ビューとシングルページアプリケーション

[!UICONTROL Adobe Target] VEC for SPA では、[!UICONTROL Views] と呼ばれる概念を利用します。これは、SPA エクスペリエンスを構成するビジュアル要素の論理的なグループです。 したがって、単一ページアプリケーションは、ユーザーのインタラクションに基づいて、URL ではなくビューを通じた移行と見なすことができます。 [!UICONTROL View] は通常、サイト全体またはサイト内のグループ化された視覚的要素を表すことができます。

ビューとは何かを詳しく説明するために、次の例では、[!DNL React] で実装された架空のオンライン e コマースサイトを使用して、[!UICONTROL Views] の例を調べています。

ホームページに移動すると、ヒーロー画像がイースターの販売とサイトで利用可能な最新の製品を宣伝します。 この場合、ホーム画面全体に対して [!UICONTROL View] を定義できます。 この [!UICONTROL View] は単に「家」と呼んでもよいでしょう。

![ ブラウザーウィンドウに表示される単一ページアプリケーションのサンプル画像 ](/help/dev/implement/client-side/aep-web-sdk/assets/example-views.png)

ビジネスで販売されている製品に対するお客様の関心が高まるにつれ、お客様は **製品** リンクをクリックすることにします。 ホームサイトと同様に、製品サイト全体を [!UICONTROL View] として定義できます。 この [!UICONTROL View] は、「products-all」という名前を付けることができます。

![ ブラウザーウィンドウに、すべての製品が表示された単一ページアプリケーションのサンプル画像 ](/help/dev/implement/client-side/aep-web-sdk/assets/example-products-all.png)

[!UICONTROL View] は、サイト全体またはサイト上の視覚要素のグループとして定義できるからです。 製品サイトに表示される 4 つの製品は、グループ化され、[!UICONTROL View] と見なすことができます。 このビューには、「products」という名前を付けることができます。

![ ブラウザーウィンドウに表示される単一ページアプリケーションのサンプル画像。表示例は製品です。](/help/dev/implement/client-side/aep-web-sdk/assets/example-products.png)

この場合、顧客が **さらに読み込む** ボタンをクリックしてサイト上のより多くの製品を参照することにしても、web サイトの URL は変更されません。 ただし、ここで [!UICONTROL View] を作成して、表示される 2 行目の製品のみを表すことができます。 [!UICONTROL View] の名前は「products-page-2」になります。

![ ブラウザーウィンドウに表示される単一ページアプリケーションのサンプル画像。追加のページに表示されるサンプル製品が含まれます。](/help/dev/implement/client-side/aep-web-sdk/assets/example-load-more.png)

お客様は、サイトからいくつかの製品を購入することを決定し、チェックアウト画面に進みます。 チェックアウトサイトでは、顧客は通常の配信または速達を選択するオプションを提供されます。 [!UICONTROL View] は、サイト上の任意の視覚要素のグループにすることができ、配信環境設定用の [!UICONTROL View] を作成して「配信環境設定」と呼ぶこともできます。

![ ブラウザーウィンドウに表示される単一ページアプリケーションのチェックアウトページのサンプル画像 ](/help/dev/implement/client-side/aep-web-sdk/assets/example-check-out.png)

[!UICONTROL Views] の概念は、このシナリオよりもはるかに拡張できます。 これらのシナリオは、サイトで定義できる [!UICONTROL Views] の例のほんの一部です。

## [!UICONTROL XDM Views] の実装

[!UICONTROL XDM Views] を [!DNL Target] で活用すると、マーケターが [!UICONTROL Visual Experience Composer] を介して SPA で A/B テストと XT テストを実行できるようになります。 これを行うには、1 回限りの開発者セットアップを完了するために、次の手順を実行する必要があります。

1. [Adobe Experience Platform Web SDK](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/install/overview) をインストールします。
2. パーソナライズする単一ページアプリケーション内のすべての [!UICONTROL XDM Views] を決定します。
3. [!UICONTROL XDM Views] を定義した後、A/B または XT VEC アクティビティを配信するには、`sendEvent()` に設定した `renderDecisions` 関数と、シングルページアプリケーションで対応す `true`[!UICONTROL XDM View] を実装します。 [!UICONTROL XDM View] は `xdm.web.webPageDetails.viewName` で渡す必要があります。 この手順を使用すると、マーケターは [!UICONTROL Visual Experience Composer] を活用して、これらの XDM に対して A/B テストと XT テストを開始できます。

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
>最初の `sendEvent()` 呼び出しで、エンドユーザーにレンダリングする必要があるすべての [!UICONTROL XDM Views] ータが取得され、キャッシュされます。 `sendEvent()` が渡された後続の [!UICONTROL XDM Views] 呼び出しは、キャッシュから読み取られ、サーバーコールなしでレンダリングされます。

## `sendEvent()` 関数の例

この節では、模擬 e コマース SPA 用に React で `sendEvent()` 関数を呼び出す方法を示す 3 つの例について説明します。

### 例 1:A/B テストのホームページ

マーケティングチームは、ホームページ全体で A/B テストを実行したいと考えています。

![ ブラウザーウィンドウに表示される単一ページアプリケーションのサンプル画像 ](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-1.png)

ホームサイト全体で A/B テストを実行するには、XDM `sendEvent()` を `viewName` に設定して `home` を呼び出す必要があります。

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

### 例 2：パーソナライズされた製品

マーケティングチームは、ユーザーが **さらに読み込む** をクリックした後で価格ラベルのカラーを赤に変更して、2 行目にある製品をパーソナライズしたいと考えています。

![ パーソナライズされたオファーを表示する、ブラウザーウィンドウ内の単一ページアプリケーションのサンプル画像 ](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-2.png)

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

### 例 3:A/B テスト配信の環境設定

マーケティングチームは、「**高速配信**」が選択されている場合に、ボタンの色を青から赤に変更するかどうかを確認するために、A/B テストを実行します。 チームは、（両方の配信オプションでボタンの色を青に保つのではなく）この変更によってコンバージョンが向上すると考えています。

![A/B テストを使用したブラウザーウィンドウ内の単一ページアプリケーションのサンプル画像 ](/help/dev/implement/client-side/aep-web-sdk/assets/use-case-3.png)

選択した配信環境設定に応じてサイトのコンテンツをパーソナライズするために、配信環境設定ごとに [!UICONTROL View] を作成できます。 **通常配信** が選択されている場合、[!UICONTROL View] ージに「checkout-normal」という名前を付けることができます。 **高速配信** が選択されている場合、[!UICONTROL View] ージに「checkout-express」という名前を付けることができます。

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

## SPA での [!UICONTROL Visual Experience Composer] の使用

[!UICONTROL XDM Views] の定義が完了し、渡された `sendEvent()` を使用して [!UICONTROL XDM Views] を実装したら、VEC は、これらの [!UICONTROL Views] を検出し、ユーザーが A/B または XT アクティビティのアクションと変更を作成できるようにします。

>[!NOTE]
>
>SPA で VEC を使用するには、{Firefox[ または ](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/)2}Chrome VEC Helper Extension[ をインストールして有効化する必要があります。](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension)

### [!UICONTROL Modifications] パネル

[!UICONTROL Modifications] パネルには、特定の [!UICONTROL View] ーザーに対して作成されたアクションが取り込まれます。 [!UICONTROL View] ージのすべてのアクションは、その [!UICONTROL View] ージの下にグループ化されます。

### アクション

アクションをクリックすると、このアクションが適用されるサイト上の要素がハイライト表示されます。 [!UICONTROL View] で作成された各 VEC アクションには、**情報**、**編集**、**クローン**、**移動**、**削除** のアイコンがあります。 これらのアイコンについて詳しくは、次の表を参照してください。

| アイコン | 説明 |
|---|---|
| 情報 | アクションの詳細を表示します。 |
| 編集 | アクションのプロパティを直接編集できます。 |
| 複製 | [!UICONTROL Views] パネルに存在する 1 つ以上の [!UICONTROL Modifications]、または VEC で参照および移動した 1 つ以上の [!UICONTROL Views] にアクションのクローンを作成します。 アクションは、必ずしも [!UICONTROL Modifications] パネルに存在する必要はありません。<br/><br/>**注意：** 複製操作を行ったら、[!UICONTROL View] を使用して VEC の [!UICONTROL Browse] に移動し、複製されたアクションが有効な操作かどうかを確認します。 アクションを [!UICONTROL View] に適用できない場合は、エラーが表示されます。 |
| 移動 | アクションを、[!UICONTROL Page Load Event] パネルに既に存在する [!UICONTROL View] ージまたはその他の [!UICONTROL Modifications] ージに移動します。<br/><br/>**ページの読み込みイベント：** ページの読み込みイベントに対応するアクションが web アプリケーションの最初のページ読み込みに適用されます。 <br/><br/>**注意：** 移動操作を行ったら、[!UICONTROL View] を使用して VEC の [!UICONTROL Browse] に移動し、移動が有効な操作かどうかを確認します。 アクションを [!UICONTROL View] に適用できない場合は、エラーが表示されます。 |
| 削除 | アクションを削除します。 |

## SPA への VEC の使用の例

この節では、[!UICONTROL Visual Experience Composer] を使用して A/B または XT アクティビティのアクションと変更を作成する 3 つの例について説明します。

### 例 1:「home」ビューを更新する

この記事の前半では、「home」という [!UICONTROL View] がホームサイト全体に対して定義されました。 マーケティングチームは、次のように「ホーム」ビューを更新します。

* **買い物かごに追加** ボタンと **いいね** ボタンを、より明るい青色に変更します。 この変更は、ヘッダーのコンポーネントを変更する必要があるので、ページの読み込み中に発生する必要があります。
* **2026 年の最新製品** ラベルを **2026 年の最もホットな製品** に変更し、テキストの色を紫色に変更します。

VEC でこれらの更新を行うには、「作成 **を選択し、その変更を「ホーム** ビューに適用します。

![Visual Experience Composer のサンプルページ ](/help/dev/implement/client-side/aep-web-sdk/assets/vec-home.png)

### 例 2：製品ラベルを変更する

「products-page-2」の [!UICONTROL View] については、マーケティングチームは **Price** ラベルを **Sale Price** に変更し、ラベルの色を赤に変更します。

VEC でこれらの更新を行うには、次の手順が必要です。

1. VEC で **参照** を選択します。
2. サイトの上部ナビゲーションで「**製品**」を選択します。
3. **さらに読み込み** を 1 回選択して、製品の 2 行目を表示します。
4. VEC で「**作成**」を選択します。
5. アクションを適用して、テキストラベルを **販売価格** に、カラーを赤に変更します。

![ 製品ラベルを使用した Visual Experience Composer サンプルページ ](/help/dev/implement/client-side/aep-web-sdk/assets/vec-products-page-2.png)

### 例 3：配信環境設定のスタイル設定をパーソナライズ

状態 [!UICONTROL Views] ラジオボタンからのオプションなど、詳細なレベルで定義することができます。 この記事の前半では、配信の環境設定、「checkout-normal」および「checkout-express」について [!UICONTROL Views] が定義されていました。 マーケティングチームは、「checkout-express」表示のボタンの色を赤に変更したいと考えています。

VEC でこれらの更新を行うには、次の手順が必要です。

1. VEC で **参照** を選択します。
2. サイト上の買い物かごに商品を追加します。
3. サイトの右上隅にある「買い物かご」アイコンを選択します。
4. **注文をチェックアウト** を選択します。
5. **配信環境設定** の下の **高速配信** ラジオボタンを選択します。
6. VEC で「**作成**」を選択します。
7. **Pay** ボタンのカラーを赤に変更します。

>[!NOTE]
>
>「チェックアウトエクスプレス」 [!UICONTROL View] は、「速達 [!UICONTROL Modifications] ラジオボタンが選択されるまで、**ールパネルに表示され** せん。 これは、`sendEvent()` 関数が、「高速配信 **** ラジオボタンが選択されたときに実行されるため、VEC は、ラジオボタンが選択されるまで「checkout-express」 [!UICONTROL View] を認識しません。

![ 配信環境設定セレクターを表示する Visual Experience Composer。](/help/dev/implement/client-side/aep-web-sdk/assets/vec-delivery-preference.png)
