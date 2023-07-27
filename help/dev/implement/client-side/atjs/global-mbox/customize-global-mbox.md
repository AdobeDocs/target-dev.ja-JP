---
keywords: グローバル mbox，グローバル mbox のカスタマイズ， at.js の編集， at.js, at.js の実装
description: で at.js のグローバル mbox をカスタマイズする方法について説明します。 [!UICONTROL 管理]-[!UICONTROL 実装] ページ内 [!DNL Adobe Target].
title: グローバル mbox のカスタマイズ方法
feature: at.js
exl-id: f7809c3d-6e77-4bbe-8da3-4ab0a448c801
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '221'
ht-degree: 17%

---

# グローバル mbox のカスタマイズ

次の項目をカスタマイズするのに役立つ情報： [!DNL Adobe Target] at.js のグローバル mbox。

1. **[!UICONTROL 管理]**／**[!UICONTROL 実装]**&#x200B;をクリックします。

1. 無効にする **[!UICONTROL ページ読み込みが有効（グローバル mbox を自動作成）]**&#x200B;次に、からアクティビティを配信する際に使用するカスタムグローバル mbox の名前を追加します。 [!DNL Target].

>[!WARNING]
>
>別のグローバル mbox を選択した場合、変更は自動的に保存されます。

このカスタムグローバル mbox は、クリック追跡にも使用されます。

![custom-global-mbox](../../assets/custom-global-mbox.png)

1. サイトに at.js ライブラリを実装します。

   詳しくは、 [at.js のデプロイ方法](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md) を参照してください。

1. リリースへの移行のタイミングをはかります。

   準備が整ったら、 [!DNL Target] 今後、すべてのアクティビティでグローバル mbox を使用し始めるには、次の手順に進みます。

   カスタムグローバル mbox の名前が前述の手順 2 で使用した名前に一致するよう更新します。


>[!WARNING]
>
>アカウント内のすべてのアクティビティがこの mbox と同期します。 アクティビティが引き続き機能するように、グローバル mbox がサイトに存在することを確認します。 で作成した、影響を受けるアクティビティを必ず編集し、再度保存してください。 [!UICONTROL Visual Experience Composer] (VEC) と同期されます。 で作成したアクティビティを再保存する必要はありません。 [!UICONTROL フォームベースの Experience Composer] または API を使用して取得できます。
