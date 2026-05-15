---
keywords: content security policy, csp, at.js, whitelist, 許可リストに加える, flicker, pre-hide, pre-hide, prehiding, content security policy, iFrame, iframe
description: ' [!DNL Adobe Target]を使用する際に追加する必要があるContent Security Policy （CSP） ディレクティブについて説明します。'
title: ' [!DNL Target]  では、コンテンツセキュリティポリシー（CSP）にどのように対応しますか？'
feature: Privacy & Security
exl-id: ec6942e5-36d8-4f88-b3d6-47f9eaca03a8
TQID: https://experienceleague.adobe.com/gGNgYyblw6-D-RiHBtzAtrOOdhVOsIzYQ-HhkhCtyuI
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 610
ht-degree: 28%

---

# コンテンツセキュリティポリシー（CSP）指令

[!DNL Adobe Target]実装に[Content Security Policy](https://en.wikipedia.org/wiki/Content_Security_Policy) （CSP）を使用している場合、[at.js 2.1以降](../../implement/client-side/atjs/target-atjs-versions.md)を使用する際には、次のCSP ディレクティブを追加する必要があります。

* `connect-src` と `*.tt.omtrdc.net` 許可リスト。 [!DNL Target] エッジへのネットワークリクエストを許可するために必要です。
* `style-src unsafe-inline`. 事前非表示およびちらつき制御に必要です。
* `script-src unsafe-inline`. HTML オファーの一部となる JavaScript の実行を許可するために必要です。

## よくある質問（FAQ）

セキュリティポリシーに関する以下の FAQ を参照してください。

### クロスオリジンリソース共有（CORS）と Flash クロスドメインポリシーには、セキュリティの問題がありますか？

CORS ポリシーを実装する推奨方法は、信頼済みのドメインの許可リストを介して、アクセスを必要とする信頼されたオリジンだけにアクセスを許可することです。 Flash クロスドメインポリシーについても同じことがいえます。 一部の[!DNL Target]のお客様は、Targetのドメインにワイルドカード文字を使用することを懸念しています。 懸念されるのは、ユーザーがアプリケーションにログインし、ポリシーで許可されているドメインにアクセスした場合、そのドメインで実行されている悪意のあるコンテンツは、アプリケーションから機密コンテンツを取得し、ログインしたユーザーのセキュリティコンテキスト内でアクションを実行する可能性があることです。 この状況は、一般的にクロスサイトリクエストフォージェリー（CSRF）と呼ばれます。

ただし、[!DNL Target]の実装では、これらのポリシーはセキュリティの問題を表すものではありません。

「adobe.tt.omtrdc.net」は、アドビが所有するドメインです。 [!DNL Adobe Target] は、テストとパーソナライゼーションのツールです。[!DNL Target] は、認証を必要とせずに、どこからでもリクエストを受け取り、処理できることが期待されます。 これらのリクエストには、A/B テスト、レコメンデーション、コンテンツのパーソナライゼーションに使用されるキーと値のペアが含まれています。

Adobeは、「adobe.tt.omtrdc.net」が指す[!DNL Adobe Target] エッジサーバーに個人情報（PII）またはその他の機密情報を保存しません。

これにより、JavaScript 呼び出しを使用してどのドメインからでも [!DNL Target] にアクセスできることが期待されます。 このアクセスを許可する唯一の方法は、「Access-Control-Allow-Origin」をワイルドカードに適用することです。

### サイトをiFrameとして外部ドメインに埋め込むのを許可または防止するにはどうすればよいですか？

[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html){target=_blank} （VEC）がWeb サイトをiFrameに埋め込むようにするには、Web サーバー設定でCSP （設定されている場合）を変更する必要があります。 [!DNL Adobe] ドメインはホワイトリストに登録して設定する必要があります。

セキュリティ上の理由から、サイトが外部ドメインにiFrameとして埋め込まれるのを防ぐ必要がある場合があります。

次の節では、VECがiFrameにサイトを埋め込むのを許可または禁止する方法について説明します。

#### VECがiFrameにサイトを埋め込むことを許可する

VECがWeb サイトをiFrameに埋め込めるようにするための最も簡単な方法は、最も広いワイルドカードである`*.adobe.com`を許可することです。

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self' *.adobe.com`

次の図のように（クリックして拡大）:


最も広いワイルドカードを含む![CSP](/help/dev/before-implement/privacy/assets/csp-adobe.png){width="600" zoomable="yes"}

実際の[!DNL Adobe] サービスのみを許可することができます。 このシナリオは、`*.experiencecloud.adobe.com + https://experiencecloud.adobe.com`を使用することで実現できます。

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self' https://*.experiencecloud.adobe.com https://experiencecloud.adobe.com https://experience.adobe.com`

次の図のように（クリックして拡大）:

![ExperienceCloud スコープ付きCSP](/help/dev/before-implement/privacy/assets/csp-experiencecloud.png){width="600" zoomable="yes"}

会社のアカウントへの最も制限的なアクセスは、`https://<Client Code>.experiencecloud.adobe.com https://experience.adobe.com`を使用することで実現できます。ここで、`<Client Code>`は特定のクライアントコードを表します。

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self'  https://ags118.experiencecloud.adobe.com https://experience.adobe.com`

次の図のように（クリックして拡大）:

クライアントコードのスコープが指定された![CSP](/help/dev/before-implement/privacy/assets/csp-clientcode.png){width="600" zoomable="yes"}

>[!NOTE]
>
>[Launch/Tag](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)が実装されている場合は、ロックも解除する必要があります。
>
>次に例を示します。
>
> `Content-Security-Policy: frame-ancestors 'self' *.adobe.com *.assets.adobedtm.com;`

#### VECがiFrameにサイトを埋め込むのを防ぐ

VECがiFrameにサイトを埋め込むのを防ぐには、「自分」のみに制限できます。

次に例を示します。

`Content-Security-Policy: frame-ancestors 'self'`

次の図に示すように（クリックして拡大）:

![CSP エラー](/help/dev/before-implement/privacy/assets/csp-error.png){width="600" zoomable="yes"}

次のエラーメッセージが表示されます。

`Refused to frame 'https://kuehl.local/' because an ancestor violates the following Content Security Policy directive: "frame-ancestors 'self'".`

