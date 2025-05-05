---
keywords: グローバル mbox、at.js の実装
description: 実装時のグローバル mbox について説明  [!DNL Adobe Target], a name used to refer to the single server call made at the top of each web page in your [!DNL Target]  ます。
title: グローバル mbox とは
feature: at.js
exl-id: 572c1dc6-5cdd-427a-9458-e5ec49990cf8
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '194'
ht-degree: 62%

---

# グローバル mbox について

グローバル mbox に関する情報です。グローバル mbox は、[!DNL Adobe Target] 実装の各 web ページの上部で行われる単一サーバー呼び出しを参照するために使用される名前です。

デフォルトでは、グローバル mbox は、`target-global-mbox` という名前が付いています。必要に応じて、アカウント用に名前を変更できます。

標準の mbox（非グローバル mbox）とグローバル mbox には、次のようないくつかの違いがあります。

| 標準の mbox | グローバル mbox |
|--- |--- |
| 標準の mbox は、通常、`<DIV>` タグでコンテンツを囲みます。 | グローバル mbox は、「空」で、コンテンツを囲みません。 |
| 1 つのアクティビティのみからのコンテンツは、1 つの標準の mbox に配信できます。 | 複数のアクティビティからのコンテンツは、1 つのグローバル mbox への 1 回の応答で配信できます。 |

複数のアクティビティがグローバル mbox または複数の通常の mbox で配信される場合、Target は [ 優先度を決定 ](https://experienceleague.adobe.com/docs/target/using/activities/priority.html?lang=ja) し、それによってアクティビティ（またはアクティビティ）が web ページに配信されます。

追加のページレベルのデータは、[!DNL Target] 関数を使用することで、グローバル mbox と共に `[!UICONTROL targetPageParams]` に送信できます。これは、mbox パラメーターの機能と同様です。詳しくは、「[グローバル mbox にパラメーターを渡す](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md)」を参照してください。
