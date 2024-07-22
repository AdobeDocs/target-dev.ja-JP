---
keywords: グローバル mbox, target classic, target classic からグローバル mbox を使用する
description: レガシー実装のページにグローバル mbox を既に作成している場合は  [!DNL Adobe Target]  アクティビティにレガシーのグローバル mbox を使用する方法を説明します。
title: レガシー実装のグローバル mbox を使用できますか？
feature: at.js
exl-id: fe608b5e-ff66-4ba2-a622-d4f7307a9ca9
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 20%

---

# レガシー実装のグローバル mbox の使用

デフォルトでは、[!DNL Target] は target-global-mbox と呼ばれるグローバル mbox を作成し、[!DNL Target] で作成されたアクティビティの実行に使用されます。 ただし、レガシー実装のページにグローバル mbox を既に作成している場合は、その mbox を [!DNL Target] アクティビティに使用できます。

>[!NOTE]
>
>グローバル mbox は、各アカウントにつき 1 つのみ作成できます。

[!DNL Target] とレガシー実装の両方で既存のグローバル mbox を使用するには、いくつかのパラメーターを設定する必要があります。

1. [!DNL Target] に移動し、**[!UICONTROL Administration]**/**[!UICONTROL Implementation]** をクリックします。

   デフォルトでは **[!UICONTROL Page load enabled (Auto-create global mbox]** が有効になっており、カスタムのグローバル mbox の名前は `target-global-mbox` です。

1. 既存の mbox を使用する場合は、**[!UICONTROL Page load enabled (Auto-create global mbox]** を無効にし、以前に作成したグローバル mbox の名前を「**[!UICONTROL Global Mbox]**」フィールドに指定します。

   グローバル mbox ドロップダウンには、アカウント内のすべての mbox が一覧表示されます。 まだ存在していない mbox を使用する場合は、mbox を作成します。

1. **[!UICONTROL Save]** をクリックします。

   アカウントの設定が更新されます。

1. 新しい at.js ファイルをダウンロードし、サイトで参照します。

   作成済みおよび実装済みのアクティビティを含むすべての既存のアクティビティが、指定したグローバル mbox を使用するように更新されます。

## グローバル mbox 実装のトラブルシューティング

以下の FAQ を使用して、グローバル mbox 実装のトラブルシューティングを行うことができます。

### グローバル mbox が読み込まれない理由や、ページの読み込み時にグローバル mbox の読み込みに遅延が発生する理由は何ですか？

at.js の参照が、そのページでの最初のJavaScript呼び出しであることを確認します。 この問題に対する他の解決策については、[ グローバル mbox に関するよくある質問 ](/help/dev/implement/client-side/atjs/global-mbox/global-mbox-faq.md) を参照してください。
