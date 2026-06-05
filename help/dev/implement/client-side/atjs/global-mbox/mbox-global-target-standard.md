---
keywords: グローバル mbox, target classic, target classic からグローバル mbox を使用する
description: レガシー実装用のページにグローバル mboxを既に作成している場合は、レガシーのグローバル mboxを [!DNL Adobe Target]  アクティビティに使用する方法を説明します。
title: 従来の実装からグローバル mboxを使用できますか？
feature: at.js
exl-id: fe608b5e-ff66-4ba2-a622-d4f7307a9ca9
TQID: https://experienceleague.adobe.com/BCubNDwB8gxZ9bpuCNhxcnFnjB1xQK8ZRkLveinPj4w
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 304
ht-degree: 19%

---

# 従来の実装からグローバル mboxを使用する

デフォルトでは、[!DNL Target]は[!DNL Target]で作成されたアクティビティの実行に使用されるtarget-global-mboxと呼ばれるグローバル mboxを作成します。 ただし、レガシー実装用にページ上にグローバル mboxを既に作成している場合は、そのmboxを[!DNL Target] アクティビティに使用できます。

>[!NOTE]
>
>グローバル mbox は、各アカウントにつき 1 つのみ作成できます。

[!DNL Target] とレガシー実装の両方で既存のグローバル mbox を使用するには、いくつかのパラメーターを設定する必要があります。

1. [!DNL Target]に移動し、**[!UICONTROL 管理]** > **[!UICONTROL 実装]**&#x200B;をクリックします。

   デフォルトでは、**[!UICONTROL ページ読み込み有効（自動作成グローバル mbox]**&#x200B;が有効で、カスタム グローバル mboxの名前は`target-global-mbox`です。

1. 既存のmboxを使用する場合は、**[!UICONTROL ページ読み込み有効（グローバル mbox]**&#x200B;を自動作成）を無効にし、**[!UICONTROL グローバル Mbox]** フィールドに以前に作成したグローバル mboxの名前を指定します。

   グローバル Mbox ドロップダウンには、アカウント内のすべてのmboxが一覧表示されます。 まだ存在しないmboxを使用する場合は、mboxを作成します。

1. 「**[!UICONTROL 保存]**」をクリックします。

   アカウントの設定が更新されます。

1. 新しいat.js ファイルをダウンロードし、サイトで参照します。

   作成済みおよび実装済みのアクティビティを含むすべての既存のアクティビティが、指定したグローバル mbox を使用するように更新されます。

## グローバル mbox実装のトラブルシューティング

次のFAQを使用して、グローバル mbox実装のトラブルシューティングを行うことができます。

### グローバル mboxが読み込まれない理由、ページの読み込み時にグローバル mboxの読み込みに遅延が発生する理由を教えてください。

at.js参照がページの最初のJavaScript呼び出しであることを確認します。 この問題に対する他の解決策については、[ グローバル mboxのよくある質問](/help/dev/implement/client-side/atjs/global-mbox/global-mbox-faq.md)を参照してください。
