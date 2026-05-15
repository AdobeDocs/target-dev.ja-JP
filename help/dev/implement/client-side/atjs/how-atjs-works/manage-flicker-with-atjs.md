---
keywords: flicker, at.js，実装，非同期，非同期，同期，同期，8 ドル
description: ページまたはアプリの読み込み中にat.jsと [!DNL Target] がフリッカーを防止する方法（デフォルトのコンテンツは、一時的にアクティビティコンテンツに置き換えられる前に表示されます）を説明します。
title: at.jsはどのようにFlickerを管理しますか？
feature: at.js
exl-id: 8aacf254-ec3d-4831-89bb-db7f163b3869
TQID: https://experienceleague.adobe.com/r8uyzkf1gSHmppyDHPOcn5jrH86Hedb4ArMmtigq93w
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 717
ht-degree: 57%

---

# at.js によるちらつきの制御方法

[!DNL Adobe Target] at.js JavaScript ライブラリが、ページまたはアプリの読み込み中にちらつきを防止する方法に関する情報。

ちらつきは、デフォルトコンテンツがアクティビティコンテンツに置き換わる前に訪問者に一時的に表示される際に発生します。 ちらつきは、訪問者を混乱させる可能性があるので、望ましくありません。

## 自動作成されたグローバル mboxの使用

「[グローバル mbox を自動作成](/help/dev/implement/client-side/atjs/global-mbox/customize-global-mbox.md)」設定を at.js 設定時に有効にすると、at.js はページの読み込み時に不透明度の設定を変更し、ちらつきを制御します。 at.jsが読み込まれると、`<body>`要素の不透明度設定が「0」に変更され、最初は訪問者にページが表示されなくなります。 [!DNL Target]からの応答を受け取った後、または[!DNL Target]要求に関するエラーが検出された場合、at.jsは不透明度を「1」にリセットします。 これにより、訪問者は、アクティビティのコンテンツが適用された後にのみページを表示できます。

グローバル mbox を自動作成設定を at.js の設定時に有効にすると、at.js は HTML BODY スタイルの不透明度を 0 に設定します。 [!DNL Target]からの応答を受け取った後、at.jsはHTML BODYの不透明度を1にリセットします。

不透明度を 0 に設定すると、ちらつきを回避するためにページコンテンツは非表示になりますが、ブラウザーは引き続きページをレンダリングし、CSS、画像などの必要なアセットをすべて読み込みます。

実装で`opacity: 0`が機能しない場合は、`bodyHiddenStyle`をカスタマイズして`body {visibility:hidden !important}`に設定し、ちらつきを管理することもできます。 `body {opacity:0 !important}`と`body {visibility:hidden !important}`のどちらか一方を使用すると、特定の状況に最適です。

次の図は、at.js 1.*x*&#x200B;とat.js 2.xの両方でボディを非表示とボディを表示の両方の呼び出しを示しています。

**at.js 2.x**

（画像をクリックして全幅に拡大します）。

![&#x200B; ターゲットフロー：at.js ページ読み込みリクエスト &#x200B;](/help/dev/implement/client-side/assets/atjs-20-flow-page-load-request.png " ターゲットフロー：at.js ページ読み込みリクエスト "){zoomable="yes"}

**at.js 1.*x***

（画像をクリックして全幅に拡大します）。

![&#x200B; ターゲットフロー：自動作成されたグローバル mbox](/help/dev/implement/client-side/atjs/how-atjs-works/assets/target-flow2.png " ターゲットフロー：自動作成されたグローバル mbox"){zoomable="yes"}

`bodyHiddenStyle` オーバーライドについて詳しくは、「[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)」を参照してください。

## at.js を非同期に読み込むときのちらつき制御

at.js を非同期で読み込む方法は、ブラウザーによるレンダリングのブロックを防ぐのに最適ですが、Web ページにちらつきが生じることがあります。

事前に非表示になっていて、対象の HTML 要素が Target によってパーソナライズされてから表示されるスニペットを使用することで、ちらつきを防ぐことができます。

at.jsは、ページに直接埋め込むか、タグマネージャー（Adobe Experience Platform Launchなど）を介して非同期で読み込むことができます。

at.jsがページに埋め込まれている場合、at.jsを読み込む前にスニペットを追加する必要があります。 非同期で読み込まれるタグマネージャーを介してat.jsを読み込む場合は、タグマネージャーを読み込む前にスニペットを追加する必要があります。 タグマネージャーが同期して読み込まれる場合、スクリプトはat.jsの前にタグマネージャー内に含まれる場合があります。

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

デフォルトでは、このスニペットによって HTML BODY 全体が事前に非表示になります。 ページ全体ではなく、一部の HTML 要素のみを事前に非表示にしたい場合もあるでしょう。 その場合はスタイルのパラメーターをカスタマイズします。 ページ内の特定のセクションのみを事前に非表示にするスタイルに置き換えることができます。

例えば、container-1 と container-2 という ID で識別される 2 つのセクションがある場合は、次のスタイルに置き換えることができます。

```
#container-1, #container-2 {opacity: 0 !important}
```

デフォルト設定の代わりに次を使用します。

```
body {opacity: 0 !important}
```

## at.js 2.xのtriggerView （）のフリッカーを管理する

SPA でターゲットとなるコンテンツを表示するために `triggerView()` を使用する場合、ちらつき制御が初期設定で提供されます。 つまり、事前に非表示にするロジックを手動で追加する必要はありません。 代わりに、at.js 2.x では、ターゲットとなるコンテンツを適用する前にビューの表示が必要になる場所を事前に非表示にします。

## getOffer （）とapplyOffer （）を使用してフリッカーを管理する

`getOffer()` と `applyOffer()` はどちらも、抽象度の低い API であるため、ビルトインのちらつき制御機能は含まれていません。 セレクターまたは HTML 要素をオプションとして `applyOffer()` に渡せます。この場合、`applyOffer()` はアクティビティコンテンツをこの特定の要素に追加します。ただし、`getOffer()` および `applyOffer()` が呼び出される前に、この要素が適切に事前に非表示になっていることを確認する必要があります。

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

## at.js 1.xでmboxCreate （）を使用した地域のmboxの使用（at.js 2.xではサポートされていません）

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
