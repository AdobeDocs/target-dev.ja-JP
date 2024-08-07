---
keywords: メール，adbox, メール画像 adbox
description: ' [!DNL Adobe Target]  を使用して、メール内の画像を動的にテストする方法と、メールを開いたときに画像をその場で変更する方法について説明します。'
title: メール画像 Adbox のテスト方法を教えてください。
feature: Implement Email
exl-id: 4512741a-567f-41bb-9721-3e1c4f5302e1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 79%

---

# 電子メール画像 adbox のテスト

電子メール内の画像を動的にテストし、電子メールが開かれたときにそれらの画像をその場で変更することもできます。

電子メールでリダイレクターを使用してクリックをトラッキングし、訪問者が到達するランディングページを動的に制御できます。

電子メールの画像テストは、修正バージョンの adbox を使用して実施されます。電子メールクライアントでは cookie の 設定ができないので、電子メールごとに一意の ID を生成する必要があります。この数字は、電子メールからのクリックをトラッキングするために、電子メールで使用する adbox の URL およびすべてのリダイレクターに追加されます。

>[!NOTE]
>
>技術的にはオープン時に画像の最適化を提供で [!DNL Target] ますが、キャッシュの処理は電子メールクライアントごとに異なるので、成功もさまざまです。 電子メールサービスプロバイダーにかかわらず、画像が開かれたときに実際に取得されるかどうかは、エンドユーザーが電子メールを開くために使用する電子メールクライアント（Gmail アプリ、Outlook、Hotmail など）によって決まります。例えば、Gmail ではほとんどすべてのものをキャッシュするので、エンドユーザーに表示される画像は、Gmail が画像を呼び出してキャッシュするタイミングによって異なります。

**電子メール画像 adbox のサンプルコード：**

```
<img src="https://{clientcode}.tt.omtrdc.net/m2/​{clientcode}/ubox/​image?
mbox={email_header}&
mboxDefault=​{http%3A%2F%2Fwww.domain.com%2Fheader.jpg}&
mboxXDomain=disabled&
mboxSession={123456}&
mboxPC={123456}" border=:"0"/>
```

ここで、以下の値はユーザー特有の値です。

| 値 | 説明 |
|--- |--- |
| clientcode | お客様のクライアントコード。`clientCode='yourclientcode'` のようにリストされた at.js でこれを見つけます。 このコードはすべて小文字で表され、特殊文字は含みません。 |
| image | オファータイプ。常に、グラフィック広告の場合は「image」、リダイレクターの場合は「page」になります。 |
| email_header | adbox の名前。 |
| `mboxDefault=http%3A%2F%2Fwww.domain.com%2Fheader.jpg` | 必須。URL を adbox の適切なデフォルトコンテンツに置き換えます。これには絶対参照を指定し、URL エンコードする必要があります。 |
| `mboxXDomain=disabled` | [!DNL Target] に Cookie を設定しないように指示します。 |
| `mboxSession=123456` および `mboxPC=123456` | このユーザーのプロファイルをサイトの既存のプロファイルと結合するには、[!DNL Target] で 2 つの値が必要です。 123456 は、電子メールごとに生成される一意の識別子です。この値は、すべての adbox およびリダイレクターの URL に動的に挿入します。この数字は、各受信者に送信される電子メールごとに一意である必要があります。毎週 1,000 人の受信者にメールを送信する場合は、一意の ID を 1,000 個生成する必要があります。<br />それぞれの電子メールの一意の識別子を、各 adbox およびリダイレクターの URL の mboxSession と mboxPC に割り当てる必要があります。この識別子に推奨される形式は、timestamp-NNNNN です。NNNNN はランダムな 5 桁の数字ですが、任意の英数字の形式も使用できます。大規模な電子メールサービスおよびプログラミング言語の中には、この一意の識別子を生成できるものがあります。 |
