---
keywords: 実装， at.js 非 JavaScript, adbox，リダイレクター， mbox
description: の実装方法を学ぶ [!DNL Adobe Target] adbox やリダイレクターの使用など、JavaScript 以外のシナリオの場合。
title: 実装方法 [!DNL Target] メール？
feature: Implement Email
exl-id: dda00b75-5d58-4405-ae58-75e7883a30ed
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '436'
ht-degree: 64%

---

# 電子メール：実装 [!DNL Target]

実装に関する情報 [!DNL Target] adbox やリダイレクターの使用など、JavaScript 以外のシナリオの場合。

広告およびその他のオフサイトコンテンツへの訪問を追跡できます。また、同じユーザーがサイト上にいるかサイト外にいるかを識別し、Web エクスペリエンス全体を通じて一貫性のあるエクスペリエンスを提供できます。adbox では、1 つの URL を使用して、JavaScript や at.js を使用せずにテストをおこなうことができます。

adbox は、アフィリエイトなど、at.js がないサイトに役立ちます。 アクティビティに動的なクリエイティブが必要な場合（例えば、買い物かごから放棄された製品を広告に表示する必要がある場合など）、adbox を使用することはできません。

adbox 広告およびリダイレクターは、あらゆるアクティビティで使用できます。次の表には、adbox とリダイレクターの比較と、それぞれの適切な用途がまとめられています。

| | 目的 | 使用するタイミング | URL 構成 | オファータイプ | オファーコンテンツ |
|--- |--- |--- |--- |--- |--- |
| adbox | 別の画像を広告に返す | 広告のコンテンツを変更する場合 | `clientcode&#x200B;.tt.&#x200B;omtrdc&#x200B;.net/&#x200B;m2&#x200B;/&#x200B;clientcode/ubox/&#x200B;image?` | リダイレクトオファー | 画像の URL |
| リダイレクター | 訪問者を別の Web ページにリダイレクトする | 広告のランディングページを変更する場合 | `clientcode&#x200B;.tt.omtrdc.net/&#x200B;m2/clientcode&#x200B;/ubox/page?` | リダイレクトオファー | ページの URL |

## セキュリティのベストプラクティス

リダイレクターを使用すると、オープンリダイレクトの脆弱性のリスクにさらされる可能性があることに注意してください。 サードパーティによるリダイレクターリンクの不正使用を防ぐため、「認証済みホスト」を使用してデフォルトのリダイレクト URL ドメインをす許可リストに加えるることをお勧めします。 [!DNL Target] は、ホストを使用して、リダイレクトを許可するドメインを許可リストに登録します。詳しくは、 [mbox 呼び許可リストに加える出しを送信する権限のあるホストを指定するの作成 [!DNL Target]](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html#allowlist) in *ホスト*.

## 制限事項

* 標準の mbox と同様に、クライアント側のタイムアウトはありません。次の場合 [!DNL Target] が完全に停止した場合、広告の訪問者にはコンテンツは表示されません。デフォルトコンテンツも表示されません。
* サードパーティ Cookie が訪問の追跡に使用されます。PCID が異なる場合、デフォルトでは、訪問者のサードパーティプロファイルと既存のファーストパーティプロファイルが結合されます。
* adbox 自体でファーストパーティ Cookie を使用するには、URL で mbox セッションを渡す必要があります。この方法について詳しくは、アカウント担当者にお問い合わせください。
* ファーストパーティ Cookie を使用して広告クリックを追跡するには、URL で mbox セッションを渡します。この方法について詳しくは、アカウント担当者にお問い合わせください。
* 同じページで複数の adbox を使用するには、URL で mbox セッションを渡す必要があります。この方法について詳しくは、アカウント担当者にお問い合わせください。同じページで使用できるのは、1 つの adbox と 1 つのリダイレクターリンクです（リダイレクターは実際には 2 番目のページにあります）。
