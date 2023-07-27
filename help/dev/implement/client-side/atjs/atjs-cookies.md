---
keywords: at.js, 2.0, 1.x, cookie
description: 方法の詳細 [!DNL Adobe Target] at.js 2.x および at.js 1.x は Cookie を処理します。
title: at.js の Cookie
feature: at.js
exl-id: 154a844a-6855-4af7-8aed-0719b4c389f5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1711'
ht-degree: 80%

---

# at.js の cookie

at.js 2.x および at.js 1 に関する情報です。*x* の Cookie の動作に関する情報です。

## at.js 2.x の Cookie の動作

at.js バージョン 2.x( 最大 ( バージョン2.10.0を含まない ) の場合、 *ファーストパーティ cookie のみがサポートされます*. at.js 1.*x*&#x200B;に値をセットする場合、ファーストパーティ Cookie「mbox」は `clientdomain.com`です。 `clientdomain` はお客様のドメインです。

at.js はセッション ID を生成し、Cookie に保存します。最初の応答には、任意のアクティビティ情報と、[!DNL Target] サーバーによって生成された `TNT` または `PC ID` も含まれています。続いて at.js は `TNT/PC ID` を Cookie に書き込みます。

The `AMCV_###@AdobeOrg` ファーストパーティ Cookie は、常にExperience CloudID サービスによって設定されます。ただし、 `ECID` が渡される [!DNL Target] リクエスト。

>[!NOTE]
>
>at.js バージョン2.10.0以降では、ファーストパーティ Cookie とクロスドメイン Cookie の両方がサポートされています。

### サードパーティ cookie とクロスドメイントラッキングのサポート

クロスドメイン追跡を使用すると、ドメインが異なる 2 つの関連サイト上のセッション同士を単一のセッションとして確認できます。`siteA.com` と `siteB.com` にまたがる [!DNL Target] アクティビティを作成し、訪問者がドメインを移るときに同じエクスペリエンスを維持できます。この機能は at.js 1.*x* のサードパーティ Cookie とファーストパーティ Cookie の動作に関連付けられています。

>[!NOTE]
>
>at.js バージョン2.10.0以降では、サードパーティ Cookie とクロスドメイントラッキングの両方がサポートされています。


## at.js 1.*x* cookie の動作

at.js バージョン 1.*x* の場合、Cookie の動作は、その Cookie がファーストパーティ Cookie であるか、ファーストパーティ Cookie を伴うサードパーティ Cookie であるか、サードパーティ Cookie のみであるかによって異なります。

### ファーストパーティ Cookie またはサードパーティ Cookie を使用するタイミング

サイトの設定によって、どの Cookie を使用するかが決まります。次の点を理解すると役に立ちます。 [!DNL Target] は、ファーストパーティ Cookie とサードパーティ Cookie を理解しようとする際に機能します。 詳しくは、 [方法 [!DNL Adobe Target] 動作](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html) を参照してください。

Cookie について、3 つの主要な使用例を次に示します。

1. 1 つのドメインの場合。

   すべてのテストは、1 つのトップレベルドメイン（`www.domain.com`、`store.domain.com`、`anysub.domain.com`、など）内で実施します。

   ファーストパーティ Cookie のみを使用します。これはデフォルトの設定です。

1. ユーザーが異なるドメインにアクセスし、それらのドメイン間における行動を追跡し、テストする場合。

   例：買い物をするためにお客様のサイトにアクセスしたユーザーが、Yahoo ストアを経由してチェックアウトする場合。次の 3 つの方法があります（最適な方法を決めるには、アカウント担当者に相談してください）。

   * ファーストパーティ Cookie およびサードパーティ Cookie を有効にする。
   * サードパーティ Cookie のみを有効にする（ほとんど利用されませんが、ドメイン外に at.js の Cookie を保持できる利点があります）。
   * ファーストパーティ Cookie のみを有効にし、異なるドメインにアクセスするときに `mboxSession` パラメーターを渡す。

     `mboxSession` パラメーターは、at.js を参照しているランディングページに渡す必要があります。中間のリダイレクターページにすることはできません。

1. サードパーティのサイトで adbox または flashbox のみを使用している場合。

   次の 2 つの方法があります（最適な方法を決めるには、クライアントサービスマネージャーに相談してください）。

   * ファーストパーティ Cookie およびサードパーティ Cookie を有効にする。

     ファーストパーティ Cookie およびサードパーティ Cookie は、flashbox と動的クリエイティブで必要になります。

   * サードパーティ Cookie のみを有効にする。

     この方法は、adbox の実装がオンサイトのターゲット化なしで使用されている場合にのみ適していますが、こうしたケースはまれです。

### ファーストパーティ Cookie の動作

ファーストパーティ Cookie は `clientdomain.com` に保存されます。ここでは、`clientdomain` がお客様のドメインになります。

at.js によって `mboxSession ID` が生成され、Cookie に保存されます。最初の の応答にはオファーが含まれています。また、アプリケーションによって生成された `mboxPC ID` を の Cookie に保存するための JavaScript も含まれています。

>[!NOTE]
>
>The `AMCV_###@AdobeOrg` ファーストパーティ Cookie は、常に訪問者 ID と共にExperience Cloudされます。

### サードパーティ Cookie の動作

サードパーティ Cookie は `clientcode.tt.omtrdc.net` に保存され、ファーストパーティ Cookie は `clientdomain.com` に保存されます。ここでは、`clientdomain` がお客様のドメインになります。

at.js によって `mboxSession ID` が生成されます。最初の場所のリクエストは、HTTP 応答ヘッダーを返します。このヘッダーによって、`mboxSession` および `mboxPC` という名前のサードパーティ Cookie の設定が試行され、リダイレクトリクエストが追加のパラメーター（`mboxXDomainCheck=true`）と共に戻されます。

ブラウザーがサードパーティ Cookie を受け入れる場合、リダイレクトリクエストにはそれらの Cookie が含まれており、オファーが返されます。

ブラウザーがサードパーティ Cookie を拒否する場合、リダイレクトリクエストにはそれらの Cookie が含まれておらず、ページのすべての場所についてデフォルトのコンテンツが表示されます。Cookie が設定されていないので、すべてのページリクエストに対して、これと同じ処理がおこなわれます。

### サードパーティ Cookie とファーストパーティ Cookie の動作

サードパーティ Cookie は `clientcode.tt.omtrdc.net` に保存され、ファーストパーティ Cookie は `clientdomain.com` に保存されます。ここでは、`clientdomain` がお客様のドメインになります。

at.js によって `mboxSession ID` が生成されます。最初の場所のリクエストは、HTTP 応答ヘッダーを返します。このヘッダーによって、`mboxSession` および `mboxPC` という名前のサードパーティ Cookie の設定が試行され、リダイレクトリクエストが追加のパラメーター（`mboxXDomainCheck=true`）と共に戻されます。

ブラウザーがサードパーティ Cookie を受け入れる場合、リダイレクトリクエストにはそれらの Cookie が含まれており、オファーが返されます。

一部のブラウザーではサードパーティ Cookie が拒否されます。サードパーティ Cookie がブロックされた場合でも、ファーストパーティ Cookie は引き続き動作します。[!DNL Target] でサードパーティ Cookie の設定が試行され、設定できなかった場合、[!DNL Target] はクライアントの特定のドメインのみを追跡できます。サードパーティ Cookie がブロックされた場合、クロスドメインのリンクに `mboxSession` が追加されていない限り、クロスドメイントラッキングは動作しません。この場合、別のファーストパーティ Cookie が設定され、前のドメインのファーストパーティ Cookie と同期されます。

## Cookie の設定

Cookie にはいくつかのデフォルト設定があります。cookie の期間を除き、必要に応じてこれらの設定を変更できます。Cookie の設定を変更する場合は、アカウント担当者にお問い合わせください。

| 設定 | 情報 |
|--- |--- |
| cookie 名 | mbox。 |
| cookie ドメイン | コンテンツを提供するドメインの 2 番目および最上位のレベルです。会社のドメインなので、cookie はファーストパーティ cookie になります。例: `mycompany.com`. |
| サーバードメイン | `clientcode.tt.omtrdc.net`。アカウントのクライアントコードを使用します。 |
| cookie の期間 | cookie が訪問者のブラウザーに残る期間は、訪問者が最後にログインしてから 2 年間です。<P>The `deviceIdLifetime` の設定は上書き可能です： [at.js バージョン 2.3.1 以降](../atjs/target-atjs-versions.md). 詳しくは、[targetGlobalSettings()](../../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。 |
| P3P ポリシー | ほとんどのブラウザーのデフォルト設定の要求に従って、cookie は P3P ポリシーに基づいて発行されます。P3P ポリシーはブラウザーに対して、cookie を扱うユーザーおよびその情報の使用方法を指示します。 |

Cookie は、キャンペーンでの訪問者のエクスペリエンスを管理するための様々な値を保持します。

| 値 | 定義 |
|--- |--- |
| session ID | ユーザーセッションの一意の ID。デフォルトでは、30 分間存続します。 |
| pc ID | 訪問者のブラウザーの半永久的な ID。14 日間存続します。 |
| check | 訪問者が cookie をサポートするかどうかを判別するために使用される簡単なテスト値。訪問者がページをリクエストするたびに設定されます。 |
| disable | 訪問者の読み込み時間がAdobe Experience Platform Web SDK または at.js ファイルで設定されているタイムアウトを超えた場合に設定されます。 デフォルトでは、1 時間存続します。 |

## 影響 [!DNL Target] Apple WebKit 追跡の変更による Safari 訪問者の場合

次の点に注意してください。

### 方法 [!DNL Adobe Target] 追跡作業？

| Cookie | 詳細 |
|--- |--- |
| ファーストパーティドメイン | これは、 [!DNL Target] 顧客。  「mbox」Cookie はお客様のドメインで設定されます。 |
| サードパーティ追跡 | サードパーティ追跡は、 [!DNL Target] および [!DNL Adobe Audience Manager] (AAM)。  サードパーティ追跡には、クロスサイトスクリプティング技法が必要となります。[!DNL Target] では、`clientcode.tt.omtrd.net` ドメインで設定される「mboxSession」と「mboxPC」の 2 つの Cookie を使用します。 |

### Apple のアプローチについて

Apple の発表内容：

「Intelligent Tracking Prevention は、Cookie およびその他の Web サイトデータを制限してクロスサイト追跡を減らすための WebKit の新機能です。」

「これはクロスサイト追跡と呼ばれ、`example-tracker.com` によって使用される Cookie はサードパーティ Cookie と呼ばれます。当社のテストでは、人気のある Web サイトで 70 を超えるこのようなトラッカーが見つかりました。これらのトラッカーはすべて、認識されることなくユーザーのデータを収集しています。」

| アプローチ | 詳細 |
|--- |--- |
| Intelligent tracking prevention | 詳しくは、WebKit オープンソース Web ブラウザーエンジン Web サイトの [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) を参照してください。 |
| Cookie | Safari における Cookie の処理方法：<ul><li>ユーザーが直接アクセスするドメインにないサードパーティ Cookie は保存されません。この動作は新しいものではありません。既に Safari ではサードパーティ Cookie はサポートされていません。</li><li>ユーザーが直接アクセスするドメインで設定されたサードパーティ Cookie は、24 時間後に消去されます。</li><li>ファーストパーティドメインがサイトを横断してユーザーを追跡していると分類されている場合、そのファーストパーティ Cookie は 30 日後に削除されます。この問題は、異なるドメインにオンラインでユーザーを送信する大企業で起こる可能性があります。Apple では、これらのドメインがどのように分類されるか、またドメインがサイトを横断してユーザーを追跡していると分類されているかどうかを調べる方法について明確にしていません。</li></ul> |
| 機械学習によるクロスサイトドメインの識別 | Apple の発表内容：<P>機械学習分類子：非公開で管理されるトップドメインがサイトを横断してユーザーを追跡できるかどうかについて、収集された統計情報に基づいて分類するために、機械学習モデルが使用されます。収集された様々な統計情報から、多数の一意のドメインの下位にあるサブリソース、多数の一意のドメインの下位にあるサブフレーム、多数の一意のドメインへのリダイレクト、という 3 つのベクトルが現在の追跡方法に基づく分類に対する強力なシグナルとなることがわかりました。すべてのデータ収集と分類はデバイス上でおこなわれます。<P>ただし、ユーザーがトップドメイン（ファーストパーティドメインと呼ばれることもあります）である example.com で何らかのアクションを起こした場合、Intelligent Tracking Prevention は、これをユーザーがこの Web サイトに関心があるシグナルと見なし、次のタイムラインに示すように、その動作を一時的に調整します。<P>このユーザーが過去 24 時間の間に example.com で何らかのアクションを起こしていた場合、`example.com` がサードパーティのときはその Cookie を使用できます。これにより、「Y で X アカウントを使用してサインインする」ログインシナリオが可能になります。<ul><li>トップレベルドメインとして訪問されるドメインは影響を受けません。例えば、OKTA のようなサイト</li><li>複数の一意のドメインを横断する現在のページのサブドメインまたはサブフレームであるドメインを識別します </li></ul> |

### アドビが受ける影響

| 影響を受ける機能 | 詳細 |
|--- |--- |
| オプトアウトのサポート | Apple の WebKit 追跡における変更により、オプトアウトのサポートが影響を受けます。<P>[!DNL Target] のオプトアウトでは、`clientcode.tt.omtrdc.net` ドメインの Cookie が使用されます。詳しくは、「[プライバシー](/help/dev/before-implement/privacy/privacy.md)」を参照してください。<P>[!DNL Target] は、次の 2 つのオプトアウトをサポートします。<ul><li>クライアントごと（クライアントがオプトアウトリンクを管理します）。</li><li>1 つは、Adobeを介し、すべてのユーザーをオプトアウトします [!DNL Target] の機能をすべてのお客様に適用できます。</li></ul>どちらの方法でもサードパーティ Cookie が使用されます。 |
| [!DNL Target] アクティビティ | 顧客は、 [プロファイルの有効期間長](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html) その [!DNL Target] アカウント — 最大 90 日間。 お客様のドメインがサイトを横断してユーザーを追跡しているとマークされているので、アカウントのプロファイル有効期間が 30 日を超え、ファーストパーティ cookie が消去されると、Safari 訪問者の行動が [!DNL Target]:<P>**[!DNL Target]レポート**： Safari ユーザーがアクティビティに参加してから 30 日後に戻り、コンバージョンを達成すると、そのユーザーは、2 人の訪問者と 1 つのコンバージョンとしてカウントされます。<P>この動作は、 [!DNL Analytics] を使用します (A4T)。<P>**プロファイルおよびアクティビティメンバーシップ**：<ul><li>プロファイルデータは、ファーストパーティ Cookie の有効期限が切れた時点で消去されます。</li><li>アクティビティメンバーシップは、ファーストパーティ Cookie の有効期限が切れた時点で消去されます。</li><li> [!DNL Target]サードパーティ Cookie 実装またはファーストパーティおよびサードパーティ Cookie 実装を使用しているアカウントの Safari では、 は動作しません。この動作は新しいものではありません。Safari では、サードパーティ Cookie を以前から許可していません。</li></ul><P>**候補**：顧客ドメインがセッションをまたいで訪問者を追跡しているとマークされる可能性が懸念される場合、でプロファイルの有効期間を 30 日以下に設定するのが安全です。 [!DNL Target]. これにより、ユーザーは Safari と他のすべてのブラウザーで同様に追跡されます。 |
