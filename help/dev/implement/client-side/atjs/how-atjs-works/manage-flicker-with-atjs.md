---
keywords: ちらつき，at.js，実装，非同期，非同期，同期，同期，$8
description: ページまたはアプリの読み込み時に at.js と  [!DNL Target] prevent のちらつき（アクティビティコンテンツに置き換えられる前に、デフォルトコンテンツが一時的に表示される）を回避する方法を説明します。
title: at.js はちらつきを管理するにはどうすればよいですか？
feature: at.js
exl-id: 8aacf254-ec3d-4831-89bb-db7f163b3869
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '699'
ht-degree: 57%

---

# at.js によるちらつきの制御方法

[!DNL Adobe Target] at.js JavaScript ライブラリを使用してページまたはアプリの読み込み中のちらつきを防ぐ方法に関する情報です。

ちらつきは、デフォルトコンテンツがアクティビティコンテンツに置き換わる前に訪問者に一時的に表示される際に発生します。ちらつきは、訪問者を混乱させる可能性があるので、望ましくありません。

## 自動作成されたグローバル mbox の使用

「[グローバル mbox を自動作成](/help/dev/implement/client-side/atjs/global-mbox/customize-global-mbox.md)」設定を at.js 設定時に有効にすると、at.js はページの読み込み時に不透明度の設定を変更し、ちらつきを制御します。at.js が読み込まれると、`<body>` 要素の不透明度設定が「0」に変更され、訪問者は最初にページを見えなくなります。 [!DNL Target] からの応答を受信した後（または [!DNL Target] リクエストのエラーが検出された場合）、at.js は不透明度を「1」にリセットします。 これにより、訪問者は、アクティビティのコンテンツが適用された後にのみページを表示できます。

グローバル mbox を自動作成設定を at.js の設定時に有効にすると、at.js は HTML BODY スタイルの不透明度を 0 に設定します。[!DNL Target] からのレスポンスを受信した後、at.js はHTMLの BODY の不透明度を 1 にリセットします。

不透明度を 0 に設定すると、ちらつきを回避するためにページコンテンツは非表示になりますが、ブラウザーは引き続きページをレンダリングし、CSS、画像などの必要なアセットをすべて読み込みます。

実装で `opacity: 0` が機能しない場合は、`bodyHiddenStyle` をカスタマイズして `body {visibility:hidden !important}` に設定することで、ちらつきを管理することもできます。 特定の状況に最適な `body {opacity:0 !important}` または `body {visibility:hidden !important}` を使用できます。

次の図は、本文非表示と本文表示の呼び出しを at.js 1.*x* と at.js 2.x の両方について示しています。

**at.js 2.x**

（全幅に拡大するには、画像をクリックします）。

![ ターゲットフロー：at.js ページ読み込みリクエスト ](/help/dev/implement/client-side/assets/atjs-20-flow-page-load-request.png " ターゲットフロー：at.js ページ読み込みリクエスト "){zoomable="yes"}

**at.js 1.*x***

（全幅に拡大するには、画像をクリックします）。

![ ターゲットフロー：自動作成されたグローバル mbox](/help/dev/implement/client-side/atjs/how-atjs-works/assets/target-flow2.png "arget フロー：自動作成されたグローバル mbox"){zoomable="yes"}

`bodyHiddenStyle` オーバーライドについて詳しくは、「[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)」を参照してください。

## at.js を非同期に読み込むときのちらつき制御

at.js を非同期で読み込む方法は、ブラウザーによるレンダリングのブロックを防ぐのに最適ですが、Web ページにちらつきが生じることがあります。

関連するHTML要素が Target によってパーソナライズされた後に表示される事前非表示のスニペットを使用すると、ちらつきを回避できます。

at.js は、ページに直接埋め込むか、タグマネージャー（Adobe Experience Platform Launchなど）を使用して、非同期で読み込むことができます。

at.js がページに埋め込まれている場合、スニペットを追加してから at.js を読み込む必要があります。 タグマネージャーを使用して at.js を読み込む場合（これも非同期で読み込まれます）、タグマネージャーを読み込む前にスニペットを追加する必要があります。 タグマネージャーが同時に読み込まれる場合、スクリプトは at.js より前のタグマネージャーに含まれる可能性があります。

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

## triggerView （）の at.js 2.x でのちらつきの管理

SPA でターゲットとなるコンテンツを表示するために `triggerView()` を使用する場合、ちらつき制御が初期設定で提供されます。つまり、事前に非表示にするロジックを手動で追加する必要はありません。代わりに、at.js 2.x では、ターゲットとなるコンテンツを適用する前にビューの表示が必要になる場所を事前に非表示にします。

## getOffer （）と applyOffer （）を使用したちらつきの管理

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

## at.js 1.x で mboxCreate （）を使用した地域 mbox の使用（at.js 2.x ではサポートされていません）

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
