---
keywords: グローバル mbox、at.jsの実装
description: ' [!DNL Adobe Target], a name used to refer to the single server call made at the top of each web page in your [!DNL Target] 実装のグローバル mboxについて説明します。'
title: グローバル mboxとは？
feature: at.js
exl-id: 572c1dc6-5cdd-427a-9458-e5ec49990cf8
TQID: https://experienceleague.adobe.com/MXEGvHHY8tFMfS6bMHcZYaG9mZtOWEkuODKH24JifZI
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 201
ht-degree: 61%

---

# グローバル mbox について

グローバル mboxに関する情報。この名前は、[!DNL Adobe Target]実装内の各web ページの上部で行われた単一のサーバーコールを参照するために使用されます。

デフォルトでは、グローバル mbox は、`target-global-mbox` という名前が付いています。 必要に応じて、アカウント用に名前を変更できます。

標準の mbox（非グローバル mbox）とグローバル mbox には、次のようないくつかの違いがあります。

| 標準の mbox | グローバル mbox |
|--- |--- |
| 標準の mbox は、通常、`<DIV>` タグでコンテンツを囲みます。 | グローバル mbox は、「空」で、コンテンツを囲みません。 |
| 1 つのアクティビティのみからのコンテンツは、1 つの標準の mbox に配信できます。 | 複数のアクティビティからのコンテンツは、1 つのグローバル mbox への 1 回の応答で配信できます。 |

複数のアクティビティがグローバル mboxまたは複数の通常のmboxを介して配信される場合、Target [は、アクティビティ（またはアクティビティ）がweb ページに配信される優先度](https://experienceleague.adobe.com/docs/target/using/activities/priority.html)を決定します。

追加のページレベルのデータは、[!DNL Target] 関数を使用することで、グローバル mbox と共に `[!UICONTROL targetPageParams]` に送信できます。 これは、mbox パラメーターの機能と同様です。 詳しくは、「[グローバル mbox にパラメーターを渡す](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md)」を参照してください。
