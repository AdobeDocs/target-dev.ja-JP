---
keywords: 実装、at.js non javascript、adbox、redirector、mbox
description: AdBoxやリダイレクターの使用など、JavaScript以外のシナリオで [!DNL Adobe Target] を実装する方法について説明します。
title: 電子メール用に [!DNL Target] を実装するにはどうすればよいですか？
feature: Implement Email
exl-id: dda00b75-5d58-4405-ae58-75e7883a30ed
TQID: https://experienceleague.adobe.com/NeITIa97pW-yiB6EB-ajqRuBgp9mx-uzdwlREdgotj8
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: c94a34eb-b51c-4dd1-a6a4-46b0d84ccccd
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e9001ce2-5245-4a8e-8601-dd958009072f
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 441
ht-degree: 62%

---

# 電子メール：[!DNL Target]を実装

AdBoxやリダイレクターの使用など、JavaScript以外のシナリオでの[!DNL Target]の実装に関する情報。

広告およびその他のオフサイトコンテンツへの訪問を追跡できます。 また、同じユーザーがサイト上にいるかサイト外にいるかを識別し、Web エクスペリエンス全体を通じて一貫性のあるエクスペリエンスを提供できます。 AdBoxでは、ひとつのURLを利用することで、JavaScriptやat.jsを使用せずにテストすることができます。

AdBoxは、at.jsを持たないサイト（アフィリエイトなど）に役立ちます。 アクティビティに動的なクリエイティブが必要な場合（例えば、買い物かごから放棄された製品を広告に表示する必要がある場合など）、adbox を使用することはできません。

adbox 広告およびリダイレクターは、あらゆるアクティビティで使用できます。 次の表には、adbox とリダイレクターの比較と、それぞれの適切な用途がまとめられています。

| | 目的 | 使用するタイミング | URL 構成 | オファータイプ | オファーコンテンツ |
|--- |--- |--- |--- |--- |--- |
| adbox | 別の画像を広告に返す | 広告のコンテンツを変更する場合 | `clientcode&#x200B;.tt.&#x200B;omtrdc&#x200B;.net/&#x200B;m2&#x200B;/&#x200B;clientcode/ubox/&#x200B;image?` | リダイレクトオファー | 画像の URL |
| リダイレクター | 訪問者を別の Web ページにリダイレクトする | 広告のランディングページを変更する場合 | `clientcode&#x200B;.tt.omtrdc.net/&#x200B;m2/clientcode&#x200B;/ubox/page?` | リダイレクトオファー | ページの URL |

## セキュリティのベストプラクティス

Redirectorでは、オープンリダイレクトの脆弱性のリスクにさらされる可能性があります。 第三者によるリダイレクトリンクの不正使用を避けるために、デフォルトのリダイレクト URL ドメインを許可リストに加えるするには、「承認済みホスト」を使用することをお勧めします。 [!DNL Target]はホストを使用して、リダイレクトを許可するドメインを許可リストに加えるします。 詳しくは、*Hosts*&#x200B;の [!DNL Target][&#128279;](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html#allowlist)へのmbox呼び出しを許可するホストを指定する ホストの作成を参照してください。

## 制限事項

* 標準の mbox と同様に、クライアント側のタイムアウトはありません。 [!DNL Target]が完全にダウンしている場合、広告への訪問者には、デフォルトではなくコンテンツも表示されません。
* サードパーティ Cookie が訪問の追跡に使用されます。 PCID が異なる場合、デフォルトでは、訪問者のサードパーティプロファイルと既存のファーストパーティプロファイルが結合されます。
* adbox 自体でファーストパーティ Cookie を使用するには、URL で mbox セッションを渡す必要があります。 この方法について詳しくは、アカウント担当者にお問い合わせください。
* ファーストパーティ Cookie を使用して広告クリックを追跡するには、URL で mbox セッションを渡します。 この方法について詳しくは、アカウント担当者にお問い合わせください。
* 同じページで複数の adbox を使用するには、URL で mbox セッションを渡す必要があります。 この方法について詳しくは、アカウント担当者にお問い合わせください。 同じページで使用できるのは、1 つの adbox と 1 つのリダイレクターリンクです（リダイレクターは実際には 2 番目のページにあります）。
