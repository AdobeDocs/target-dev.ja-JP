---
keywords: at.js、2.0、1.x、cookie
description: ' [!DNL Adobe Target] at.js 2.xおよびat.js 1.xでのCookieの処理方法の詳細'
title: at.js Cookie
feature: at.js
exl-id: 154a844a-6855-4af7-8aed-0719b4c389f5
TQID: https://experienceleague.adobe.com/BRauW1ppIMya4aX-vTJDGZFCv1fijYgDuxbHjXCI6D8
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
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 929e1f10bc5dd0741f0fe28cd46435e680a4a308
workflow-type: tm+mt
source-wordcount: 1830
ht-degree: 67%

---

# at.js の cookie

at.js 2.xおよびat.js 1.*x* Cookieの動作に関する情報。

## at.js 2.x の Cookie の動作

at.js バージョン 2.x （バージョン 2.10.0以前）の場合、*1st パーティ Cookieのみがサポートされます*。 at.js 1.*x*&#x200B;と同様に、1st パーティ Cookieである「mbox」は`clientdomain.com`に保存され、`clientdomain`はドメインです。

at.js はセッション ID を生成し、Cookie に保存します。 最初の応答には、任意のアクティビティ情報と、[!DNL Target] サーバーによって生成された `TNT` または `PC ID` も含まれています。 続いて at.js は `TNT/PC ID` を Cookie に書き込みます。

`AMCV_###@AdobeOrg`のファーストパーティ Cookieは、Experience Cloud ID サービスによって常に設定されますが、`ECID`は[!DNL Target]要求で渡されます。

>[!NOTE]
>
>at.js バージョン 2.10.0以降では、ファーストパーティクッキーとクロスドメインクッキーの両方がサポートされています。

### サードパーティ Cookieとクロスドメイン追跡のサポート

クロスドメイン追跡を使用すると、ドメインが異なる 2 つの関連サイト上のセッション同士を単一のセッションとして確認できます。 `siteA.com` と `siteB.com` にまたがる [!DNL Target] アクティビティを作成し、訪問者がドメインを移るときに同じエクスペリエンスを維持できます。 この機能は、at.js 1.*x* サードパーティおよびファーストパーティ Cookieの動作に関連付けられます。

>[!NOTE]
>
>at.js バージョン 2.10.0以降では、サードパーティ Cookieとクロスドメイン トラッキングの両方がサポートされています。


## at.js 1.*x* cookieの動作

at.js バージョン 1.*x*&#x200B;の場合、Cookieの動作は、ファーストパーティ Cookie、ファーストパーティ Cookieを含むサードパーティ Cookie、またはサードパーティ Cookieのみによって異なります。

### ファーストパーティ Cookie またはサードパーティ Cookie を使用するタイミング

サイトの設定によって、どの Cookie を使用するかが決まります。 ファーストパーティおよびサードパーティのCookieを理解する際に、[!DNL Target]がどのように機能するかを理解すると便利です。 詳しくは、[How [!DNL Adobe Target] works](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html?lang=ja)を参照してください。

Cookie について、3 つの主要な使用例を次に示します。

1. 1 つのドメインの場合。

   すべてのテストは、1 つのトップレベルドメイン（`www.domain.com`、`store.domain.com`、`anysub.domain.com`、など）内で実施します。

   ファーストパーティ Cookie のみを使用します。 これはデフォルトの設定です。

1. ユーザーが異なるドメインにアクセスし、それらのドメイン間における行動を追跡し、テストする場合。

   例：買い物をするためにお客様のサイトにアクセスしたユーザーが、Yahoo ストアを経由してチェックアウトする場合。 次の 3 つの方法があります（最適な方法を決めるには、アカウント担当者に相談してください）。

   * ファーストパーティ Cookie およびサードパーティ Cookie を有効にする。
   * サードパーティ Cookie のみを有効にする（ほとんど利用されませんが、ドメイン外に at.js の Cookie を保持できる利点があります）。
   * ファーストパーティ Cookie のみを有効にし、異なるドメインにアクセスするときに `mboxSession` パラメーターを渡す。

     `mboxSession` パラメーターは、at.js を参照しているランディングページに渡す必要があります。 中間のリダイレクターページにすることはできません。

1. サードパーティのサイトで adbox または flashbox のみを使用している場合。

   次の 2 つの方法があります（最適な方法を決めるには、クライアントサービスマネージャーに相談してください）。

   * ファーストパーティ Cookie およびサードパーティ Cookie を有効にする。

     ファーストパーティ Cookie およびサードパーティ Cookie は、flashbox と動的クリエイティブで必要になります。

   * サードパーティ Cookie のみを有効にする。

     この方法は、adbox の実装がオンサイトのターゲティングなしで使用されている場合にのみ適していますが、こうしたケースはまれです。

### ファーストパーティ Cookie の動作

ファーストパーティ Cookie は `clientdomain.com` に保存されます。ここでは、`clientdomain` がお客様のドメインになります。

at.js によって `mboxSession ID` が生成され、Cookie に保存されます。 最初の の応答にはオファーが含まれています。また、アプリケーションによって生成された `mboxPC ID` を の Cookie に保存するための JavaScript も含まれています。

>[!NOTE]
>
>`AMCV_###@AdobeOrg`の1st パーティ Cookieは、常にExperience Cloud訪問者IDで設定されます。

### サードパーティ Cookie の動作

サードパーティ Cookie は `clientcode.tt.omtrdc.net` に保存され、ファーストパーティ Cookie は `clientdomain.com` に保存されます。ここでは、`clientdomain` がお客様のドメインになります。

at.js によって `mboxSession ID` が生成されます。 最初の場所のリクエストは、HTTP 応答ヘッダーを返します。このヘッダーによって、`mboxSession` および `mboxPC` という名前のサードパーティ Cookie の設定が試行され、リダイレクトリクエストが追加のパラメーター（`mboxXDomainCheck=true`）と共に戻されます。

ブラウザーがサードパーティ Cookie を受け入れる場合、リダイレクトリクエストにはそれらの Cookie が含まれており、オファーが返されます。

ブラウザーがサードパーティ Cookie を拒否する場合、リダイレクトリクエストにはそれらの Cookie が含まれておらず、ページのすべての場所についてデフォルトのコンテンツが表示されます。 Cookie が設定されていないので、すべてのページリクエストに対して、これと同じ処理がおこなわれます。

### サードパーティ Cookie とファーストパーティ Cookie の動作

サードパーティ Cookie は `clientcode.tt.omtrdc.net` に保存され、ファーストパーティ Cookie は `clientdomain.com` に保存されます。ここでは、`clientdomain` がお客様のドメインになります。

at.js によって `mboxSession ID` が生成されます。 最初の場所のリクエストは、HTTP 応答ヘッダーを返します。このヘッダーによって、`mboxSession` および `mboxPC` という名前のサードパーティ Cookie の設定が試行され、リダイレクトリクエストが追加のパラメーター（`mboxXDomainCheck=true`）と共に戻されます。

ブラウザーがサードパーティ Cookie を受け入れる場合、リダイレクトリクエストにはそれらの Cookie が含まれており、オファーが返されます。

一部のブラウザーではサードパーティ Cookie が拒否されます。 サードパーティ Cookie がブロックされた場合でも、ファーストパーティ Cookie は引き続き動作します。 [!DNL Target] でサードパーティ Cookie の設定が試行され、設定できなかった場合、[!DNL Target] はクライアントの特定のドメインのみを追跡できます。 サードパーティ Cookie がブロックされた場合、クロスドメインのリンクに `mboxSession` が追加されていない限り、クロスドメイントラッキングは動作しません。 この場合、別のファーストパーティ Cookie が設定され、前のドメインのファーストパーティ Cookie と同期されます。

## Cookie の設定

Cookie にはいくつかのデフォルト設定があります。 cookie の期間を除き、必要に応じてこれらの設定を変更できます。 Cookie の設定を変更する場合は、アカウント担当者にお問い合わせください。

| 設定 | 情報 |
|--- |--- |
| cookie 名 | mbox。 |
| cookie ドメイン | コンテンツを提供するドメインの 2 番目および最上位のレベルです。 会社のドメインなので、cookie はファーストパーティ cookie になります。 例: `mycompany.com`. |
| サーバードメイン | `clientcode.tt.omtrdc.net`。アカウントのクライアントコードを使用します。 |
| cookie の期間 | Cookieは、訪問者の最後のログインから2年間、訪問者のブラウザーに残ります。<P>`deviceIdLifetime`設定は、[at.js バージョン 2.3.1以降](../atjs/target-atjs-versions.md)で上書きできます。 詳しくは、[targetGlobalSettings()](../../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。 |
| P3P ポリシー | ほとんどのブラウザーのデフォルト設定の要求に従って、cookie は P3P ポリシーに基づいて発行されます。 P3P ポリシーはブラウザーに対して、cookie を扱うユーザーおよびその情報の使用方法を指示します。 |

Cookie は、キャンペーンでの訪問者のエクスペリエンスを管理するための様々な値を保持します。

| 値 | 定義 |
|--- |--- |
| session ID | ユーザーセッションの一意の ID。 デフォルトでは、30 分間存続します。 |
| pc ID | 訪問者のブラウザーの半永久的な ID。 14 日間存続します。 |
| check | 訪問者が cookie をサポートするかどうかを判別するために使用される簡単なテスト値。 訪問者がページをリクエストするたびに設定されます。 |
| disable | 訪問者の読み込み時間が、Adobe Experience Platform Web SDKまたはat.js ファイルで設定されたタイムアウトを超えているかどうかを設定します。 デフォルトでは、1 時間存続します。 |

## Apple WebKit トラッキングの変更によるSafari訪問者に対する[!DNL Target]への影響

次の点に注意してください。

### [!DNL Adobe Target] トラッキングの仕組み

| Cookie | 詳細 |
|--- |--- |
| ファーストパーティドメイン | これは、[!DNL Target]のお客様の標準的な実装です。  「mbox」Cookie はお客様のドメインで設定されます。 |
| サードパーティ追跡 | [!DNL Target]および[!DNL Adobe Audience Manager] （AAM）の広告とターゲティングのユースケースでは、サードパーティによるトラッキングが重要です。  サードパーティによる追跡には、クロスサイトスクリプティング手法が必要です。 [!DNL Target]は、`clientcode.tt.omtrd.net` ドメインで設定された「mboxSession」と「mboxPC」の2つのCookieを使用しています。 |

### Apple のアプローチについて

Apple の発表内容：

「Intelligent Tracking Prevention は、Cookie およびその他の Web サイトデータを制限してクロスサイト追跡を減らすための WebKit の新機能です。」

「これはクロスサイト追跡と呼ばれ、`example-tracker.com` によって使用される Cookie はサードパーティ Cookie と呼ばれます。 当社のテストでは、人気のある Web サイトで 70 を超えるこのようなトラッカーが見つかりました。これらのトラッカーはすべて、認識されることなくユーザーのデータを収集しています。」

| アプローチ | 詳細 |
|--- |--- |
| Intelligent tracking prevention | 詳しくは、WebKit オープンソース Web ブラウザーエンジン Web サイトの [Intelligent Tracking Prevention](https://webkit.org/blog/7675/intelligent-tracking-prevention/) を参照してください。 |
| Cookie | Safari における Cookie の処理方法：<ul><li>ユーザーが直接アクセスするドメインにないサードパーティ Cookie は保存されません。 この動作は新しいものではありません。 既に Safari ではサードパーティ Cookie はサポートされていません。</li><li>ユーザーが直接アクセスするドメインで設定されたサードパーティ Cookie は、24 時間後に消去されます。</li><li>ファーストパーティドメインがサイトを横断してユーザーを追跡していると分類されている場合、そのファーストパーティ Cookie は 30 日後に削除されます。 この問題は、異なるドメインにオンラインでユーザーを送信する大企業で起こる可能性があります。 Apple では、これらのドメインがどのように分類されるか、またドメインがサイトを横断してユーザーを追跡していると分類されているかどうかを調べる方法について明確にしていません。</li></ul> |
| 機械学習によるクロスサイトドメインの識別 | Apple の発表内容：<P>機械学習分類子：非公開で管理されるトップドメインがサイトを横断してユーザーを追跡できるかどうかについて、収集された統計情報に基づいて分類するために、機械学習モデルが使用されます。 収集された様々な統計情報から、多数の一意のドメインの下位にあるサブリソース、多数の一意のドメインの下位にあるサブフレーム、多数の一意のドメインへのリダイレクト、という 3 つのベクトルが現在の追跡方法に基づく分類に対する強力なシグナルとなることがわかりました。 すべてのデータ収集と分類はデバイス上でおこなわれます。<P>ただし、ユーザーがexample.comをトップドメインとして操作する場合（1st パーティドメインと呼ばれることが多い）、インテリジェントトラッキング防止は、ユーザーがweb サイトに興味を持っていることを示すシグナルと見なし、このタイムラインに示されているように一時的に動作を調整します。<P>ユーザーが過去24時間にexample.comとやり取りした場合、`example.com`がサードパーティの場合、そのCookieは利用できます。 これにより、「Y で X アカウントを使用してサインインする」ログインシナリオが可能になります。<ul><li>トップレベルドメインとして訪問されるドメインは影響を受けません。 例えば、OKTA のようなサイト</li><li>複数の一意のドメインにまたがる、現在のページのサブドメインまたはサブフレームであるドメインを識別します。</li></ul> |

### アドビが受ける影響

| 影響を受ける機能 | 詳細 |
|--- |--- |
| オプトアウトのサポート | Apple の WebKit 追跡における変更により、オプトアウトのサポートが影響を受けます。<P>[!DNL Target] オプトアウトは、`clientcode.tt.omtrdc.net` ドメインでCookieを使用します。 詳しくは、「[プライバシー](/help/dev/before-implement/privacy/privacy.md)」を参照してください。<P>[!DNL Target]は2つのオプトアウトをサポートしています：<ul><li>クライアントごと（クライアントがオプトアウトリンクを管理します）。</li><li>Adobeを使用して、すべてのユーザーに対して[!DNL Target]機能からユーザーをオプトアウトします。</li></ul>どちらの方法でもサードパーティ Cookie が使用されます。 |
| [!DNL Target]件のアクティビティ | お客様は、[!DNL Target] アカウントの[&#x200B; プロファイルの有効期間](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/visitor-profile-lifetime.html?lang=ja)を最大90日間まで選択できます。 問題は、アカウントのプロファイルの有効期間が30日を超え、顧客のドメインがクロスサイトでユーザーを追跡するようにマークされているため、ファーストパーティ Cookieが消去された場合、Safari訪問者の行動は[!DNL Target]の次の領域で影響を受けることです。<P>**[!DNL Target]件のレポート**: Safari ユーザーがアクティビティに参加し、30日後に戻ってコンバージョンした場合、そのユーザーは2人の訪問者と1人のコンバージョンとしてカウントされます。<P>この動作は、[!DNL Analytics]をレポートソース （A4T）として使用するアクティビティに対しても同じです。<P>**プロファイルとアクティビティ メンバーシップ**:<ul><li>プロファイルデータは、ファーストパーティ Cookie の有効期限が切れた時点で消去されます。</li><li>アクティビティメンバーシップは、ファーストパーティ Cookie の有効期限が切れた時点で消去されます。</li><li> サードパーティ Cookieの実装またはファーストパーティ Cookieとサードパーティ Cookieの実装を使用しているアカウントでは、[!DNL Target]はSafariで機能しません。 この動作は新しいものではありません。 Safari では、サードパーティ Cookie を以前から許可していません。</li></ul><P>**提案**：顧客ドメインがクロスセッションで1人のトラッキング訪問者としてマークされる可能性がある場合、[!DNL Target]でプロファイルの有効期間を30日以内に設定するのが最も安全です。 これにより、ユーザーは Safari と他のすべてのブラウザーで同様に追跡されます。 |

