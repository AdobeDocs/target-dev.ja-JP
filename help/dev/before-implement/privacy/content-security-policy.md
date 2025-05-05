---
keywords: コンテンツセキュリティポリシー，csp, at.js, ホワイトリスト，^許可リスト，ちらつき，事前非表示，事前非表示，事前非表示，コンテンツセキュリティポリシー，iFrame, iframe
description: ' [!DNL Adobe Target] を使用する際に追加する必要があるコンテンツセキュリティポリシー（CSP）指令について説明します。'
title: ' [!DNL Target]  では、コンテンツセキュリティポリシー（CSP）にどのように対応しますか？'
feature: Privacy & Security
exl-id: ec6942e5-36d8-4f88-b3d6-47f9eaca03a8
source-git-commit: c43c79b29768694eac534e22047b5ee6a3d0ccd5
workflow-type: tm+mt
source-wordcount: '589'
ht-degree: 28%

---

# コンテンツセキュリティポリシー（CSP）指令

[ コンテンツセキュリティポリシー ](https://en.wikipedia.org/wiki/Content_Security_Policy) （CSP）を [!DNL Adobe Target] の実装に使用する場合、[at.js 2.1 以降 ](../../implement/client-side/atjs/target-atjs-versions.md) を使用する際は以下の CSP 指令を追加する必要があります。

* `connect-src` と `*.tt.omtrdc.net` 許可リスト。[!DNL Target] エッジへのネットワークリクエストを許可するために必要です。
* `style-src unsafe-inline`をインストールします。事前非表示およびちらつき制御に必要です。
* `script-src unsafe-inline`HTML オファーの一部となる JavaScript の実行を許可するために必要です。

## よくある質問（FAQ）

セキュリティポリシーに関する以下の FAQ を参照してください。

### クロスオリジンリソース共有（CORS）と Flash クロスドメインポリシーには、セキュリティの問題がありますか？

CORS ポリシーを実装する推奨方法は、信頼済みのドメインの許可リストを介して、アクセスを必要とする信頼されたオリジンだけにアクセスを許可することです。Flash クロスドメインポリシーについても同じことがいえます。一部の [!DNL Target] のお客様は、Target でのドメインに対するワイルドカード文字の使用を心配しています。 ユーザーがアプリケーションにログインし、ポリシーで許可されたドメインを訪問した場合に、そのドメインで実行されている悪意のあるコンテンツがアプリケーションから機密コンテンツを取得し、ログインしているユーザーのセキュリティコンテキスト内でアクションを実行する可能性があるという懸念があります。 これは、一般に、クロスサイトリクエストフォージェリ（CSRF）と呼ばれます。

ただし、[!DNL Target] 実装では、これらのポリシーは、セキュリティの問題にはならないはずです。

「adobe.tt.omtrdc.net」は、アドビが所有するドメインです。[!DNL Adobe Target] は、テストとパーソナライゼーションのツールです。[!DNL Target] は、認証を必要とせずに、どこからでもリクエストを受け取り、処理できることが期待されます。これらのリクエストには、A/B テスト、レコメンデーション、コンテンツのパーソナライゼーションに使用されるキーと値のペアが含まれています。

Adobeは、個人を特定できる情報（PII）やその他の機密情報を、「adobe.tt.omtrdc.net」で指定される [!DNL Adobe Target] エッジサーバー上に格納しません。

これにより、JavaScript 呼び出しを使用してどのドメインからでも [!DNL Target] にアクセスできることが期待されます。このアクセスを許可する唯一の方法は、ワイルドカードと共に「Access-Control-Allow-Origin」を適用することです。

### 外部ドメインの下でサイトが iFrame として埋め込まれるのを許可または禁止するにはどうすればよいですか？

[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=ja){target=_blank} （VEC）が web サイトを iFrame に埋め込めるようにするには、web サーバー設定で CSP （設定されている場合）を変更する必要があります。 ドメイン [!DNL Adobe]、許可リストに登録して設定する必要があります。

セキュリティ上の理由から、サイトが外部ドメインの下に iFrame として埋め込まれないようにする場合があります。

次の節では、VEC による iFrame へのサイトの埋め込みを許可または禁止する方法について説明します。

#### VEC による iFrame へのサイトの埋め込みを許可

VEC を使用して web サイトを iFrame に埋め込む最も簡単な解決策は、最も広いワイルドカードである `*.adobe.com` を許可することです。

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self' *.adobe.com`

次の図のように（クリックすると拡大できます）。


![ 最も広範なワイルドカードを使用した CSP](/help/dev/before-implement/privacy/assets/csp-adobe.png){width="600" zoomable="yes"}

実際の [!DNL Adobe] サービスのみを許可することもできます。 このシナリオは、`*.experiencecloud.adobe.com + https://experiencecloud.adobe.com` を使用することで達成できます。

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self' https://*.experiencecloud.adobe.com https://experiencecloud.adobe.com https://experience.adobe.com`

次の図のように（クリックすると拡大できます）。

![ExperienceCloud スコープの CSP](/help/dev/before-implement/privacy/assets/csp-experiencecloud.png){width="600" zoomable="yes"}

会社のアカウントに対する最も厳しいアクセスは、`https://<Client Code>.experiencecloud.adobe.com https://experience.adobe.com` を使用して実現できます。`<Client Code>` は特定のクライアントコードを表します。

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self'  https://ags118.experiencecloud.adobe.com https://experience.adobe.com`

次の図のように（クリックすると拡大できます）。

![ クライアントコードスコープの CSP](/help/dev/before-implement/privacy/assets/csp-clientcode.png){width="600" zoomable="yes"}

>[!NOTE]
>
>[Launch/Tag](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) を実装している場合は、ロックも解除する必要があります。
>
>次に例を示します。
>
> `Content-Security-Policy: frame-ancestors 'self' *.adobe.com *.assets.adobedtm.com;`

#### VEC が iFrame にサイトを埋め込まないようにする

VEC が iFrame にサイトを埋め込まないようにするには、「self」のみに制限します。

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self'`

次の図に示します（クリックすると拡大できます）。

![CSP エラー ](/help/dev/before-implement/privacy/assets/csp-error.png){width="600" zoomable="yes"}

次のエラーメッセージが表示されます。

`Refused to frame 'https://kuehl.local/' because an ancestor violates the following Content Security Policy directive: "frame-ancestors 'self'".`

