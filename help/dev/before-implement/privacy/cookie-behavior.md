---
keywords: 概要とリファレンス， Webkit, Cookie，ファーストパーティ，サードパーティ，ファーストパーティ，サードパーティ，サードパーティ，
description: 詳細 [!DNL Target] Cookie の動作（ファーストパーティ Cookie、ファーストパーティ Cookie を伴うサードパーティ Cookie、またはサードパーティ Cookie のみ）。
title: ～に関する情報はどこで入手できるか [!DNL Target] クッキー？
feature: at.js
exl-id: d44e02ce-8920-4130-bcad-699ca77c0dad
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1599'
ht-degree: 57%

---

# [!DNL Target] cookie

Cookie の動作は、その Cookie がファーストパーティ Cookie であるか、ファーストパーティ Cookie を伴うサードパーティ Cookie であるか、サードパーティ Cookie のみであるかによって異なります。

>[!NOTE]
>
>に使用される様々な cookie の詳細 [!DNL Target]を参照してください。 [[!DNL Adobe Target] cookies](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-target.html?lang=ja){target=_blank} （内） *Experience Cloud中央インターフェイスコンポーネントガイド*.
>
>このトピックには、`mboxSession` および `mboxPC` に関する情報が含まれています。実装のベストプラクティスでは、機密情報を Cookie データにリンクしたり格納したりしないことをお勧めします。 `mboxSession` または `mboxPC`.

関連トピック [を削除します。 [!DNL Target] cookie](cookie-deleting.md).

## ファーストパーティ Cookie またはサードパーティ Cookie を使用するタイミング

サイトの設定によって、どの Cookie を使用するかが決まります。次の点を理解すると役に立ちます。 [!DNL Target] は、ファーストパーティ Cookie とサードパーティ Cookie を理解しようとする際に機能します。 詳しくは、 [方法 [!DNL Adobe] [!DNL Target] 仕事](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html) を参照してください。

Cookie について、3 つの主要な使用例を次に示します。

1. 1 つのドメインの場合。

   すべてのテストは、1 つのトップレベルドメイン (`www.domain.com`, `store.domain.com`, `anysub.domain.com`など ) を含める必要があります。

   方法：ファーストパーティ Cookie のみを使用します（デフォルト）。

1. ユーザーが異なるドメインにアクセスし、それらのドメイン間における行動を追跡し、テストする場合。

   例：買い物をするためにお客様のサイトにアクセスしたユーザーが、Yahoo ストアを経由してチェックアウトする場合。次の 3 つの方法があります（最適な方法を決めるには、アカウント担当者に相談してください）。

   * ファーストパーティ Cookie およびサードパーティ Cookie を有効にする。
   * サードパーティ Cookie のみを有効にする（まれにですが、ドメイン外に mbox Cookie を保持できるという利点があります）。
   * ファーストパーティ Cookie のみを有効にし、異なるドメインにアクセスするときに `mboxSession` パラメーターを渡す。

     The `mboxSession` パラメーターは、ランディングページに渡し、JavaScript ライブラリ (Adobe Experience Platform Web SDK または at.js) から参照する必要があります。 中間のリダイレクターページにすることはできません。

1. サードパーティサイトで adbox または flashbox のみを使用している場合。

   次の 2 つの方法があります（最適な方法を決定するには、アカウント担当者にお問い合わせください）。

   * ファーストパーティ Cookie およびサードパーティ Cookie を有効にする。

     ファーストパーティ Cookie およびサードパーティ Cookie は、flashbox と動的クリエイティブで必要になります。

   * サードパーティ Cookie のみを有効にする。

     この方法は、adbox の実装がオンサイトのターゲット化なしで使用されている場合にのみ適していますが、こうしたケースはまれです。

## ファーストパーティ Cookie の動作

ファーストパーティ Cookie は clientdomain.com に保存されます。ここでは、`clientdomain` がお客様のドメインになります。

JavaScript ライブラリは、 `mboxSession ID` そして、 [!DNL Target] cookie. 最初の mbox の応答にはオファーが含まれています。また、 `mboxPC ID` を mbox の Cookie に生成します。

>[!NOTE]
>
>AMCV_###@AdobeOrgファーストパーティ cookie は、常に訪問者 ID と共にExperience Cloudされます。

## サードパーティ Cookie の動作

サードパーティ Cookie は clientcode.tt.omtrdc.net に保存され、ファーストパーティ Cookie は clientdomain.com に保存されます。ここでは、`clientdomain` がお客様のドメインになります。

JavaScript ライブラリは、 `mboxSession ID`. 最初の場所のリクエストは、HTTP 応答ヘッダーを返します。このヘッダーによって、`mboxSession` および `mboxPC` という名前のサードパーティ Cookie の設定が試行され、リダイレクトリクエストが追加のパラメーター（`mboxXDomainCheck=true`）と共に戻されます。

ブラウザーがサードパーティ Cookie を受け入れる場合、リダイレクトリクエストにはそれらの Cookie が含まれており、オファーが返されます。

ブラウザーがサードパーティ Cookie を拒否する場合、リダイレクトリクエストにはそれらの Cookie が含まれておらず、ページのすべての場所についてデフォルトのコンテンツが表示されます。Cookie が設定されていないので、すべてのページリクエストに対して、これと同じ処理がおこなわれます。

>[!NOTE]
>
>サードパーティ Cookie がブロックされない場合、demdex.net Cookie が設定されます。

## サードパーティ Cookie とファーストパーティ Cookie の動作

サードパーティ Cookie は clientcode.tt.omtrdc.net に保存され、ファーストパーティ Cookie は clientdomain.com に保存されます。ここでは、`clientdomain` がお客様のドメインになります。

JavaScript ライブラリは、 `mboxSession ID`. 最初の場所のリクエストは、HTTP 応答ヘッダーを返します。このヘッダーによって、`mboxSession` および `mboxPC` という名前のサードパーティ Cookie の設定が試行され、リダイレクトリクエストが追加のパラメーター（`mboxXDomainCheck=true`）と共に戻されます。

ブラウザーがサードパーティ Cookie を受け入れる場合、リダイレクトリクエストにはそれらの Cookie が含まれており、オファーが返されます。

一部のブラウザーではサードパーティ Cookie が拒否されます。サードパーティ Cookie がブロックされた場合でも、ファーストパーティ Cookie は引き続き動作します。[!DNL Target] でサードパーティ Cookie の設定が試行され、設定できなかった場合、[!DNL Target] はクライアントの特定のドメインのみを追跡できます。サードパーティ Cookie がブロックされた場合、クロスドメイントラッキングは機能しません ( ただし、 `mboxSession` が追加されます。 この場合、別のファーストパーティ Cookie が設定され、前のドメインのファーストパーティ Cookie と同期されます。

## Cookie の設定

Cookie にはいくつかのデフォルト設定があります。cookie の期間を除き、必要に応じてこれらの設定を変更できます。 Cookie の設定を変更する場合は、アカウント担当者にお問い合わせください。

| 設定 | 情報 |
|--- |--- |
| cookie 名 | mbox。 |
| cookie ドメイン | コンテンツを提供するドメインの 2 番目および最上位のレベルです。会社のドメインなので、cookie はファーストパーティ cookie になります。<br />例: `mycompany.com`. |
| サーバードメイン | `clientcode.tt.omtrdc.net`。アカウントのクライアントコードを使用します。 |
| cookie の期間 | cookie が訪問者のブラウザーに残る期間は、最後のログインから 2 週間です。 cookie の期間は変更できません。 |
| P3P ポリシー | ほとんどのブラウザーのデフォルト設定の要求に従って、cookie は P3P ポリシーに基づいて発行されます。P3P ポリシーは、Cookie を提供しているブラウザーに対し、その情報の使用方法を示します。 |

cookie には、訪問者がキャンペーンを体験する方法を管理するための様々な値が保持されます。

| 値 | 定義 |
|--- |--- |
| session ID | ユーザーセッションの一意の ID。デフォルトでは、この ID は 30 分間存続します。 |
| pc ID | 訪問者のブラウザーの半永久的な ID。14 日間存続します。 |
| check | 訪問者が cookie をサポートするかどうかを判別するために使用される簡単なテスト値。訪問者がページをリクエストするたびに設定されます。 |
| disable | 訪問者の読み込み時間が JavaScript ライブラリファイルで設定されているタイムアウトを超えた場合に設定されます。 デフォルトでは、この値は 1 時間存続します。 |

## 影響 [!DNL Target] Apple WebKit 追跡の変更による Safari 訪問者の場合

**方法 [!DNL Target] 追跡作業？**

| Cookie | 詳細 |
|--- |--- |
| ファーストパーティドメイン | の標準的な実装 [!DNL Target] 顧客。 「mbox」Cookie はお客様のドメインで設定されます。 |
| サードパーティ追跡 | サードパーティ追跡は、 [!DNL Target] および [!DNL Adobe Audience Manager] (AAM)。 サードパーティ追跡には、クロスサイトスクリプティング技法が必要となります。[!DNL Target] では、`clientcode.tt.omtrd.net` ドメインで設定される「mboxSession」と「mboxPC」の 2 つの Cookie を使用します。 |
**Apple のアプローチについて**

Apple の発表内容：

「Intelligent Tracking Prevention は、Cookie およびその他の Web サイトデータを制限してクロスサイト追跡を減らすための WebKit の新機能です。」

「これはクロスサイト追跡と呼ばれ、`example-tracker.com` によって使用される Cookie はサードパーティ Cookie と呼ばれます。当社のテストでは、人気のある Web サイトで 70 を超えるこのようなトラッカーが見つかりました。これらのトラッカーはすべて、認識されることなくユーザーのデータを収集しています。」

| アプローチ | 詳細 |
|--- |--- |
| Intelligent tracking prevention | 詳しくは、WebKit オープンソース Web ブラウザーエンジン Web サイトの [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) を参照してください。 |
| Cookie | Safari における Cookie の処理方法：<ul><li>ユーザーが直接アクセスするドメインにないサードパーティ Cookie は保存されません。この動作は新しいものではありません。既に Safari ではサードパーティ Cookie はサポートされていません。</li><li>ユーザーが直接アクセスするドメインで設定されたサードパーティ Cookie は、24 時間後に消去されます。</li><li>ファーストパーティドメインがサイトを横断してユーザーを追跡していると分類されている場合、そのファーストパーティ Cookie は 30 日後に削除されます。この問題は、異なるドメインにオンラインでユーザーを送信する大企業で起こる可能性があります。Appleでは、これらのドメインがどのように正確に分類されているか、またはドメインがサイトを横断してユーザーを追跡しているかどうかを判断する方法について明確にしていません。</li></ul> |
| 機械学習によるクロスサイトドメインの識別 | Apple の発表内容：<br />機械学習分類子：非公開で管理される上位のドメインがサイトを横断してユーザーを追跡できるかどうかについて、収集された統計に基づいて分類するために、機械学習モデルが使用されます。 収集された様々な統計情報から、多数の一意のドメインの下位にあるサブリソース、多数の一意のドメインの下位にあるサブフレーム、多数の一意のドメインへのリダイレクト、という 3 つのベクトルが現在の追跡方法に基づく分類に対する強力なシグナルとなることがわかりました。すべてのデータ収集と分類はデバイス上でおこなわれます。<br />ただし、ユーザーが `example.com` トップドメイン（ファーストパーティドメインとも呼ばれる）は、Intelligent Tracking Prevention では、ユーザーが web サイトに興味を持ったことを示すシグナルと見なし、次のタイムラインに示すように動作を一時的に調整します。<br />ユーザーが `example.com` 過去 24 時間の間、 `example.com` はサードパーティです。 この方法を使用すると、「Y で X アカウントを使用してサインインする」ログインシナリオが可能になります。<ul><li>トップレベルドメインとして訪問されるドメインは影響を受けません。 例えば、OKTA のようなサイト</li><li>複数の一意のドメインを横断する現在のページのサブドメインまたはサブフレームであるドメインを識別します </li></ul> |

**の仕組み [!DNL Adobe] 影響を受けた？**

| 影響を受ける機能 | 詳細 |
|--- |--- |
| オプトアウトのサポート | Apple の WebKit 追跡における変更により、オプトアウトのサポートが影響を受けます。<br />Target のオプトアウトでは、`clientcode.tt.omtrdc.net` ドメインの Cookie が使用されます。詳しくは、「[プライバシー](privacy.md)」を参照してください。<br />Target では、次の 2 種類のオプトアウトがサポートされます。<ul><li>クライアントごと（クライアントがオプトアウトリンクを管理します）。</li><li>1 つの経由 [!DNL Adobe] すべての [!DNL Target] の機能をすべてのお客様に適用できます。</li></ul>どちらの方法でもサードパーティ Cookie が使用されます。 |
| Target アクティビティ | お客様は、Target アカウントの [プロファイルの有効期間長](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html) その [!DNL Target] アカウント（最大 90 日）。 お客様のドメインがサイトを横断してユーザーを追跡しているとマークされているので、アカウントのプロファイル有効期間が 30 日を超え、ファーストパーティ cookie が消去されると、Safari 訪問者の行動が Target の次の領域で影響を受けます。<br />**ターゲットレポート**:Safari ユーザーがアクティビティに参加し、30 日後に戻ってコンバージョンをおこなうと、そのユーザーは 2 人の訪問者と 1 つのコンバージョンとしてカウントされます。<br />この動作は、Analytics をレポートソースに使用する（A4T）アクティビティでも同じです。<br />**プロファイルとアクティビティのメンバーシップ**:<ul><li>プロファイルデータは、ファーストパーティ Cookie の有効期限が切れた時点で消去されます。</li><li>アクティビティメンバーシップは、ファーストパーティ Cookie の有効期限が切れた時点で消去されます。</li><li> [!DNL Target]サードパーティ Cookie 実装またはファーストパーティおよびサードパーティ Cookie 実装を使用しているアカウントの Safari では、 は動作しません。この動作は新しいものではありません。Safari では、しばらくの間、サードパーティ Cookie を許可していません。</li></ul><br />**候補**：顧客ドメインがセッションをまたいで訪問者を追跡しているとマークされている可能性が懸念される場合は、Target でプロファイルの有効期間を 30 日以下に設定するのが安全です。 この制限により、ユーザーは Safari と他のすべてのブラウザーで同様に追跡されます。 |
