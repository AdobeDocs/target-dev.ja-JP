---
keywords: at.js faq, at.js に関するよくある質問, faq, ちらつき, ローダー, ページローダー, クロスドメイン, ファイルサイズ, ファイルのサイズ, x-domain, at.js と mbox.js, x のみ, クロスドメイン, safari, シングルページアプリ, セレクターが見つかりません, セレクター, シングルページアプリケーション, tt.omtrdc.net, spa, Adobe Experience Manager, AEM, ip アドレス, httponly, HttpOnly, Secure, ip, cookie ドメイン
description: at [!DNL Adobe Target] js JavaScript ライブラリに関するよくある質問への回答を紹介します。
title: at.js に関するよくある質問と回答
feature: at.js
exl-id: 362ccc5b-8731-46c0-bc52-3e55c273e216
source-git-commit: 448c43c0c10e22ad054f4ee98bfc282f8c96cdcb
workflow-type: tm+mt
source-wordcount: '2923'
ht-degree: 67%

---

# at.js に関するよくある質問

[!DNL Adobe Target] の at.js JavaScript ライブラリに関するよくある質問への回答

## mbox.js と比較して at.js を使用するメリットは何ですか。

at.js ライブラリは、mbox.js に代わるものです。 mbox.js ライブラリはサポートされなくなりました。 ただし、ほとんどの場合、at.js は mbox.js よりもメリットがあります。

特に、at.js は、web 実装のページ読み込み時間を改善し、セキュリティを向上し、単一ページアプリケーション向けのより優れた実装オプションを提供します。

次の図は、mbox.js と at.js を使用した場合のページ読み込みパフォーマンスを示しています。

（全幅に拡大するには、画像をクリックします）。

![mbox.js と at.js の比較を行ったページパフォーマンス図 ](/help/dev/implement/client-side/atjs/assets/atjs_versus_mboxjs.png "mbox.js と at.js の比較を行ったページパフォーマンス図 "){zoomable="yes"}

上図のとおり、mbox.js を使用すると、[!DNL Target] の呼び出しが完了するまでページコンテンツの読み込みが開始されませんでした。at.js を使用すると、[!DNL Target] の呼び出しが開始するとページ内容が読み込まれ、呼び出しが完了するまで待ちません。 

## at.js および mbox.js はページ読み込み時間にどのように影響しますか？

多くのお客様とコンサルタントは、at.js と mbox.js がページ読み込み時間に与える影響を、特に新規ユーザーと再ユーザーのコンテキストで知りたいと考えています。 残念ながら、at.js や mbox.js が各顧客の実装によってページ読み込み時間にどのように影響するかについて、測定し、具体的な数値を示すのは困難です。

ただし、訪問者 API がページに存在 [!DNL Target] る場合は、at.js と mbox.js がページ読み込み時間にどのように影響するかを把握しやすくなります。

>[!NOTE]
>
>訪問者 API および at.js または mbox.js がページ読み込み時間に影響するのは、グローバル mbox を使用している場合のみです（本文を非表示にする手法があるので）。 リージョナル mbox は訪問者 API 統合の影響を受けません。

以降のセクションでは、新しい訪問者と再訪問者の一連のアクションについて説明します。

### 新しい訪問者

1. 訪問者 API が読み込まれ、解析され、実行されます。
1. at.js または mbox.js が読み込まれ、解析され、実行されます。
1. グローバル mbox の自動作成が有効な場合、[!DNL Target] JavaScript ライブラリは以下を実行します。

   * Visitor オブジェクトをインスタンス化します。
   * [!DNL Target] ライブラリは、Experience Cloudの訪問者 ID データの取得を試みます。
   * この訪問者は新規訪問者なので、訪問者 API によって demdex.net へのクロスドメインリクエストがトリガーされます。
   * Experience Cloud訪問者 ID データが取得されたら、[!DNL Target] へのリクエストがトリガーされます。

### 再訪問者

1. 訪問者 API が読み込まれ、解析され、実行されます。
1. at.js または mbox.js が読み込まれ、解析され、実行されます。
1. グローバル mbox の自動作成が有効な場合、[!DNL Target] JavaScript ライブラリは以下を実行します。

   * Visitor オブジェクトをインスタンス化します。
   * [!DNL Target] ライブラリは、Experience Cloudの訪問者 ID データの取得を試みます。
   * 訪問者 API が、Cookie からデータを取得します。
   * Experience Cloud訪問者 ID データが取得されたら、[!DNL Target] へのリクエストがトリガーされます。

>[!NOTE]
>
>新規訪問者の場合、訪問者 API が存在す [!DNL Target] と、リクエストにExperience Cloudの訪問者 ID データが含まれていることを確認するために複数回やり取りを行う必要 [!DNL Target] あります。 再訪問者の場合、[!DNL Target] は、パーソナライズされたコンテンツを取得するためにのみ [!DNL Target] とやり取りします。

## at.js を旧バージョンから 1.0.0 にアップグレードした後、応答時間が長くなったように感じるのはなぜですか。

at.js バージョン 1.0.0 以降では、すべてのリクエストが同時並行で実行されます。 以前のバージョンではリクエストが順番に実行されます。つまり、リクエストがキューに入り、[!DNL Target] は、最初のリクエストの処理が完了するまで待ってから次のリクエストに移ります。

at.js の以前のバージョンでのリクエストの実行方法では、いわゆる「ヘッドオブラインブロッキング」の問題が生じやすくなります。 at.js 1.0.0 以降では、が同時並行でリクエストを実行するように変更されまし [!DNL Target]。

at.js 0.9.1 で「ネットワーク」タブのウォーターフォールをチェックすると、の現在のリクエストの処理が完了するまで、次のリク [!DNL Target] ストが実行されないことがわかります。 at.js 1.0.0 以降では、このような順次処理は行われず、基本的にすべてのリクエストが同時に開始されます。

応答時間に関しては、次のような数式が成り立ちます。

<ul class="simplelist"> 
 <li> at.js 0.9.1：すべての [!DNL Target] リクエストの応答時間= リクエストの応答時間の合計 </li> 
 <li> at.js 1.0.0 以降：すべての [!DNL Target] リクエストの応答時間= リクエストの応答時間の最大値 </li> 
</ul>

at.js ライブラリバージョン 1.0.0 の方がリクエストの処理が早く完了します。 また、at.js のリクエストは非同期なので、によってページレンダリング [!DNL Target] ブロックされることはありません。 リクエストの処理に数秒を要した場合でも、レンダリングされたページが表示されます。[!DNL Target] が [!DNL Target] エッジサーバーからの応答を受け取るまで、ページの一部分が空白になるだけです。

## [!DNL Target] ライブラリを非同期で読み込むことはできますか？

at.js 1.0.0 リリースでは、[!DNL Target] ライブラリを非同期で読み込めるようになりました。

at.js を非同期で読み込む手順は次のとおりです。

* Adobe Experience Platformのタグを使用する方法をお勧めします。
* at.js を読み込むスクリプトタグに async 属性を追加することで、at.js を非同期で読み込むこともできます。次のように使用します。

  ```
  <script src="<URL to at.js>" async></script>
  ```

* 次のコードでも、at.js を非同期で読み込むことができます。

  ```
  var script = document.createElement('script'); 
  script.async = true; 
  script.src = "<URL to at.js>"; 
  document.head.appendChild(script);
  ```

at.js を非同期で読み込む方法は、ブラウザーによるレンダリングのブロックを防ぐのに最適ですが、Web ページにちらつきが生じることがあります。

ページ（または指定された部分）を事前に非表示にしておき、at.js とグローバルリクエストが読み込まれてからページを表示するスニペットを使用すると、ちらつきを回避できます。このスニペットは、at.js の読み込みの前に追加する必要があります。

非同期 [!UICONTROL Adobe Experience Platform] 実装を通じて at.js をデプロイする場合は、 [!UICONTROL Adobe Experience Platform] 埋め込みコードを使用して [!DNL Target] を実装する前に、事前非表示のスニペットをページに直接含めてください。

同期 DTM 実装を介して at.js を導入する場合、ページの最上部にあるページ型ルールを通して、スニペットを非表示にすることができます。

詳しくは、「[at.js によるちらつきの制御方法](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)」を参照してください。

## at.js は [!DNL Adobe Experience Manager] 統合（Experience Manager）と互換性がありますか？

[!DNL Adobe Experience Manager] 6.2 と FP-11577 （またはそれ以降）で、at.js 実装とその [!UICONTROL Adobe Target Cloud Services] 統合をサポートします。

## どうしたら at.js を使用してページ読み込み時のちらつきを回避できますか？

[!DNL Target] には、ページ読み込み時のちらつきを防ぐ方法がいくつか用意されています。 詳しくは、「[at.js によるちらつきの防止 ](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)」を参照してください。

## at.js のファイルサイズはどれくらいですか？

at.js ファイルはダウンロード時には約 109 KB あります。ただし、ほとんどのサーバーは自動的にファイルを圧縮してファイルのサイズを小さくするので、（GZIP またはその他の方法を使用して）サーバーで圧縮され、ユーザーが Web サイトを訪問した際に読み込まれる at.js は約 34 KB になります。at.js をインストールしたサーバーの圧縮設定により、実際の圧縮サイズが決まります。

## at.js が mbox.js よりも大きいのはなぜですか？

at.js の実装は単一のライブラリ（at.js）を使用し、mbox.js の実装は実際に 2 つのライブラリ（mbox.js と target.js）を使用します。 そのため、より公平な比較は、at.js 対 mbox.js *および* `target.js` になります。2 つのバージョンの gzip 圧縮サイズを比較すると、at.js バージョン 1.2 は 34 KB で、mbox.js バージョン 63 は 26.2 KB です。

at.js がより大きいのは、mbox.js に比べて、より多くの DOM 解析をおこなうためです。at.js は JSON 応答で「生」データを取得し、その意味を理解する必要があるので、これが必要です。mbox.js では `document.write()` を使用し、すべての解析はブラウザーによって行われます。

より大きなファイルサイズにもかかわらず、アドビのテストでは、at.js のページの読み込みは mbox.js に比べて高速であることを示しています。さらに、at.js は、追加のファイルを動的に読み込まず、`document.write` も使用しないので、セキュリティの観点からも優れています。

## at.js には jQuery が含まれていますか。既に Web ページに jQuery があるので、at.js のこの部分を削除できますか。

at.js は、現在、jQuery の一部を使用しています。そのため、at.js の上部に MIT ライセンス通知が表示されます。jQuery は、公開されておらず、バージョンが異なる可能性のある、既にページにある jQuery ライブラリに干渉しません。at.js 内の jQuery コードの削除は、サポートされていません。

## at.js は、Safari とクロスドメインの「x のみ」の設定をサポートしますか。

いいえ、クロスドメインが「x のみ」に設定され、Safari がサードパーティの Cookie を無効化している場合、mbox.js と at.js の両方が無効化された Cookie を設定し、そのクライアントのドメインでは mbox のリクエストが実行されません。

Safari の訪問者をサポートするには、「無効化」（ファーストパーティの Cookie のみ設定）または「有効化」（Safari ではファーストパーティの Cookie のみ設定、他のブラウザーではファーストパーティとサードパーティの Cookie を設定）の方がクロスドメインの設定として優れています。

## Target [!UICONTROL Visual Experience Composer] （VEC）を単一ページアプリケーションで使用できますか？

はい。at.js 2.x を使用する場合は、SPA に VEC を使用できます。詳しくは、[単一ページアプリケーション（SPA）Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/spa-visual-experience-composer.html) を参照してください。

## Adobe Experience Cloud デバッガーを at.js の実装と一緒に使用できますか。

はい。また、mboxTrace をデバッグ目的で使用したり、ブラウザーの開発者ツールを使用して、ネットワーク要求を調査し、「mbox」にフィルターして mbox 呼び出しを分離することもできます。

## at.js を使用した mbox 名に特殊文字を使用できますか。

はい、mbox.js の場合と同じです。

## Web ページで mbox が実行されないのはなぜですか。

[!DNL Target] のお客様は、[!DNL Target] でクラウドベースのインスタンスを使用してテストをおこなったり、簡単な概念実証に利用したりする場合があります。これらのドメインは、他の多くのドメインと同様に[パブリックサフィックスリスト](https://publicsuffix.org/list/public_suffix_list.dat)に含まれています。

これらのドメインを使用する場合は、targetGlobalSettings() を使用して `cookieDomain` 設定をカスタマイズしない限り、最新のブラウザーでは Cookie が保存されません。詳しくは、「[ でのクラウドベースのインスタンスの使用  [!DNL Target]](/help/dev/implement/client-side/target-debugging-atjs/targeting-using-cloud-based-instances.md) を参照してください。

## at.js を使用する際に、IP アドレスを Cookie ドメインとして使用することはできますか。

はい。[at.js バージョン 1.2 以降](/help/dev/implement/client-side/atjs/target-atjs-versions.md)では使用可能です。ただし、Adobeでは、常に最新バージョンを使用することを強くお勧めします。

>[!NOTE]
>
>次の例は、at.js バージョン 1.2 以降を使用する場合は不要です。

[targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) の使用方法によっては、at.js をダウンロードした後にコードを追加修正する必要があります。例えば、様々な Web サイトで [!DNL Target] の実装にそれぞれ若干異なる設定が必要で、これらの設定をカスタム JavaScript により動的に定義することができない場合、ファイルをダウンロードした後、各 Web サイトにアップロードする前に、カスタマイズを手動でおこなってください。

次の例では、`targetGlobalSettings()` at.js 関数を使用して、IP アドレスをサポートするためのコードスニペットを挿入できます。

この例は単一の IP アドレス向けです。

```
if (window.location.hostname === '123.456.78.9') { 
    window.targetGlobalSettings = window.targetGlobalSettings || {}; 
    window.targetGlobalSettings.cookieDomain = window.location.hostname; 
}
```

この例は IP アドレスの範囲向けです。

```
if (/^123\.456\.78\..*/g.test(window.location.hostname)) { 
    window.targetGlobalSettings = window.targetGlobalSettings || {}; 
    window.targetGlobalSettings.cookieDomain = window.location.hostname; 
}
```

## 「アクションでセレクターが見つかりません」などの警告メッセージが表示されるのはなぜですか。

これらのメッセージは at.js 機能と関係がありません。 at.js ライブラリは、DOM に見つからないすべての情報を報告しようとします。

この警告メッセージが表示された場合は、次のような原因が考えられます。

* ページが動的に作成されており、at.js が要素を見つけられない。
* （ネットワークが低速なので）ページの作成に時間がかかり、at.js が DOM でセレクターを見つけられない。
* アクティビティが実行されているページの構造が変更されている。Visual Experience Composer（VEC）でアクティビティを再度開くと、警告メッセージが表示されます。アクティビティを更新して、必要な要素がすべて見つかるようにします。
* 基になるページが Single Page Application （SPA）の一部であるか、ページの下部に表示される要素がページに含まれていて、at.js の「セレクターポーリングメカニズム」でこれらの要素が見つからない。 `selectorsPollingTimeout` の値を増やすと問題が解決する場合があります。詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。
* いずれかのクリック追跡指標が、その指標が設定された URL に関係なく、それ自体をすべてのページに追加しようとしている。害はありませんが、この状況ではこれらのメッセージの多くが表示されます。

  最良の結果を得るには、[ 最新バージョンの at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md) をダウンロードして使用してください。 at.js のダウンロード方法について詳しくは、[at.js のデプロイ方法 ](how-to-deployatjs/implement-target-without-a-tag-manager.md#download-atjs-using-the-target-interface)/*タグマネージャーを使用しない [!DNL Target] の実装* 記事の [*at [!DNL Target] js のダウンロードの節を参照してください*](how-to-deployatjs/implement-target-without-a-tag-manager.md)

## [!DNL Target] のサーバー呼び出しが送られる tt.omtrdc.net というドメインは何ですか？

tt.omtrdc.netは、[!DNL Target] のすべてのサーバー呼び出しを受信するAdobeのEDGE ネットワークのドメイン名です。

## at.js で HttpOnly および Secure の Cookie フラグが常に使用されるとは限らないのはなぜですか？

HttpOnly はサーバー側コードでのみ設定できます。[!DNL Target]mbox などの Cookieは JavaScript コードで作成され保存されるので、 では HttpOnly の [!DNL Target] Cookie フラグを使用できません。[!DNL Target] では、クロスドメインが有効な場合にサーバー側から設定されたサードパーティ Cookie に対して HttpOnly を設定します。

Secure は、ページが HTTPS でロードされた場合にのみ、JavaScript で設定できます。ページが最初 HTTP でロードされる場合、JavaScript ではこのフラグを設定できません。さらに、Secure フラグを使用する場合、Cookie は HTTPS ページでのみ使用できます。HTTPS を介して読み込まれたページの場合、[!DNL Target] は Secure 属性と SameSite=None 属性を設定します。

Cookie がクライアント側で生成されるので、[!DNL Target] がユーザーを適切に追跡できるようにするため、[!DNL Target] では、上記の状況を除き、これらのフラグのどちらも使用しません。

## at.js は、XSS や MITM 攻撃などのセキュリティ上の問題をどのように処理しますか。

at.js によって有効になるAdobe Edge ネットワークとの通信は、targetGlobalSettings （）関数（[targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)）で `secureOnly` オプションが true に設定されている場合にのみ、HTTPS 経由で行われます。それ以外の場合、at.js は、ページプロトコルに基づいて HTTP と HTTPS を切り替えることができます。

次のヘッダーがデフォルトで適用されます。
* HTTP 完全転送セキュリティ（HSTS）
* X-XSS 保護
* X コンテンツタイプオプション
* Referrer-Policy

クライアントページで既に使用されているすべてのヘッダーを適用できます。 これを行う一般的な方法の 1 つは、「HTTP リクエストヘッダー認証」を介することです。 Adobeカスタマーケアは、ベストプラクティスに関してさらにアドバイスを提供できます。

さらに、Adobe Edge Network へのリクエストは公開されており（訪問者のブラウザーから作成されるように設計されているので）、表示される訪問者の詳細は含まれていません（訪問者 ID のみが含まれています）。 これらのリクエストは、訪問者にエクスペリエンスを提供し、訪問者がページで表示する内容の詳細が含まれます。

これらのリクエストで送信される応答トークンおよびセッション ID に関しては、次の点に注意してください。

* コミュニケーションセッションを追跡します
* ランダムな文字で構成されています
* セッション ID は 30 分間有効です
* レスポンストークンは無効にできます（[ レスポンストークン ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)）
* これらは、Adobeソリューションの環境でのみ役立ちます。

at.js リクエストでは、値が「*」の `Access-Control-Allow-Origin` ヘッダーが表示されることが予想されます。これは、これらが公開されており、認証が必要ではなく、JavaScript呼び出しを使用してどのドメインからでもAdobe Edge Network にアクセスする必要があるためです。

ただし、コンテンツセキュリティポリシー（CSP）をページに適用する必要があります。 at.js の CSP 要件について詳しくは、[ コンテンツセキュリティポリシー ](/help/dev/before-implement/privacy/content-security-policy.md) および [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。

## at.js はどのくらいの頻度でネットワークリクエストを送信しますか？

[!DNL Target] は、すべての決定をサーバー側で実行します。つまり、at.js は、ページの再読み込みが発生するたび、または at.js パブリック API が呼び出されるたびに、ネットワークリクエストを送信します。

## ベストケースのシナリオの場合、コンテンツの非表示、置換、表示に関連するページ読み込みの際に、ユーザーが何らかの目に見える効果に気付くことはないと考えてもよいですか？

at.js は、HTML BODY 要素やその他の DOM 要素があらかじめ非表示にされている時間が長くならないように努めますが、これはネットワークの状態やアクティビティの設定に依存します。at.js [設定](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)を使用すると、BODY を非表示にする CSS スタイルをカスタマイズして、HTML BODY 全体を非表示にする代わりに、ページの一部のみをあらかじめ非表示にできます。これらの部分には、「パーソナライズ」する必要のある DOM 要素が含まれていることが予想されます。

## ユーザーがアクティビティの対象になる標準的なシナリオにおいてイベントはどのような順序で発生しますか？

at.js リクエストは非同期の `XMLHttpRequest` なので、次の手順が実行されます。

1. ページが読み込まれます。
1. at.js により、HTML BODY があらかじめ非表示にされます。HTML BODY の代わりに特定のコンテナをあらかじめ非表示にする設定もあります。
1. at.js リクエストが送信されます。
1. [!DNL Target] 応答が受信された後、[!DNL Target] が CSS セレクターを抽出します。
1. [!DNL Target] が CSS セレクターを使用して、カスタマイズする DOM 要素をあらかじめ非表示にするための STYLE タグを作成します。
1. HTML BODY をあらかじめ非表示にする STYLE が削除されます。
1. [!DNL Target] が DOM 要素のポーリングを開始します。
1. DOM 要素が見つかった場合は、[!DNL Target] が DOM の変更を適用し、その要素をあらかじめ非表示にする STYLE が削除されます。
1. DOM 要素が見つからない場合は、破損したページにならないように、グローバルタイムアウトによって要素の非表示が解除されます。

## アクティビティによって変更されている要素の非表示が at.js によって最終的に解除された後、ページのコンテンツはどのくらいの頻度で完全に読み込まれて表示されますか？

「アクティビティによって変更されている要素の非表示が at.js によって最終的に解除された後、ページのコンテンツはどのくらいの頻度で完全に読み込まれて表示されますか？」という上記のシナリオを検討してみましょう。言い換えると、そのページはアクティビティのコンテンツを除いて完全に表示され、その少し後にアクティビティのコンテンツが表示されます。

at.js は、ページのレンダリングをブロックしません。ユーザーは、ページ上のいくつかの空白領域に気付く可能性があります。これらの領域は、[!DNL Target] によってカスタマイズされる要素の配置場所になります。適用されるコンテンツに SCRIPT や IMG などのリモートアセットが多く含まれていない場合は、すべてが瞬時にレンダリングされます。

## 完全にキャッシュされたページは上記のシナリオにどのように影響しますか？ページ上の他のコンテンツが読み込まれた後は、アクティビティのコンテンツがよりはっきりと見えるようになるのですか？

ユーザーの場所から近くても [!DNL Target] エッジからは遠い CDN にページがキャッシュされている場合、そのユーザーは多少の遅延を感じる可能性があります。[!DNL Target] エッジは世界中に分散されているので、ほとんどの場合、これは問題になりません。

## ヒーロー画像が表示された後に、少し遅れてスワップアウトされる可能性はありますか？

次のシナリオを考えてみましょう。

[!DNL Target] のタイムアウトは 5 秒です。ヒーロー画像をカスタマイズするアクティビティが存在するページをユーザーが読み込みます。適用するアクティビティがあるかどうかを確認するリクエストを at.js が送信したところ、初期応答がありません。関連付けられているアクティビティがあるかどうかに関する応答を [!DNL Target] から受け取っていないので、ユーザーにはヒーロー画像の通常のコンテンツが表示されるとします。4 秒後、[!DNL Target] がアクティビティコンテンツを含んだ応答を返します。

この段階で代替バージョンが表示される可能性はありますか？つまり、4 秒後にヒーロー画像がスワップアウトされて、この画像のスワップにユーザーが気付く可能性はありますか？

最初は、画像ヒーロー DOM 要素は非表示になっています。[!DNL Target] からの応答を受信した後、at.js は、DOM の変更（IMG の置き換えや、カスタマイズされたヒーロー画像の表示など）を適用します。

## at.js にはどのような HTML の doctype が必要ですか?

a.js には HTML5 の doctype が必要です。

この構文は次のとおりです。

`<!DOCTYPE html>`

HTML5 の doctype を使用すると、ページが標準モードで読み込まれます。互換モードでロードする場合、at.js が依存する一部の JS API は無効になります。[!DNL Target]は、互換モードで at.js を無効にします。

## at.js は Ionic アプリ環境で機能しますか。

at.js は web 以外の環境での動作を想定していなかったので、この実装はテストされませんでした。 [!DNL Adobe] では、モバイル実装用の [SDK](/help/dev/implement/mobile/overview.md) を推奨しています。
