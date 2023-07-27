---
keywords: コンテンツセキュリティポリシー， csp, at.js，ホワイトリスト，許可リストに加える，ちらつき，事前非表示，事前非表示，事前非表示，コンテンツセキュリティポリシー， iFrame, iframe
description: 使用時に追加する必要があるコンテンツセキュリティポリシー (CSP) ディレクティブについて説明します。 [!DNL Adobe Target].
title: ' [!DNL Target]  では、コンテンツセキュリティポリシー（CSP）にどのように対応しますか？'
feature: Privacy & Security
exl-id: ec6942e5-36d8-4f88-b3d6-47f9eaca03a8
source-git-commit: c43c79b29768694eac534e22047b5ee6a3d0ccd5
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 29%

---

# コンテンツセキュリティポリシー（CSP）指令

を使用している場合、 [コンテンツセキュリティポリシー](https://en.wikipedia.org/wiki/Content_Security_Policy) (CSP) を [!DNL Adobe Target] 実装する場合、を使用する際に次の CSP ディレクティブを追加する必要があります [at.js 2.1 以降](../../implement/client-side/atjs/target-atjs-versions.md):

* `connect-src` と `*.tt.omtrdc.net` 許可リスト。[!DNL Target] エッジへのネットワークリクエストを許可するために必要です。
* `style-src unsafe-inline`をインストールします。事前非表示およびちらつき制御に必要です。
* `script-src unsafe-inline`をインストールします。HTML オファーの一部となる JavaScript の実行を許可するために必要です。

## よくある質問（FAQ）

セキュリティポリシーに関する以下の FAQ を参照してください。

### クロスオリジンリソース共有（CORS）と Flash クロスドメインポリシーには、セキュリティの問題がありますか？

CORS ポリシーを実装する推奨方法は、信頼済みのドメインの許可リストを介して、アクセスを必要とする信頼されたオリジンだけにアクセスを許可することです。Flash クロスドメインポリシーについても同じことがいえます。一部 [!DNL Target] のお客様は、Target でのドメインに対するワイルドカード文字の使用を気にかけています。 ユーザーがアプリケーションにログインし、ポリシーで許可されたドメインにアクセスすると、そのドメインで実行する悪意のあるコンテンツは、潜在的にアプリケーションから機密コンテンツを取得し、ログインユーザーのセキュリティコンテキスト内でアクションを実行します。 この状況は、一般に、クロスサイトリクエストフォージェリ (CSRF) と呼ばれます。

内 [!DNL Target] ただし、これらのポリシーはセキュリティ上の問題を表すものではありません。

「adobe.tt.omtrdc.net」は、アドビが所有するドメインです。[!DNL Adobe Target] は、テストとパーソナライゼーションのツールです。[!DNL Target] は、認証を必要とせずに、どこからでもリクエストを受け取り、処理できることが期待されます。これらのリクエストには、A/B テスト、レコメンデーション、コンテンツのパーソナライゼーションに使用されるキーと値のペアが含まれています。

Adobeは、個人を特定できる情報 (PII) や、 [!DNL Adobe Target] エッジサーバー。「adobe.tt.omtrdc.net」が指し示します。

これにより、JavaScript 呼び出しを使用してどのドメインからでも [!DNL Target] にアクセスできることが期待されます。このアクセスを許可する唯一の方法は、ワイルドカードを使用して「Access-Control-Allow-Origin」を適用することです。

### サイトが外部ドメインの iFrame として埋め込まれるのを許可または防ぐ方法を教えてください。

次を許可するには、 [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html){target=_blank} (VEC)Web サイトを iFrame に埋め込むには、Web サーバーの設定で CSP（設定されている場合）を変更する必要があります。 [!DNL Adobe] ドメインは、ホワイトリストに登録され、設定されている必要があります。

セキュリティ上の理由から、サイトが外部ドメインの iFrame として埋め込まれるのを防ぐ必要がある場合があります。

次の節では、VEC が iFrame にサイトを埋め込むことを許可または禁止する方法について説明します。

#### VEC がサイトを iFrame に埋め込むのを許可する

VEC で Web サイトを iFrame に埋め込む最も簡単な方法は、次のとおりです。 `*.adobe.com`：最も広いワイルドカードです。

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self' *.adobe.com`

次の図に示すように（クリックして拡大） :


![最も広いワイルドカードを持つ CSP](/help/dev/before-implement/privacy/assets/csp-adobe.png){width="600" zoomable="yes"}

実際の [!DNL Adobe] サービス。 このシナリオは、 `*.experiencecloud.adobe.com + https://experiencecloud.adobe.com`.

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self' https://*.experiencecloud.adobe.com https://experiencecloud.adobe.com https://experience.adobe.com`

次の図に示すように（クリックして拡大） :

![ExperienceCloud 範囲を持つ CSP](/help/dev/before-implement/privacy/assets/csp-experiencecloud.png){width="600" zoomable="yes"}

会社のアカウントへの最も制限が厳しいアクセスは、 `https://<Client Code>.experiencecloud.adobe.com https://experience.adobe.com`です。 `<Client Code>` は、特定のクライアントコードを表します。

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self'  https://ags118.experiencecloud.adobe.com https://experience.adobe.com`

次の図に示すように（クリックして拡大） :

![clientcode スコープを持つ CSP](/help/dev/before-implement/privacy/assets/csp-clientcode.png){width="600" zoomable="yes"}

>[!NOTE]
>
>次の条件を満たしている場合： [起動/タグ](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) 実装済みの場合は、ロックを解除する必要があります。
>
>次に例を示します。
>
> `Content-Security-Policy: frame-ancestors 'self' *.adobe.com *.assets.adobedtm.com;`

#### VEC が iFrame にサイトを埋め込まないようにする

VEC が iFrame にサイトを埋め込まないようにするには、「自己」のみに制限します。

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self'`

次の図に示すように（クリックして拡大）、

![CSP エラー](/help/dev/before-implement/privacy/assets/csp-error.png){width="600" zoomable="yes"}

次のエラーメッセージが表示されます。

`Refused to frame 'https://kuehl.local/' because an ancestor violates the following Content Security Policy directive: "frame-ancestors 'self'".`

