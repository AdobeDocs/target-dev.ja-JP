---
keywords: グローバル mbox, グローバル mbox のカスタマイズ，at.js の編集，at.js, at.js の実装
description: ' [!DNL Adobe Target] の [!UICONTROL Administration]-[!UICONTROL Implementation] ページで、at.js のグローバル mbox をカスタマイズする方法を説明します。'
title: グローバル mbox をカスタマイズするにはどうすればよいですか？
feature: at.js
exl-id: f7809c3d-6e77-4bbe-8da3-4ab0a448c801
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '205'
ht-degree: 18%

---

# グローバル mbox のカスタマイズ

at.js 用の [!DNL Adobe Target] グローバル mbox のカスタマイズに役立つ情報です。

1. **[!UICONTROL Administration]**／**[!UICONTROL Implementation]**&#x200B;をクリックします。

1. 「**[!UICONTROL Page load enabled (Auto create global mbox)]**」を無効にし、[!DNL Target] からアクティビティを配信するために使用するカスタムグローバル mbox の名前を追加します。

>[!WARNING]
>
>別のグローバル mbox を選択すると、変更は自動的に保存されます。

このカスタムグローバル mbox は、クリック追跡にも使用されます。

![custom-global-mbox](../../assets/custom-global-mbox.png)

1. サイトに at.js ライブラリを実装します。

   詳しくは、[at.js のデプロイ方法 ](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md) を参照してください。

1. リリースへの移行のタイミングをはかります。

   今後、すべてのアクティビティでグローバル mbox の使用を開始する準備が整 [!DNL Target] たら、この手順に進むことができます。

   カスタムグローバル mbox の名前が前述の手順 2 で使用した名前に一致するよう更新します。


>[!WARNING]
>
>アカウント内のすべてのアクティビティがこの mbox に同期されます。 アクティビティが引き続き機能するように、グローバル mbox がサイトに存在することを確認します。 この mbox と同期する [!UICONTROL Visual Experience Composer] （VEC）で作成された、影響を受けるアクティビティを編集して再保存してください。 [!UICONTROL Form-Based Experience Composer] または API を使用して作成されたアクティビティを再保存する必要はありません。
