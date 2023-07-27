---
keywords: グローバル mbox, at.js の実装
description: のグローバル mbox について説明します。 [!DNL Adobe Target], a name used to refer to the single server call made at the top of each web page in your [!DNL Target] 実装。
title: グローバル mbox とは
feature: at.js
exl-id: 572c1dc6-5cdd-427a-9458-e5ec49990cf8
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '200'
ht-degree: 73%

---

# グローバル mbox について

グローバル mbox について説明します。グローバル mbox とは、[!DNL Adobe Target] 実装の各 Web ページの最上部でおこなわれる単一のサーバーコールを参照するために使用される名前です。

デフォルトでは、グローバル mbox は、`target-global-mbox` という名前が付いています。必要に応じて、アカウント用に名前を変更できます。

標準の mbox（非グローバル mbox）とグローバル mbox には、次のようないくつかの違いがあります。

| 標準の mbox | グローバル mbox |
|--- |--- |
| 標準の mbox は、通常、`<DIV>` タグでコンテンツを囲みます。 | グローバル mbox は、「空」で、コンテンツを囲みません。 |
| 1 つのアクティビティのみからのコンテンツは、1 つの標準の mbox に配信できます。 | 複数のアクティビティからのコンテンツは、1 つのグローバル mbox への 1 回の応答で配信できます。 |

複数のアクティビティがグローバル mbox または複数の標準 mbox を使用して配信される場合、Target [優先度を決定](https://experienceleague.adobe.com/docs/target/using/activities/priority.html) アクティビティ（またはアクティビティ）が Web ページに配信される条件です。

追加のページレベルのデータは、[!DNL Target] 関数を使用することで、グローバル mbox と共に `[!UICONTROL targetPageParams]` に送信できます。これは、mbox パラメーターの機能と同様です。詳しくは、「[グローバル mbox にパラメーターを渡す](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md)」を参照してください。
