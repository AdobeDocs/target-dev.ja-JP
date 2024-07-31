---
keywords: 概要とリファレンス，webkit, cookie, ファーストパーティ，サードパーティ，ファーストパーティ，サードパーティ，$8
description: Target の cookie の動作（ファーストパーティ cookie、ファーストパーティ cookie を使用したサードパーティ cookie、またはサードパーティ cookie のみ）について説明します。
title: Target の Cookie に関する情報はどこで入手できますか？
feature: at.js
role: Developer
source-git-commit: 39f390a0e5eedf8c6957333759d31d96ed11b321
workflow-type: tm+mt
source-wordcount: '1580'
ht-degree: 53%

---

# Target の Cookie

Cookie の動作は、その Cookie がファーストパーティ Cookie であるか、ファーストパーティ Cookie を伴うサードパーティ Cookie であるか、サードパーティ Cookie のみであるかによって異なります。

>[!NOTE]
>
>このトピックには、`mboxSession` および `mboxPC` に関する情報が含まれています。実装のベストプラクティスでは、機密情報を cookie データ（`mboxSession` または `mboxPC`）にリンクしたり保存したりしないことをお勧めします。

[Target の Cookie の削除 ](/help/dev/before-implement/privacy/cookie-deleting.md) も参照してください。

## ファーストパーティ Cookie またはサードパーティ Cookie を使用するタイミング

サイトの設定によって、どの Cookie を使用するかが決まります。ファーストパーティ Cookie およびサードパーティ Cookie を理解するには、Target の仕組みを理解することが役立ちます。詳しくは [Adobe Targetの仕組み ](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html) を参照してください。

Cookie について、3 つの主要な使用例を次に示します。

1. 1 つのドメインの場合。

   すべてのテストは、1 つの最上位ドメイン（`www.domain.com`、`store.domain.com`、`anysub.domain.com` など）内で行われます。

   アプローチ：ファーストパーティ cookie のみを使用します（デフォルト）。

1. ユーザーが異なるドメインにアクセスし、それらのドメイン間における行動を追跡し、テストする場合。

   例：買い物をするためにお客様のサイトにアクセスしたユーザーが、Yahoo ストアを経由してチェックアウトする場合。次の 3 つの方法があります（最適な方法を決めるには、アカウント担当者に相談してください）。

   * ファーストパーティ Cookie およびサードパーティ Cookie を有効にする。
   * サードパーティのみを有効にする（まれですが、mbox cookie をドメインから除外できるという利点があります）。
   * ファーストパーティ Cookie のみを有効にし、異なるドメインにアクセスするときに `mboxSession` パラメーターを渡す。

     `mboxSession` パラメーターは、ランディングページに渡し、JavaScript ライブラリ（Adobe Experience Platform Web SDK または at.js）から参照する必要があります。 中間のリダイレクターページにすることはできません。

1. サードパーティサイトで adbox または flashbox のみを使用している場合。

   2 つの方法（アカウント担当者と協力して最適な方法を決定する）:

   * ファーストパーティ Cookie およびサードパーティ Cookie を有効にする。

     ファーストパーティ Cookie およびサードパーティ Cookie は、flashbox と動的クリエイティブで必要になります。

   * サードパーティ Cookie のみを有効にする。

     この方法は、adbox の実装がオンサイトのターゲット化なしで使用されている場合にのみ適していますが、こうしたケースはまれです。

## ファーストパーティ Cookie の動作

ファーストパーティ cookie はclientdomain.comに格納されます。ここで、`clientdomain` はドメインです。

JavaScript ライブラリは、`mboxSession ID` を生成して Target の Cookie に保存します。 最初の mbox 応答には、オファーと、アプリケーションで生成された `mboxPC ID` ファーを mbox cookie に保存するためのJavaScriptが含まれています。

>[!NOTE]
>
>AMCV_###@AdobeOrg ファーストパーティ cookie は、常にExperience Cloud訪問者 ID に設定されます。

## サードパーティ Cookie の動作

サードパーティ Cookie はclientcode.tt.omtrdc.netに格納され、ファーストパーティ Cookie はclientdomain.comに格納されます。ここで、`clientdomain` はドメインです。

JavaScript ライブラリが `mboxSession ID` を生成します。 最初の場所のリクエストは、HTTP 応答ヘッダーを返します。このヘッダーによって、`mboxSession` および `mboxPC` という名前のサードパーティ Cookie の設定が試行され、リダイレクトリクエストが追加のパラメーター（`mboxXDomainCheck=true`）と共に戻されます。

ブラウザーがサードパーティ Cookie を受け入れる場合、リダイレクトリクエストにはそれらの Cookie が含まれており、オファーが返されます。

ブラウザーがサードパーティ Cookie を拒否する場合、リダイレクトリクエストにはそれらの Cookie が含まれておらず、ページのすべての場所についてデフォルトのコンテンツが表示されます。Cookie が設定されていないので、すべてのページリクエストに対して、これと同じ処理がおこなわれます。

>[!NOTE]
>
>サードパーティ cookie がブロックされていない場合、demdex.net cookie が設定されます。

## ファーストパーティ Cookie を伴うサードパーティ Cookie の動作

サードパーティ Cookie はclientcode.tt.omtrdc.netに格納され、ファーストパーティ Cookie はclientdomain.comに格納されます。ここで、`clientdomain` はドメインです。

JavaScript ライブラリが `mboxSession ID` を生成します。 最初の場所のリクエストは、HTTP 応答ヘッダーを返します。このヘッダーによって、`mboxSession` および `mboxPC` という名前のサードパーティ Cookie の設定が試行され、リダイレクトリクエストが追加のパラメーター（`mboxXDomainCheck=true`）と共に戻されます。

ブラウザーがサードパーティ Cookie を受け入れる場合、リダイレクトリクエストにはそれらの Cookie が含まれており、オファーが返されます。

一部のブラウザーではサードパーティ Cookie が拒否されます。サードパーティ Cookie がブロックされた場合でも、ファーストパーティ Cookie は引き続き動作します。Target は、サードパーティ Cookie の設定を試みます。設定できない場合、Target は、クライアントの特定のドメイン上でのみ追跡できます。 サードパーティ cookie がブロックされている場合、ドメインをまたぐリンクに `mboxSession` が追加されていない限り、クロスドメイントラッキングは機能しません。 この場合、別のファーストパーティ Cookie が設定され、前のドメインのファーストパーティ Cookie と同期されます。

## Cookie の設定

Cookie にはいくつかのデフォルト設定があります。cookie の期間を除き、必要に応じてこれらの設定を変更できます。 Cookie の設定を変更する場合は、アカウント担当者にお問い合わせください。

| 設定 | 情報 |
|--- |--- |
| cookie 名 | mbox。 |
| cookie ドメイン | コンテンツを提供するドメインの 2 番目および最上位のレベルです。cookie は会社のドメインから提供されるので、ファーストパーティ cookie です。<br /> 例：`mycompany.com`。 |
| サーバードメイン | `clientcode.tt.omtrdc.net`。アカウントのクライアントコードを使用します。 |
| cookie の期間 | cookie は、最後にログインしてから 2 週間後に訪問者のブラウザーに残ります。 cookie の期間は変更できません。 |
| P3P ポリシー | ほとんどのブラウザーのデフォルト設定の要求に従って、cookie は P3P ポリシーに基づいて発行されます。P3P ポリシーは、cookie を提供するブラウザーに対して、および情報の使用方法を示します。 |

Cookie には、訪問者がキャンペーンをエクスペリエンスとして体験する方法を管理するために、様々な値が保持されています。

| 値 | 定義 |
|--- |--- |
| session ID | ユーザーセッションの一意の ID。デフォルトでは、この ID は 30 分間保持されます。 |
| pc ID | 訪問者のブラウザーの半永久的な ID。14 日間存続します。 |
| check | 訪問者が cookie をサポートするかどうかを判別するために使用される簡単なテスト値。訪問者がページをリクエストするたびに設定されます。 |
| disable | 訪問者の読み込み時間がJavaScript ライブラリファイルで設定されているタイムアウトを超えた場合に設定されます。 デフォルトでは、この値は 1 時間続きます。 |

## Safari 訪問者の Target に対する Apple WebKit 追跡の変更の影響

**Adobe Targetのトラッキングの仕組み**

| Cookie | 詳細 |
|--- |--- |
| ファーストパーティドメイン | Target のお客様向けの標準実装。 「mbox」Cookie はお客様のドメインで設定されます。 |
| サードパーティ追跡 | サードパーティトラッキングは、Target およびAdobe Audience Manager（AAM）での広告およびターゲティングのユースケースにとって重要です。 サードパーティトラッキングには、クロスサイトスクリプティング技術が必要です。 Target では、`clientcode.tt.omtrd.net` ドメインで設定される「mboxSession」と「mboxPC」の 2 つの Cookie を使用します。 |

**Apple のアプローチについて**

Apple の発表内容：

「Intelligent Tracking Prevention は、Cookie およびその他の Web サイトデータを制限してクロスサイト追跡を減らすための WebKit の新機能です。」

「これはクロスサイト追跡と呼ばれ、`example-tracker.com` によって使用される Cookie はサードパーティ Cookie と呼ばれます。当社のテストでは、人気のある Web サイトで 70 を超えるこのようなトラッカーが見つかりました。これらのトラッカーはすべて、認識されることなくユーザーのデータを収集しています。」

| アプローチ | 詳細 |
|--- |--- |
| Intelligent tracking prevention | 詳しくは、WebKit オープンソース Web ブラウザーエンジン Web サイトの [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) を参照してください。 |
| Cookie | Safari における Cookie の処理方法：<ul><li>ユーザーが直接アクセスするドメインにないサードパーティ Cookie は保存されません。この動作は新しいものではありません。既に Safari ではサードパーティ Cookie はサポートされていません。</li><li>ユーザーが直接アクセスするドメインで設定されたサードパーティ Cookie は、24 時間後に消去されます。</li><li>ファーストパーティドメインがサイトを横断してユーザーを追跡していると分類されている場合、そのファーストパーティ Cookie は 30 日後に削除されます。この問題は、異なるドメインにオンラインでユーザーを送信する大企業で起こる可能性があります。Appleでは、これらのドメインの正確な分類方法や、クロスサイトでドメインがトラッキングユーザーとして分類されたかどうかを判断する方法について明らかにしていません。</li></ul> |
| 機械学習によるクロスサイトドメインの識別 | Appleから：<br /> 機械学習分類子：機械学習モデルは、収集された統計情報に基づいて、ユーザーのクロスサイトを追跡できる、非公開最上位のドメインを分類するために使用されます。 収集された様々な統計情報から、多数の一意のドメインの下位にあるサブリソース、多数の一意のドメインの下位にあるサブフレーム、多数の一意のドメインへのリダイレクト、という 3 つのベクトルが現在の追跡方法に基づく分類に対する強力なシグナルとなることがわかりました。すべてのデータ収集と分類はデバイス上でおこなわれます。<br /> ただし、ユーザーが `example.com` をトップドメイン（ファーストパーティドメインと呼ばれることが多い）としてやり取りする場合、インテリジェントトラッキング防止は、ユーザーが web サイトに興味を持っていることを示すシグナルと見なし、このタイムラインに示すように一時的に動作を調整します。<br /> 過去 24 時間 `example.com` にユーザーがやり取りした場合、`example.com` がサードパーティである限り、その Cookie は使用できます。 これにより、「Y で X アカウントを使用してログイン」というログインシナリオが可能になります。<ul><li>最上位ドメインとしてアクセスされたドメインは影響を受けません。 例えば、OKTA のようなサイト</li><li>複数の一意のドメインをまたいで、現在のページのサブドメインまたはサブフレームであるドメインを識別します。</li></ul> |

**Adobeはどのような影響を受けますか？**

| 影響を受ける機能 | 詳細 |
|--- |--- |
| オプトアウトのサポート | Apple の WebKit 追跡における変更により、オプトアウトのサポートが影響を受けます。<br />Target のオプトアウトでは、`clientcode.tt.omtrdc.net` ドメインの Cookie が使用されます。詳しくは、「[プライバシー](/help/dev/before-implement/privacy/privacy.md)」を参照してください。<br />Target では、次の 2 種類のオプトアウトがサポートされます。<ul><li>クライアントごと（クライアントがオプトアウトリンクを管理します）。</li><li>アドビ経由。すべてのお客様のすべての Target 機能からユーザーをオプトアウトします。</li></ul>どちらの方法でもサードパーティ Cookie が使用されます。 |
| Target アクティビティ | 顧客は、Target アカウントに対して [ プロファイルの有効期間 ](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html) を選択できます（最大 90 日）。 懸念されるのは、アカウントのプロファイルの有効期間が 30 日を超え、顧客のドメインがクロスサイトでトラッキングユーザーとしてマークされているのでファーストパーティ cookie がパージされた場合、Target の次の領域で Safari 訪問者の行動が影響を受けることです。<br />**[!UICONTROL Target reports]**:Safari ユーザーがアクティビティにエントリし、30 日後に戻った後にコンバージョンを行った場合、そのユーザーは 2 人のと 1 つのコンバージョンにカウントされます。<br /> この動作は、Analytics をレポートソースとして使用するアクティビティ（A4T）でも同じです。<br />**[!UICONTROL Profile & activity membership]**:<ul><li>プロファイルデータは、ファーストパーティ Cookie の有効期限が切れた時点で消去されます。</li><li>アクティビティメンバーシップは、ファーストパーティ Cookie の有効期限が切れた時点で消去されます。</li><li> サードパーティ Cookie 実装またはファーストパーティおよびサードパーティ Cookie 実装を使用しているアカウントの Safari では、Target は動作しません。この動作は新しいものではありません。Safari では、しばらくサードパーティ cookie を許可していません。</li></ul><br />**[!UICONTROL Suggestions]**：顧客ドメインがクロスセッションで 1 つのトラッキング訪問者としてマークされる可能性が懸念される場合は、Target でプロファイルの有効期間を 30 日以内に設定するのが最も安全です。 この制限により、ユーザーは Safari およびその他のすべてのブラウザーで同様に追跡されます。 |
