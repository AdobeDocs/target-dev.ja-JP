---
keywords: SDK、ちらつき、ちらつき防止、事前非表示、事前非表示、合金、at.js、実装、同意、CMP、スクリプトの配置、インライン、外部、SDKの選択を事前非表示にする
description: SDKの事前非表示 [!DNL Adobe Target] 機能を組み込んで、ページ読み込み中にパーソナライズされていないコンテンツ（ちらつき）のフラッシュを排除する方法について説明します。 SDKは、Adobe Alloy （Web SDK）とat.jsの両方で使用できます。
title: SDK統合ガイドの事前非表示
feature: Implementation
hide: true
source-git-commit: 81818370d32ee8c3f3538e5d8d942f66c13e6a13
workflow-type: tm+mt
source-wordcount: '1137'
ht-degree: 1%

---


# SDK統合ガイドの事前非表示

[!DNL Adobe Target]個のパーソナライゼーションがページのミッドレンダリングを書き換えることに起因するビジュアルフリッカーを防ぐ、小さな同期JavaScript ライブラリ。 `<head>`の上部に1つのインライン `<script>` タグを追加すると、パーソナライズされたコンテンツの準備が整った後、またはセーフティタイマーが起動した後にのみ、ページが表示されます。

## 統合ステップ {#integration-steps}

1. バンドルをダウンロードします。

   Flicker Manager UIを使用して`prehide.min.js`をダウンロードします。 ファイルはクライアントコードとガードタイムアウトで事前設定されているため、`PrehideConfig` ブロックは必要ありません。

1. `<head>`の最上部にインラインで埋め込みます。

   インライン `<script>` タグ内に`prehide.min.js`の内容を`<head>`の最初の子として直接貼り付けます。 インラインが推奨される理由については、[&#x200B; インラインと外部](#inline-vs-external)を参照してください。

   ```html
   <!-- 1. Prehide SDK: must be FIRST in <head> and BEFORE any Adobe SDK -->
   <script>
     // paste the contents of prehide.min.js here
   </script>
   
   <!-- 2. Then load Alloy.js OR at.js -->
   <script src="https://your-cdn/alloy.min.js"></script>
   ```

1. （オプション）ランタイム設定ブロックを追加します。

   UIを介してダウンロードせずにバンドルをセルフホストする場合、またはSDKの選択肢を上書きする必要がある場合にのみ必要です。 事前非表示スクリプトの前にconfig ブロックを配置します。

   ```html
   <script>
     window.PrehideConfig = {
       org: "your-client-code",
       sdk: "alloy"            // or "atjs" (defaults to "alloy")
     };
   </script>
   <script> /* prehide.min.js inline contents */ </script>
   <script src="https://your-cdn/alloy.min.js"></script>
   ```

1. （オプション）同意の電信送金。

   実装で同意管理プラットフォーム （CMP）を使用している場合は、同意状態がわかったら直ちに`window.Prehide.setConsent(...)`に電話します。 [同意管理](#consent-management)を参照してください。

1. 検証：

   DevToolsを開き、最初のペイントの`<head>`に`<style id="alloy-prehiding">` （またはat.jsの`at-body-style`）が表示され、SDKが終了すると削除されることを確認します。 コンソールで`window.Prehide.getState()`を実行して、ランタイム状態を調べます。

## スクリプトを配置する場所 {#script-placement}

>[!IMPORTANT]
>
>Prehide SDKはAlloy/at.jsの前に実行する必要があります。 Alloyが最初に読み込まれた場合、ページはパーソナライズされていないコンテンツをレンダリングしてから再レンダリングします。 これが、このSDKが防ぐように設計された正確なフリッカーです。
></br>>SDKの事前非表示スクリプトタグに`async`または`defer`を追加しないでください。 ブラウザーがページのレイアウトを開始する前に非表示ルールを挿入するには、同期実行が必要です。

「SDKを事前に非表示」は、後からクリーンアップする[!DNL Adobe Target] SDKよりも前に表示する必要があります。 読み込み順序は交渉不可能です：

```html
<!doctype html>
<html>
<head>
  <!-- ① Prehide SDK FIRST. Inline. Synchronous. No async/defer. -->
  <script> /* prehide.min.js inline contents */ </script>

  <!-- ② Alloy or at.js loads next -->
  <script src="https://cdn.alloy.adobe.com/alloy.min.js"></script>

  <!-- ③ Then everything else: meta, css, third-party tags, ... -->
  <link rel="stylesheet" href="main.css">
</head>
<body> ... your page ... </body>
</html>
```

## インラインと外部 {#inline-vs-external}

`prehide.min.js`を含めるには、次の2つの方法があります。

| メソッド | 例 | メモ |
| --- | --- | --- |
| インライン（推奨） | `<script>/* full contents of prehide.min.js pasted directly into the page */</script>` | ネットワークのラウンドトリップをゼロにする： SDKは、他の何かがレンダリングされる前に実行されます。 |
| 外部（インライン化が不可能な場合のみ） | `<script src="/static/prehide.min.js"></script>` | 最初のレンダリングの前にブロッキングネットワークリクエストを導入します。 HTTP/2およびエッジキャッシュを使用する場合でも、これには通常30～80 ミリ秒のコストがかかります。 |

### インラインが好まれる理由

>[!TIP]
>
>HTML テンプレートの`<script>...</script>`内でバンドルを直接インライン化します。 小さく、インラインで、常に一番上にある、重要なCSS ブロックのように扱います。

* レンダーブロッキングのフェッチがありません。 SDKの事前非表示の目的は、最初のレンダリングの&#x200B;*前*&#x200B;に非表示ルールを挿入することです。 外部`<script src>`は、まさにそのクリティカルウィンドウにネットワークのラウンドトリップを追加します。
* 新しい障害モードはありません。 外部ファイルは、404、タイムアウト、または広告ブロッカーによってブロックされる可能性があります。 インラインコピーは使用できません。
* バンドルは小さい（約6 KB）。 インライン化のコストは一般的なファビコンよりも低く、キャッシュのメリットは、最初のレンダリング時に余分なラウンドトリップを上回るほど大きくありません。
* キャッシュフレンドリー： HTML レスポンスでインライン化すると、SDKは、既存のキャッシュ層（CDNまたはブラウザーのHTTP キャッシュ）によって、ドキュメントの残りの部分と共にキャッシュされます。
* 顧客固有のバンドル： ダウンロードしたファイルには、ダウンロード時にクライアントコードが組み込まれています。 インライン化により、すべての訪問者が、追加のリクエストなしで正しいカスタマイズされたバンドルを受け取ることができます。

## 設定 {#configuration}

SDKは、2つのソースから優先順に設定を受け付けます。 最初に使用可能な方を読み上げます。

### A: ダウンロード時のプレースホルダー（ランタイム設定なし）

Flicker Manager UIから`prehide.min.js`をダウンロードすると、サーバーはバンドル内の3つのプレースホルダーに置き換わります。

| プレースホルダ | 次で置き換え | 置き換えられない場合のフォールバック |
| --- | --- | --- |
| `__FM_CLIENT_CODE__` | あなたのクライアントコード （例：`"acmecorp"`） | `window.PrehideConfig.org`を読み取ります |
| `__FM_TIMEOUT__` | ガードタイマーの継続時間（ミリ秒） （例：`"3000"`） | `5000` ミリ秒 |
| `__FM_VERSION__` | SDK バージョン （例：`"1.0.0"`） | `"0.0.0-dev"` |

UIでダウンロードしたバンドルを使用する場合、`PrehideConfig` ブロックは必要ありません。 スクリプトをインライン化するだけです。

### B: ランタイム `window.PrehideConfig` （手動統合）

セルフホストまたは未変更のバンドルの場合は、事前非表示スクリプトを実行する前にconfig オブジェクトを宣言します。

```html
<script>
  window.PrehideConfig = {
    org: "acmecorp",        // required (or rely on baked-in __FM_CLIENT_CODE__)
    sdk: "alloy"             // optional: "alloy" (default) or "atjs"
  };
</script>
```

| フィールド | タイプ | 必須 | 説明 |
| --- | --- | --- | --- |
| `org` | string | はい（焼き込まれていない限り） | 顧客の顧客コード。 事前非表示ルールの取得元となるCDN URLの組織セグメントとして使用されます。 |
| `sdk` | `"alloy"` \| `"atjs"` | × | Adobe SDKがページに読み込まれました。 [SDKの選択範囲](#sdk-selection)を参照してください。 |

## SDKの選定 {#sdk-selection}

[!DNL Adobe Target]では、Alloy （最新のWeb SDK）とat.js （クラシックライブラリ）の2つの配信SDKを提供しています。 パーソナライゼーションが完了すると、それぞれ&#x200B;*different* `<style>`要素`id`を探し、削除してページを表示します。 Prehide SDKは、一致する`id`を挿入する必要があります。そうしないと、セーフティタイマーが起動するまでページは非表示のままになります。

| `sdk`値 | 挿入されたスタイルタグ ID | 削除者 | 使用するタイミング |
| --- | --- | --- | --- |
| `"alloy"` *（デフォルト）* | `<style id="alloy-prehiding">` | Alloy SDK on personalize-complete | このページでAlloy / Adobe Web SDKを読み込んでいます。 |
| `"atjs"` | `<style id="at-body-style">` | personalize-complete上のat.js | このページでクラシック at.js ライブラリを読み込んでいます。 |

### 設定方法

```html
<!-- For at.js -->
<script>
  window.PrehideConfig = { org: "acmecorp", sdk: "atjs" };
</script>
<script> /* prehide.min.js inline */ </script>
<script src="https://cdn.adobe.com/.../at.js"></script>
```

>[!WARNING]
>
>一致しない警告。 at.jsの読み込み中に`sdk: "alloy"`を設定すると（またはその逆）、SDKで削除するprehide要素が見つからなくなります。 ガードタイマーは最終的にページを表示しますが、訪問者はより長い非表示ウィンドウを体験します。 読み込むライブラリに合わせて、常に`sdk`を設定してください。

不明または不足している値は`"alloy"`にフォールバックするため、既存のAlloy統合は設定を変更することなく引き続き機能します。

## 同意管理 {#consent-management}

>[!NOTE]
>
>* 同意値は`window`に保存されません。 関数のみが公開されます。内部ステートはSDKに対して非公開のままです。
>* `"out"`から`"in"`への移行では、完全にレンダリングされたコンテンツを再非表示にすると視覚的に混乱が生じるため、ページを再表示しません。
>* `setConsent`は、1つのページビュー内で複数回呼び出すことができます。 各呼び出しは、前の状態を置き換えます。

Prehide SDKには、CMPと連携するための同意に応じたAPIが含まれています。 使用はオプションです。 `setConsent`が呼び出されない場合、SDKは標準の非同意統合のように動作します。

### API サーフェス

```js
// Single function call. Pass a status string.
window.Prehide.setConsent("pending");  // banner shown, awaiting decision
window.Prehide.setConsent("in");       // user accepted personalization
window.Prehide.setConsent("out");      // user declined personalization

// Read-only debug snapshot.
window.Prehide.getState();
// → { sdk, version, consentStatus, consentApiUsed,
//      rulesResolved, hasSelectorsToGuard, guardTimeout }
```

### 各ステータスの機能

| 呼び出し | ガードタイマーへの影響 | 非表示のコンテンツへの影響 |
| --- | --- | --- |
| `setConsent("pending")` | アクティブタイマーが消去されます。 同意が解決されるまで、安全性の再表示は行われません。 | セレクターは無期限に非表示のままです。 |
| `setConsent("in")` | クリアしてから、設定されたタイムアウトで再起動しました。 ルールがまだ解決されていない場合は、解決されるのを待ちます。 | SDKがパーソナライズされるか、ガードタイマーが起動するまで、コンテンツは非表示のままです。 |
| `setConsent("out")` | クリアされました。 このページはすぐに非表示になります。 | ページがすぐに表示されます。 後で解決するルールは、*not*&#x200B;でコンテンツを再非表示にします。 |
| *（呼び出されることはありません）* | 既定のタイマーは、設定済みの期間`init()`から実行されます。 | SDKがパーソナライズされるか、ガードタイマーが起動するまで、コンテンツは非表示のままです。 （後方互換性のあるレガシーモード） |

### 推奨パターン：明示的な保留段階

1. CMPが同意UIを表示したら、すぐに`setConsent("pending")`に電話します。 これにより、訪問者の意思決定中はページのセーフティタイマーが解除され、バナーの背後でパーソナライズされていないコンテンツが瞬く間に表示されるのを防ぎます。

   ```js
   window.Prehide.setConsent("pending");
   ```

1. 訪問者がパーソナライゼーションを受け入れたら、`setConsent("in")`に電話してください。 ガードタイマーが再起動し、Alloy/at.jsが引き継ぎ、パーソナライゼーションが適用されるとページが表示されます。

   ```js
   window.Prehide.setConsent("in");
   ```

1. 訪問者がパーソナライゼーションを辞退する場合は、`setConsent("out")`に電話してください。 ページがすぐに表示され、表示されたままになります。 後で解決するCDN ルールは、再非表示にしません。

   ```js
   window.Prehide.setConsent("out");
   ```

### 例：OneTrust スタイルの統合

```js
// Called once OneTrust has rendered its banner.
function onOneTrustReady() {
  // Pause the guard timer while the banner is visible.
  window.Prehide.setConsent("pending");

  OneTrust.OnConsentChanged(function (event) {
    if (event.consentedToTargeting) {
      window.Prehide.setConsent("in");
    } else {
      window.Prehide.setConsent("out");
    }
  });
}
```

### 例：TCF / IAB スタイル

```js
// Optional: pause the guard while UI is up.
window.Prehide.setConsent("pending");

// Your CMP eventually emits the final TC string.
function onTcData(tcData) {
  const hasTargetConsent = /* derive from tcData */;
  window.Prehide.setConsent(hasTargetConsent ? "in" : "out");
}
```

