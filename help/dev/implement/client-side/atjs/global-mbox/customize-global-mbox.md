---
keywords: グローバル mbox、グローバル mboxのカスタマイズ、at.jsの編集、at.jsの実装
description: at.jsのグローバル mboxをカスタマイズする方法については、 [!DNL Adobe Target]の[!UICONTROL 管理]-[!UICONTROL 実装] ページを参照してください。
title: グローバル mboxをカスタマイズするにはどうすればよいですか？
feature: at.js
exl-id: f7809c3d-6e77-4bbe-8da3-4ab0a448c801
TQID: https://experienceleague.adobe.com/MtbjwpKrZ-WmBnE5tBY74oJgQVB-zPLrCuFDrFshkGo
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 227
ht-degree: 16%

---

# グローバル mbox のカスタマイズ

at.jsの[!DNL Adobe Target] グローバル mboxのカスタマイズに役立つ情報。

1. **[!UICONTROL 管理]** / **[!UICONTROL 実装]**&#x200B;をクリックします。

1. **[!UICONTROL ページ読み込み有効（グローバル mboxを自動作成）]**&#x200B;を無効にし、アクティビティを[!DNL Target]から配信するために使用するカスタム グローバル mboxの名前を追加します。

>[!WARNING]
>
>変更は、別のグローバル mboxを選択すると自動的に保存されます。

このカスタムグローバル mbox は、クリック追跡にも使用されます。

![custom-global-mbox](../../assets/custom-global-mbox.png)

1. at.js ライブラリをサイトに実装します。

   詳しくは、[at.js](/help/dev/implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md)のデプロイ方法を参照してください。

1. リリースへの移行のタイミングをはかります。

   今後すべてのアクティビティでグローバル mboxを使用する準備ができたら、この手順に進みます。[!DNL Target]

   カスタムグローバル mbox の名前が前述の手順 2 で使用した名前に一致するよう更新します。


>[!WARNING]
>
>アカウント内のすべてのアクティビティがこのmboxと同期されます。 アクティビティが引き続き機能するように、グローバル mboxがサイトに存在することを確認します。 このmboxと同期する[!UICONTROL Visual Experience Composer] （VEC）で作成された影響を受けるアクティビティを必ず編集して再保存してください。 [!UICONTROL  フォームベースのExperience Composer]またはAPI経由で作成されたアクティビティを再保存する必要はありません。
