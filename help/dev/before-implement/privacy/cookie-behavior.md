---
keywords: 概要と参照、web キット、Cookie、ファーストパーティ、サードパーティ、ファーストパーティ、サードパーティ，
description: ' [!DNL Target] Cookieの動作（ファーストパーティ Cookie、ファーストパーティ Cookieを使用したサードパーティ Cookie、またはサードパーティ Cookieのみ）について説明します。'
title: ' [!DNL Target] Cookieに関する情報はどこで入手できますか？'
feature: at.js
exl-id: d44e02ce-8920-4130-bcad-699ca77c0dad
TQID: https://experienceleague.adobe.com/Uc9Gb06t9DIkvBvLQJ9ZhopE8pyJovjyrQdsUmFD9-o
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d3cdead0-685a-4489-9250-4bb709942f66
  - id: eb30f47f-d87a-400f-8f78-63ce7979ff56
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1692
ht-degree: 49%

---

# [!DNL Target]個のCookie

Cookie の動作は、その Cookie がファーストパーティ Cookie であるか、ファーストパーティ Cookie を伴うサードパーティ Cookie であるか、サードパーティ Cookie のみであるかによって異なります。

>[!NOTE]
>
>[!DNL Target]が使用する様々なCookieについて詳しくは、*Experience Cloud中央インターフェイスコンポーネントガイド*&#x200B;の[[!DNL Adobe Target] cookie](https://experienceleague.adobe.com/docs/core-services/interface/administration/ec-cookies/cookies-target.html?lang=ja){target=_blank}を参照してください。
>
>このトピックには、`mboxSession` および `mboxPC` に関する情報が含まれています。 実装のベストプラクティスでは、機密情報をcookie データ `mboxSession`または`mboxPC`とリンクまたは保存しないことを推奨します。

[cookie [!DNL Target] を削除](cookie-deleting.md)も参照してください。

## ファーストパーティまたはサードパーティのCookieを使用するタイミング

サイトの設定によって、どの Cookie を使用するかが決まります。 ファーストパーティおよびサードパーティのCookieを理解する際に、[!DNL Target]がどのように機能するかを理解すると便利です。 詳細については、[How [!DNL Adobe] [!DNL Target]Works](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html)を参照してください。

Cookie について、3 つの主要な使用例を次に示します。

1. 1 つのドメインの場合。

   すべてのテストは、1つのトップレベルドメイン （`www.domain.com`、`store.domain.com`、`anysub.domain.com`など）内で実行されます。

   アプローチ：1st パーティ Cookie （デフォルト）のみを使用します。

1. ユーザーが異なるドメインにアクセスし、それらのドメイン間における行動を追跡し、テストする場合。

   例：買い物をするためにお客様のサイトにアクセスしたユーザーが、Yahoo ストアを経由してチェックアウトする場合。 次の 3 つの方法があります（最適な方法を決めるには、アカウント担当者に相談してください）。

   * ファーストパーティ Cookie およびサードパーティ Cookie を有効にする。
   * サードパーティのみを有効にする（まれですが、mbox Cookieをドメインから除外するメリットがあります）。
   * ファーストパーティ Cookie のみを有効にし、異なるドメインにアクセスするときに `mboxSession` パラメーターを渡す。

     `mboxSession` パラメーターは、ランディングページに渡され、JavaScript ライブラリ（Adobe Experience Platform Web SDKまたはat.js）から参照される必要があります。 中間のリダイレクターページにすることはできません。

1. サードパーティサイトで adbox または flashbox のみを使用している場合。

   2つのアプローチ（担当者と協力して、最適なアプローチを決定します）:

   * ファーストパーティ Cookie およびサードパーティ Cookie を有効にする。

     ファーストパーティ Cookie およびサードパーティ Cookie は、flashbox と動的クリエイティブで必要になります。

   * サードパーティ Cookie のみを有効にする。

     この方法は、adbox の実装がオンサイトのターゲティングなしで使用されている場合にのみ適していますが、こうしたケースはまれです。

## ファーストパーティ Cookie の動作

1st パーティ Cookieはclientdomain.comに保存され、`clientdomain`はお客様のドメインです。

JavaScript ライブラリは`mboxSession ID`を生成し、[!DNL Target] Cookieに保存します。 最初のmbox応答には、オファーと、アプリケーションによって生成された`mboxPC ID`をmbox cookieに格納するJavaScriptが含まれています。

>[!NOTE]
>
>AMCV_###@AdobeOrg ファーストパーティ Cookieは、常にExperience Cloud訪問者IDで設定されます。

## サードパーティ Cookie の動作

サードパーティ Cookieはclientcode.tt.omtrdc.netに保存され、ファーストパーティ Cookieはclientdomain.comに保存されます。ここで、`clientdomain`はお客様のドメインです。

JavaScript ライブラリが`mboxSession ID`を生成します。 最初の場所のリクエストは、HTTP 応答ヘッダーを返します。このヘッダーによって、`mboxSession` および `mboxPC` という名前のサードパーティ Cookie の設定が試行され、リダイレクトリクエストが追加のパラメーター（`mboxXDomainCheck=true`）と共に戻されます。

ブラウザーがサードパーティ Cookie を受け入れる場合、リダイレクトリクエストにはそれらの Cookie が含まれており、オファーが返されます。

ブラウザーがサードパーティ Cookie を拒否する場合、リダイレクトリクエストにはそれらの Cookie が含まれておらず、ページのすべての場所についてデフォルトのコンテンツが表示されます。 Cookie が設定されていないので、すべてのページリクエストに対して、これと同じ処理がおこなわれます。

>[!NOTE]
>
>サードパーティ Cookie がブロックされない場合、demdex.net Cookie が設定されます。

## サードパーティ Cookie とファーストパーティ Cookie の動作

サードパーティ Cookieはclientcode.tt.omtrdc.netに保存され、ファーストパーティ Cookieはclientdomain.comに保存されます。ここで、`clientdomain`はお客様のドメインです。

JavaScript ライブラリが`mboxSession ID`を生成します。 最初の場所のリクエストは、HTTP 応答ヘッダーを返します。このヘッダーによって、`mboxSession` および `mboxPC` という名前のサードパーティ Cookie の設定が試行され、リダイレクトリクエストが追加のパラメーター（`mboxXDomainCheck=true`）と共に戻されます。

ブラウザーがサードパーティ Cookie を受け入れる場合、リダイレクトリクエストにはそれらの Cookie が含まれており、オファーが返されます。

一部のブラウザーではサードパーティ Cookie が拒否されます。 サードパーティ Cookie がブロックされた場合でも、ファーストパーティ Cookie は引き続き動作します。 [!DNL Target] でサードパーティ Cookie の設定が試行され、設定できなかった場合、[!DNL Target] はクライアントの特定のドメインのみを追跡できます。 サードパーティ Cookieがブロックされている場合、ドメインを横断するリンクに`mboxSession`が追加されない限り、クロスドメイン トラッキングは機能しません。 この場合、別のファーストパーティ Cookie が設定され、前のドメインのファーストパーティ Cookie と同期されます。

## Cookie の設定

Cookie にはいくつかのデフォルト設定があります。 必要に応じて、Cookieの期間を除いてこれらの設定を変更できます。 Cookie の設定を変更する場合は、アカウント担当者にお問い合わせください。

| 設定 | 情報 |
|--- |--- |
| cookie 名 | mbox。 |
| cookie ドメイン | コンテンツを提供するドメインの 2 番目および最上位のレベルです。 会社のドメインから提供されるので、Cookieは1st パーティ Cookieです。<br />例：`mycompany.com`。 |
| サーバードメイン | `clientcode.tt.omtrdc.net`。アカウントのクライアントコードを使用します。 |
| cookie の期間 | Cookieは、最後のログインから2週間後も訪問者のブラウザーに残ります。 cookie の期間は変更できません。 |
| P3P ポリシー | ほとんどのブラウザーのデフォルト設定の要求に従って、cookie は P3P ポリシーに基づいて発行されます。 P3P ポリシーは、Cookieを提供しているブラウザーとその情報の使用方法を示します。 |

Cookieは、訪問者がキャンペーンを体験する方法を管理するために、様々な値を保持します。

| 値 | 定義 |
|--- |--- |
| session ID | ユーザーセッションの一意の ID。 デフォルトでは、このIDは30分続きます。 |
| pc ID | 訪問者のブラウザーの半永久的な ID。 14 日間存続します。 |
| check | 訪問者が cookie をサポートするかどうかを判別するために使用される簡単なテスト値。 訪問者がページをリクエストするたびに設定されます。 |
| disable | 訪問者の読み込み時間が、JavaScript ライブラリファイルで設定されたタイムアウトを超えているかどうかを設定します。 デフォルトでは、この値は1時間続きます。 |

## Apple WebKit トラッキングの変更によるSafari訪問者に対する[!DNL Target]への影響

**[!DNL Target]のトラッキングの仕組みはどのようになっていますか？**

| Cookie | 詳細 |
|--- |--- |
| ファーストパーティドメイン | [!DNL Target]のお客様の標準実装。 「mbox」Cookie はお客様のドメインで設定されます。 |
| サードパーティ追跡 | [!DNL Target]および[!DNL Adobe Audience Manager] （AAM）の広告とターゲティングのユースケースでは、サードパーティによるトラッキングが重要です。 サードパーティ追跡には、クロスサイトスクリプティング技法が必要となります。 [!DNL Target]は、`clientcode.tt.omtrd.net` ドメインで設定された「mboxSession」と「mboxPC」の2つのCookieを使用しています。 |

**Apple のアプローチについて**

Apple の発表内容：

「Intelligent Tracking Prevention は、Cookie およびその他の Web サイトデータを制限してクロスサイト追跡を減らすための WebKit の新機能です。」

「これはクロスサイト追跡と呼ばれ、`example-tracker.com` によって使用される Cookie はサードパーティ Cookie と呼ばれます。 当社のテストでは、人気のある Web サイトで 70 を超えるこのようなトラッカーが見つかりました。これらのトラッカーはすべて、認識されることなくユーザーのデータを収集しています。」

| アプローチ | 詳細 |
|--- |--- |
| Intelligent tracking prevention | 詳しくは、WebKit オープンソース Web ブラウザーエンジン Web サイトの [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) を参照してください。 |
| Cookie | Safari における Cookie の処理方法：<ul><li>ユーザーが直接アクセスするドメインにないサードパーティ Cookie は保存されません。 この動作は新しいものではありません。 既に Safari ではサードパーティ Cookie はサポートされていません。</li><li>ユーザーが直接アクセスするドメインで設定されたサードパーティ Cookie は、24 時間後に消去されます。</li><li>ファーストパーティドメインがサイトを横断してユーザーを追跡していると分類されている場合、そのファーストパーティ Cookie は 30 日後に削除されます。 この問題は、異なるドメインにオンラインでユーザーを送信する大企業で起こる可能性があります。 Appleは、これらのドメインがどのように分類されているか、またはドメインがクロスサイトのトラッキングユーザーとして分類されているかどうかをドメインがどのように判断できるかを明確にしていません。</li></ul> |
| 機械学習によるクロスサイトドメインの識別 | Appleから：<br />機械学習の分類子：収集された統計に基づいて、ユーザーのクロスサイトをトラッキングできる上位のプライベート制御ドメインを分類するために機械学習モデルが使用されます。 収集された様々な統計情報から、多数の一意のドメインの下位にあるサブリソース、多数の一意のドメインの下位にあるサブフレーム、多数の一意のドメインへのリダイレクト、という 3 つのベクトルが現在の追跡方法に基づく分類に対する強力なシグナルとなることがわかりました。 データの収集と分類はすべてオンデバイスで行われます。<br />ただし、ユーザーがトップドメインとして`example.com`と対話し、しばしばファーストパーティドメインと呼ばれる場合、インテリジェントなトラッキング防止は、ユーザーがweb サイトに興味を持っていることを示すシグナルと見なし、このタイムラインに示すように一時的に動作を調整します。<br /> ユーザーが過去24時間に`example.com`と対話した場合、`example.com`がサードパーティです。 この方法では、「YのX アカウントでログイン」ログインのシナリオを使用できます。<ul><li>トップレベルドメインとして訪問されたドメインは影響を受けません。 例えば、OKTA のようなサイト</li><li>複数の一意のドメインにまたがる、現在のページのサブドメインまたはサブフレームであるドメインを識別します。</li></ul> |

**影響を受けるのは[!DNL Adobe]さんですか？**

| 影響を受ける機能 | 詳細 |
|--- |--- |
| オプトアウトのサポート | Apple の WebKit 追跡における変更により、オプトアウトのサポートが影響を受けます。<br />Target のオプトアウトでは、`clientcode.tt.omtrdc.net` ドメインの Cookie が使用されます。 詳しくは、[&#x200B; プライバシー](privacy.md)を参照してください。<br />Targetは2つのオプトアウトをサポートしています。<ul><li>クライアントごと（クライアントがオプトアウトリンクを管理します）。</li><li>[!DNL Adobe]を介した1つは、すべての顧客のすべての[!DNL Target]機能からユーザーをオプトアウトします。</li></ul>どちらの方法でもサードパーティ Cookie が使用されます。 |
| Target アクティビティ | お客様は、[!DNL Target] アカウントの[&#x200B; プロファイルの有効期間](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html)を選択できます（最大90日間）。 懸念は、アカウントのプロファイルの有効期間が30日を超え、顧客のドメインがクロスサイトでユーザーを追跡するようにマークされているため、ファーストパーティ Cookieが消去された場合、Safari ユーザーの行動がTarget:<br />**ターゲットレポート**: Safari ユーザーがアクティビティに入り、30日後に戻ってコンバージョンした場合、そのユーザーは2人の訪問者と1つのコンバージョンとしてカウントされます。<br />この動作は、Analyticsをレポートソース （A4T）として使用するアクティビティと同じです。<br />**プロファイルとアクティビティメンバーシップ**:<ul><li>プロファイルデータは、ファーストパーティ Cookie の有効期限が切れた時点で消去されます。</li><li>アクティビティメンバーシップは、ファーストパーティ Cookie の有効期限が切れた時点で消去されます。</li><li> サードパーティ Cookieの実装またはファーストパーティ Cookieとサードパーティ Cookieの実装を使用しているアカウントでは、[!DNL Target]はSafariで機能しません。 この動作は新しいものではありません。 Safariはしばらくサードパーティ Cookieを許可していません。</li></ul><br />**提案**：顧客ドメインが1人のトラッキング訪問者としてクロスセッションでマークされる可能性がある場合は、Targetでプロファイルの有効期間を30日以内に設定するのが最も安全です。 この制限により、Safariやその他のすべてのブラウザーでユーザーが同じように追跡されます。 |
