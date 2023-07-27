---
keywords: グローバル mbox, target classic, target classic からグローバル mbox を使用する
description: レガシーグローバル mbox を [!DNL Adobe Target] アクティビティを作成します（レガシー実装用にページ上で既にグローバル mbox を作成している場合）。
title: レガシー実装のグローバル mbox を使用できますか？
feature: at.js
exl-id: fe608b5e-ff66-4ba2-a622-d4f7307a9ca9
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 36%

---

# レガシー実装のグローバル mbox の使用

デフォルトでは、 [!DNL Target] target-global-mbox というグローバル mbox を作成します。この mbox は、 [!DNL Target]. ただし、レガシー実装のページで既にグローバル mbox を作成している場合は、その mbox を [!DNL Target] アクティビティで使用できます。

>[!NOTE]
>
>グローバル mbox は、各アカウントにつき 1 つのみ作成できます。

[!DNL Target] とレガシー実装の両方で既存のグローバル mbox を使用するには、いくつかのパラメーターを設定する必要があります。

1. に移動します。 [!DNL Target]を選択し、次に **[!UICONTROL 管理]** > **[!UICONTROL 実装]**.

   デフォルトでは、 **[!UICONTROL ページ読み込みが有効（グローバル mbox を自動作成）]** が有効になっていて、カスタムグローバル mbox の名前が `target-global-mbox`.

1. 既存の mbox を使用する場合は、 **[!UICONTROL ページ読み込みが有効（グローバル mbox を自動作成）]**&#x200B;をクリックし、以前に作成したグローバル mbox の名前を **[!UICONTROL グローバル mbox]** フィールドに入力します。

   グローバル mbox ドロップダウンには、アカウント内のすべての mbox が一覧表示されます。 存在しない mbox を使用する場合は、mbox を作成します。

1. 「**[!UICONTROL 保存]**」をクリックします。

   アカウントの 設定が更新されます。

1. 新しい at.js ファイルをダウンロードし、サイトで参照します。

   作成済みおよび実装済みのアクティビティを含むすべての既存のアクティビティが、指定したグローバル mbox を使用するように更新されます。

## グローバル mbox 実装のトラブルシューティング

次の FAQ を使用して、グローバル mbox 実装のトラブルシューティングをおこなうことができます。

### グローバル mbox が読み込まれない理由、ページ読み込み時にグローバル mbox を読み込む時間に遅延が発生する理由を調べます。

at.js 参照が、ページ上で最初の JavaScript 呼び出しであることを確認します。 この問題に対する他の解決策については、 [グローバル mbox に関するよくある質問](/help/dev/implement/client-side/atjs/global-mbox/global-mbox-faq.md).
