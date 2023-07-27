---
keywords: ちらつき， at.js，実装，非同期，非同期，同期，同期， $8
description: at.js と [!DNL Target] ページまたはアプリの読み込み中にちらつきを防ぎます（デフォルトのコンテンツは、アクティビティのコンテンツに置き換えられる前に一時的に表示されます）。
title: at.js はどのようにしてちらつきを制御しますか。
feature: at.js
exl-id: 8aacf254-ec3d-4831-89bb-db7f163b3869
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 63%

---

# at.js によるちらつきの制御方法

次に関する情報： [!DNL Adobe Target] at.js JavaScript ライブラリは、ページやアプリの読み込み中にちらつきを回避します。

ちらつきは、デフォルトコンテンツがアクティビティコンテンツに置き換わる前に訪問者に一時的に表示される際に発生します。ちらつきは、訪問者を混乱させる可能性があるので、望ましくありません。

## 自動作成されたグローバル mbox の使用

「[グローバル mbox を自動作成](/help/dev/implement/client-side/atjs/global-mbox/customize-global-mbox.md)」設定を at.js 設定時に有効にすると、at.js はページの読み込み時に不透明度の設定を変更し、ちらつきを制御します。at.js が読み込まれると、その要素の不透明度の設定が`<body>`「0」に変更され、そのページが最初は訪問者に対して表示されなくなります。からの応答の後 [!DNL Target] を受け取った、または [!DNL Target] リクエストが検出されました。at.js は不透明度を「1」にリセットします。 これにより、訪問者は、アクティビティのコンテンツが適用された後にのみページを表示できます。

グローバル mbox を自動作成設定を at.js の設定時に有効にすると、at.js は HTML BODY スタイルの不透明度を 0 に設定します。からの応答の後 [!DNL Target] を受け取ると、at.js はHTMLBODY の不透明度を 1 にリセットします。

不透明度を 0 に設定すると、ちらつきを回避するためにページコンテンツは非表示になりますが、ブラウザーは引き続きページをレンダリングし、CSS、画像などの必要なアセットをすべて読み込みます。

次の場合 `opacity: 0` 実装では機能しない場合は、をカスタマイズして、ちらつきを制御することもできます。 `bodyHiddenStyle` を設定し、 `body {visibility:hidden !important}`. 次のいずれかを使用できます。 `body {opacity:0 !important}` または `body {visibility:hidden !important}`のうち、特定の環境に適した方を選択します。

次の図は、本文非表示と本文表示の呼び出しを at.js 1.*x* と at.js 2.x の両方について示しています。

**at.js 2.x**

（全幅に拡大するには、画像をクリックします）。

![Target フロー： at.js ページ読み込みリクエスト](/help/dev/implement/client-side/assets/atjs-20-flow-page-load-request.png "Target フロー： at.js ページ読み込みリクエスト"){zoomable=&quot;yes&quot;}

**at.js 1.*x***

（全幅に拡大するには、画像をクリックします）。

![Target フロー：自動作成されたグローバル mbox](/help/dev/implement/client-side/atjs/how-atjs-works/assets/target-flow2.png "ターゲットフロー：自動作成されたグローバル mbox"){zoomable=&quot;yes&quot;}

`bodyHiddenStyle` オーバーライドについて詳しくは、「[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)」を参照してください。

## at.js を非同期に読み込むときのちらつき制御

at.js を非同期で読み込む方法は、ブラウザーによるレンダリングのブロックを防ぐのに最適ですが、Web ページにちらつきが生じることがあります。

事前に非表示になっていて、対象の HTML 要素が Target によってパーソナライズされてから表示されるスニペットを使用することで、ちらつきを防ぐことができます。

at.js は、ページに直接埋め込むか、タグマネージャー ( 例：Adobe Experience Platform Launch) を使用して、非同期で読み込むことができます。

at.js がページに埋め込まれている場合は、at.js を読み込む前にスニペットを追加する必要があります。 タグマネージャーを使用して at.js を読み込むと（同時に非同期で読み込まれる）、タグマネージャーを読み込む前にスニペットを追加する必要があります。 タグマネージャーが同期して読み込まれる場合、スクリプトは at.js の前にタグマネージャー内に含まれる可能性があります。

事前に非表示になるコードスニペットの例を次に示します。

```
;(function(win, doc, style, timeout) {
  var STYLE_ID = 'at-body-style';

  function getParent() {
    return doc.getElementsByTagName('head')[0];
  }

  function addStyle(parent, id, def) {
    if (!parent) {
      return;
    }

    var style = doc.createElement('style');
    style.id = id;
    style.innerHTML = def;
    parent.appendChild(style);
  }

  function removeStyle(parent, id) {
    if (!parent) {
      return;
    }

    var style = doc.getElementById(id);

    if (!style) {
      return;
    }

    parent.removeChild(style);
  }

  addStyle(getParent(), STYLE_ID, style);
  setTimeout(function() {
    removeStyle(getParent(), STYLE_ID);
  }, timeout);
}(window, document, "body {opacity: 0 !important}", 3000));
```

デフォルトでは、このスニペットによって HTML BODY 全体が事前に非表示になります。ページ全体ではなく、一部の HTML 要素のみを事前に非表示にしたい場合もあるでしょう。その場合はスタイルのパラメーターをカスタマイズします。ページ内の特定のセクションのみを事前に非表示にするスタイルに置き換えることができます。

例えば、container-1 と container-2 という ID で識別される 2 つのセクションがある場合は、次のスタイルに置き換えることができます。

```
#container-1, #container-2 {opacity: 0 !important}
```

デフォルト設定の代わりに次を使用します。

```
body {opacity: 0 !important}
```

## at.js 2.x の triggerView() でのちらつき制御

SPA でターゲットとなるコンテンツを表示するために `triggerView()` を使用する場合、ちらつき制御が初期設定で提供されます。つまり、事前に非表示にするロジックを手動で追加する必要はありません。代わりに、at.js 2.x では、ターゲットとなるコンテンツを適用する前にビューの表示が必要になる場所を事前に非表示にします。

## getOffer() および applyOffer() によるちらつき制御

`getOffer()` と `applyOffer()` はどちらも、抽象度の低い API であるため、ちらつき制御の機能は組み込まれていません。セレクターまたは HTML 要素をオプションとして `applyOffer()` に渡せます。この場合、`applyOffer()` はアクティビティコンテンツをこの特定の要素に追加します。ただし、`getOffer()` および `applyOffer()` が呼び出される前に、この要素が適切に事前に非表示になっていることを確認する必要があります。

```
document.documentElement.style.opacity = "0";
 
adobe.target.getOffer({
    mbox: 'target-global-mbox',
    success: function(offer) {
        adobe.target.applyOffer({
            mbox: 'target-global-mbox',
            offer: offer
        });
 
        document.documentElement.style.opacity = "1";
    },
    error: function() {
        document.documentElement.style.opacity = "1";        
    }
});
```

## at.js 1.x での mboxCreate() によるリージョナル mbox の使用（at.js 2.x では未サポート）

リージョナル mbox 実装を使用する場合、`mboxCreate()` を以下のサンプルコードと同様にプロビジョニングされたページと共に使用できます。

```
<div class="mboxDefault">
Some default content
</div>
<script>
mboxCreate('some-mbox');
</script>
```

ページが適切にプロビジョニングされている場合、at.js は、mboxDefault クラスを使用して要素の CSS「visibility」プロパティを適切に切り替えることで、ちらつきを制御します。
