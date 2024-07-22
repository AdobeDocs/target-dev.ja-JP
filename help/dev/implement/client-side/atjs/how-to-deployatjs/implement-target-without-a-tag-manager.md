---
keywords: target の実装，実装，at.js の実装，タグマネージャー，オンデバイス判定，オンデバイス判定
description: 設定（アカウントの詳細、実装方法など）の指定方法を説明します タグマネージャーを使用せずに at [!DNL Adobe Target] js ライブラリを実装するには、次の手順を実行します。
title: タグマネージャ  [!DNL Target]  なしで実装できますか？
feature: Implement Server-side
exl-id: f675ae21-105d-4aa3-9926-59291f1136b5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1693'
ht-degree: 34%

---

# タグマネージャーを使用しない [!DNL Target] の実装

[!DNL Adobe Experience Platform] でタグマネージャーまたはタグを使用せずに [!DNL Adobe Target] を実装する方法に関する情報です。

>[!NOTE]
>
>[Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) のタグは、[!DNL Target] および at.js ライブラリを実装するための推奨される方法です。 次の情報は、[!DNL Adobe Experience Platform] でタグを使用して [!DNL Target] を実装する場合は適用されません。

実装ページにアクセスするには、**[!UICONTROL Administration]**/**[!UICONTROL Implementation]** をクリックします。

このページでは、次の設定を指定できます。

* アカウントの詳細
* 実装方法
* プロファイル API
* デバッガーツール
* プライバシー

>[!NOTE]
>
>[!DNL Target] Standard/Premium UI や REST API を使用して設定を設定する代わりに、at.js ライブラリで設定を上書きできます。 詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。

## アカウントの詳細

次のアカウントの詳細を表示できます。 これらの設定は変更できません。

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL Client Code] | クライアントコードは、[!DNL Target] API を使用する際に必要となることが多い、クライアント固有の文字シーケンスです。 |
| [!UICONTROL IMS Organization ID] | この ID は、実装とAdobe Experience Cloud アカウントを結び付けます。 |
| [!UICONTROL On-Device Decisioning] | オンデバイス判定を有効にするには、切り替えスイッチを「オン」の位置にスライドします。<p>オンデバイス判定を使用すると、A/B およびエクスペリエンスのターゲット設定（XT）キャンペーンをサーバーにキャッシュし、ほぼゼロの待ち時間でメモリ内判定を実行できます。 詳しくは、[ オンデバイス判定の概要 ](../../../server-side/sdk-guides/on-device-decisioning/overview.md) を参照してください。 |
| [!UICONTROL Include all existing on-device decisioning qualified activities in the artifact] | （条件付き）このオプションは、オンデバイス判定を有効にした場合に表示されます。<p>オンデバイス判定の対象となるすべてのライブ [!DNL Target] アクティビティをアーティファクトに自動的に含める場合は、切り替えスイッチを「オン」の位置にスライドします。<p>このトグルをオフのままにすると、生成されたルールアーティファクトに含めるために、オンデバイス判定アクティビティを再作成してアクティブ化する必要があります。 |

## 実装方法

次の設定を実装方法パネルで設定できます。

### グローバル設定

>[!NOTE]
>
>これらの設定は、すべての.js ライブラリ [!DNL Target] 適用されます。 「実装方法」セクションで変更を実行した後、ライブラリをダウンロードして、実装で更新する必要があります。

| 設定 | 説明 |
| --- | --- |
| [!UICONTROL Page load enabled (Auto-create global mbox)] | 各ページが読み込まれると自動的に実行されるように、グローバル mbox 呼び出しを at.js ファイルに埋め込むかどうかを選択します。 |
| [!UICONTROL Global mbox] | global mbox の名前を選択します。デフォルトでは、この名前は target-global-mbox です。<p>特殊文字（アンパサンド（&amp;）を含む）は、at.js を使用した mbox 名で使用できます。 |
| [!UICONTROL Timeout (seconds)] | [!DNL Target] が定義された期間内にコンテンツの応答をしない場合、サーバー呼び出しはタイムアウトし、デフォルトコンテンツが表示されます。訪問者のセッション中、追加の呼び出しが引き続き試行されます。デフォルト値は 5 秒です。<p>at.js ライブラリは、`XMLHttpRequest` のタイムアウト設定を使用します。 タイムアウトは、リクエストが発生すると開始し、サーバーから応答を受け取 [!DNL Target] と停止します。 詳しくは、Mozilla Developer Network の [XMLHttpRequest.timeout](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/timeout) を参照してください。<p>指定したタイムアウトが応答の受信前に発生した場合、デフォルトのコンテンツが表示され、すべてのデータ収集は [!DNL Target] ーザーエッジで行われるので、訪問者はアクティビティの参加者としてカウントされる場合があります。 リクエストが [!DNL Target] エッジに到達すると、訪問者がカウントされます。<p>タイムアウト設定を構成する際は、次の点を考慮してください。<ul><li>値が低すぎると、訪問者はアクティビティの参加者としてカウントされるものの、ほとんどの時間デフォルトのコンテンツが表示される可能性があります。</li><li>値が高すぎると、Web ページに空白の領域が表示されるか、長時間の本文の非表示を使用している場合は空白のページが表示される可能性があります。</li></ul>mbox の応答時間をよりよく把握するには、ブラウザーの開発者ツールの「ネットワーク」タブを確認してください。また、Catchpoint など、サードパーティの Web パフォーマンス監視ツールを使用することもできます。<p>**注意**: [visitorApiTimeout](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#visitorapitimeout) 設定を使用すると、訪問者 API 応答の待機時間が長くなりすぎないよ [!DNL Target] になります。 この設定と、ここで説明している at.js のタイムアウト設定は相互に影響しません。 |
| [!UICONTROL Profile Lifetime] | この設定は、訪問者プロファイルが保存される期間を決定します。デフォルトでは、プロファイルは 2 週間保存されます。この設定は、最大 90 日まで増やすことができます。<p>プロファイルの有効期間の設定を変更するには、[Client Care](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html#reference_ACA3391A00EF467B87930A450050077C) にお問い合わせください。 |

### 主な実装方法

>[!NOTE]
>
>[!DNL Adobe Target] は、両方の at.js 1 をサポートしています。*x* と at.js 2 の両方について示しています。*x* を通じてクロスドメイントラッキングを使用している場合です。サポート対象のバージョンを使用するには、at.js のいずれかのメジャーバージョンの最新アップデートにアップグレードしてください。

目的の at.js バージョンをダウンロードするには、該当する **ダウンロード** ボタンをクリックします。

at.js の設定を編集するには、目的の at.js バージョンの横にある **[!UICONTROL Edit]** をクリックします。

>[!WARNING]
>
>これらのデフォルト設定を変更する前に、現在の実装に影響を与えないように、[Client Care](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html#reference_ACA3391A00EF467B87930A450050077C) に相談してください。

上記の設定に加えて、次の特定の at.js 設定も使用できます。

| 設定 | 説明 |
|--- |--- |
| クロスドメイン | at.js v1 の場合。*x*:`enabled` （ブラウザーがファーストパーティ cookie とサードパーティ cookie の両方を設定する）を選択することにより、クロスドメイン機能を `disabled` （ブラウザーがドメインに cookie を設定する（ファーストパーティ cookie のみ））、`x only` （ブラウザーが Target のドメインにのみ cookie を設定する）、またはその両方のいずれにするかを指定します。 at.js v2.10 以降で、クロスドメイン機能を `enabled` （ブラウザーがファーストパーティとサードパーティの Cookie を両方とも設定する）にするか、`disabled` （ブラウザーがサードパーティの Cookie を設定しない）にするかを指定します。 |
| カスタムライブラリヘッダー | ライブラリの最上部に含めるカスタム JavaScript を追加します。 |
| カスタムライブラリフッター | ライブラリの最下部に含めるカスタム JavaScript を追加します。 |

### プロファイル API

API による一括更新の認証を有効または無効にし、プロファイル認証トークンを生成します。

詳しくは、[ プロファイル API 設定 ](/help/dev/before-implement/methods-to-get-data-into-target/profile-api-settings.md) を参照してください。

### デバッガーツール

高度な [!DNL Target] デバッグツールを使用するための認証トークンを生成します。 **[!UICONTROL Generate New Authentication Token]** をクリックします。

![新しい認証トークンの生成](../../../../before-implement/methods-to-get-data-into-target/assets/debugger-auth-token.png)

### プライバシー

これらの設定により、適用されるデータプライバシー法に従って [!DNL Target] を使用できます。

訪問者 IP アドレスを不明化ドロップダウンリストから目的の設定を選択します。

* 最後のオクテットの不明化
* IP の不明化全体
* None

詳しくは、[プライバシー](/help/dev/before-implement/privacy/privacy.md)を参照してください。

>[!NOTE]
>
>レガシーブラウザーサポートオプションは、at.js バージョン 0.9.3 以前で使用できました。 このオプションは、at.js バージョン 0.9.4 で削除されました。at.js でサポートされているブラウザーのリストについては、[ サポートされているブラウザー ](/help/dev/before-implement/supported-browsers.md) を参照してください。<p>レガシーブラウザーは、CORS（クロスオリジンリソース共有）を完全にはサポートしない古いブラウザーです。こうしたブラウザーには、バージョン 11 より前の Internet Explorer およびバージョン 6 以下の Safari が含まれます。従来のブラウザーサポートが無効になっている場合、[!DNL Target] れらのブラウザーでコンテンツを配信しなかったか、レポートで訪問者がカウントされていました。 このオプションを有効にした場合、カスタマーエクスペリエンス（顧客体験）を良好にするために、古いブラウザーをまたいで品質保証を行うことをお勧めします。

## at.js のダウンロード

[!DNL Target] インターフェイスまたはダウンロード API を使用してライブラリをダウンロードする手順。

>[!NOTE]
>
>[Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) は、[!DNL Target] および at.js ライブラリを実装するための推奨される方法です。 次の情報は、[!DNL Adobe Experience Platform] でタグを使用して [!DNL Target] を実装する場合は適用されません。
>
>[!DNL Adobe Target] は、両方の at.js 1 をサポートしています。*x* と at.js 2 の両方について示しています。*x* を通じてクロスドメイントラッキングを使用している場合です。サポート対象のバージョンを使用するには、at.js のいずれかのメジャーバージョンの最新アップデートにアップグレードしてください。 各バージョンについて詳しくは、 [at.js のバージョンの詳細](/help/dev/implement/client-side/atjs/target-atjs-versions.md)を参照してください。

### [!DNL Target] インターフェイスを使用した at.js のダウンロード

[!DNL Target] インターフェイスから at.js をダウンロードするには：

1. **[!UICONTROL Administration]**／**[!UICONTROL Implementation]**&#x200B;をクリックします。
1. 「実装方法」セクションで、目的の at.js バージョンの横にある「**[!UICONTROL Download]**」ボタンをクリックします。

### [!DNL Target] Download API を使用して at.js をダウンロードします

API を使用して at.js をダウンロードするには、次の手順に従います。

1. クライアントコードを取得します。

   クライアントコードは、[!DNL Target] インターフェイスの **[!UICONTROL Administration]**/**[!UICONTROL Implementation]** ページの上部に表示されます。

1. 管理番号を取得します。

   次の URL を読み込みます。

   ```
   https://admin.testandtarget.omniture.com/rest/v1/endpoint/<varname>client code</varname>
   ```

   `client code` を手順 1 のクライアントコードに置き換えます。

   この URL を読み込んだ結果は、次の例のようになります。

   ```
   { 
     "api": "https://admin6.testandtarget.omniture.com/admin/rest/v1" 
   }
   ```

   この例では、「6」が管理番号です。

1. at.js をダウンロードします。

   この URL を次の構造で読み込みます。 この URL を読み込むと、カスタマイズされた at.js ファイルのダウンロードが開始されます。

   ```
   https://admin<varname>admin number</varname>.testandtarget.omniture.com/admin/rest/v1/libraries/atjs/download?client=<varname>client code</varname>&version=<version number>
   ```

   * `admin number` を管理番号に置き換えます。
   * `client code` を手順 1 のクライアントコードに置き換えます。
   * `version number` を目的の at.js バージョン番号（例：2.2）に置き換えます。

>[!WARNING]
>
>[!DNL Target] チームがサポートを提供しているのは、at.js の最新バージョンとその 1 つ前のバージョンの 2 つのみです。 必要に応じて at.js をアップグレードし、サポート対象のバージョンを使用するようにしてください。 各バージョンについて詳しくは、 [at.js のバージョンの詳細](/help/dev/implement/client-side/atjs/target-atjs-versions.md)を参照してください。

## at.js の実装

at.js は、Web サイトのすべてのページの `<head>` 要素で実装する必要があります。

[Adobe Experience Platformのタグなど、タグマネージャーを使用しない [!DNL Target] の一般的な実装は次のよ ](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) になります。

```
<!doctype html> 
<html> 
<head> 
    <meta charset="utf-8"> 
    <title>Title of the Page</title> 
    <!--Preconnect and DNS-Prefetch to improve page load time--> 
    <link rel="preconnect" href="//<client code>.tt.omtrdc.net"> 
    <link rel="dns-prefetch" href="//<client code>.tt.omtrdc.net"> 
    <!--/Preconnect and DNS-Prefetch--> 
    <!--Data Layer to enable rich data collection and targeting--> 
    <script> 
        var digitalData = { 
            "page": { 
                "pageInfo": { 
                    "pageName": "Home" 
                } 
            } 
        }; 
    </script> 
    <!--/Data Layer--> 
    <!-- targetPageParams(), targetPageParamsAll(), Data Providers or targetGlobalSettings() functions to enrich the visitor profile or modify the library settings--> 
    <script> 
        targetPageParams = function() { 
            return { 
                "a": 1, 
                "b": 2, 
                "pageName": digitalData.page.pageInfo.pageName, 
                "profile": { 
                    "age": 26, 
                    "country": { 
                        "city": "San Francisco" 
                    } 
                } 
            }; 
        }; 
    </script> 
    <!--/targetPageParams()--> 
 
    <!--jQuery or other helper libraries should be implemented before at.js if you would like to use their methods in Target--> 
    <script src="jquery-3.3.1.min.js"></script> 
    <!--/jQuery--> 
    <!--Target's JavaScript SDK, at.js--> 
    <script src="at.js"></script> 
    <!--/at.js--> 
</head>
<body> 
    The default content of the page 
</body> 
</html>
```

次の重要な注意点を考慮してください。

* HTML 5 Doctype （例：`<!doctype html>`）を使用する必要があります。 サポートされていない doctype または古い doctype を使用すると、[!DNL Target] がリクエストを作成できなくなる可能性があります。
* 事前接続とプリフェッチのオプションは、Web ページの読み込みを高速化するのに役立ちます。これらの設定を使用する場合は、`<client code>` を独自のクライアントコードに置き換えます。これは、**[!UICONTROL Administration]**/**[!UICONTROL Implementation]** ページから取得できます。
* データレイヤーがある場合、at.js が読み込まれる前にページの `<head>` でデータレイヤーについてできるだけ多く定義することが最適です。このプレースメントは、パーソナライゼーションに [!DNL Target] してこの情報を使用する最大限の機能を提供します。
* `targetPageParams()`、`targetPageParamsAll()`、データプロバイダー、`targetGlobalSettings()` などの特別な [!DNL Target] 関数は、データレイヤーの後、および at.js の読み込み前に定義する必要があります。 または、これらの関数を at.js 設定を編集ページのライブラリヘッダーセクションに保存して、at.js ライブラリ自体の一部として保存することもできます。 これらの関数について詳しくは、「[at.js 関数 ](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md)」を参照してください。
* jQuery などのJavaScript ヘルパーライブラリを使用する場合は、[!DNL Target] エクスペリエンスの構築時に構文とメソッドを使用できるように、[!DNL Target] の前にそれらを含めます。
* at.js はページの `<head>` に含めます。

## コンバージョンの追跡

注文の確認 mbox では、サイトでの注文に関する詳細が記録され、売上高および注文に基づくレポートが可能になります。また、注文の確認 mbox は、「商品 x および商品 y を購入した人」などのレコメンデーションアルゴリズムを駆動できます。

>[!NOTE]
>
>Adobeでは、Web サイトで購入する場合は、レポートに Analytics for [!DNL Target] （A4T）を使用していても、注文確認 mbox を実装することをお勧めします。

1. 注文の詳細ページで、以下のモデルに示す mbox スクリプトを挿入します。
1. 大文字のテキストを、カタログの動的値または静的値に置き換えます。

   >[!TIP]
   >
   >また、任意の mbox で注文情報を渡すこともできます（`orderConfirmPage` という名前にする必要はありません）。 また、同じキャンペーン内の複数の mbox に注文情報を渡すこともできます。

   ```
   <script type="text/javascript"> 
   adobe.target.trackEvent({ 
       "mbox": "orderConfirmPage", 
       "params":{  
           "orderId": "ORDER ID FROM YOUR ORDER PAGE",  
           "orderTotal": "ORDER TOTAL FROM YOUR ORDER PAGE",  
           "productPurchasedId": "PRODUCT ID FROM YOUR ORDER PAGE, PRODUCT ID2, PRODUCT ID3"  
       } 
   }); 
   </script> 
   ```

>[!NOTE]
>
>注文確認 mbox で、複数の製品 ID を区切るにはコンマで区切ります。

注文の確認 mbox では、次のパラメーターを使用します。

| パラメーター | 説明 |
|--- |--- |
| orderId | 注文を識別する一意の値（コンバージョンのカウントに使用）。<p>`orderId` は一意である必要があります。重複する注文はレポートで無視されます。 |
| orderTotal | 購入金額。<p>通貨記号は渡さないでください。（コンマではなく）小数点を使用して、10 進数値を示します。 |
| productPurchasedId（オプション） | この注文で購入された製品 ID のコンマ区切りのリスト。<p>これらの製品 ID は、追加のレポート分析をサポートするために監査レポートに表示されます。 |
