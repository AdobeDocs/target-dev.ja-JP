---
title: Experience Platform Web SDKでのA4T データのサーバーサイドロギング
description: Experience Platform Web SDKを使用して、Adobe Analytics for Target （A4T）のサーバーサイドログを有効にする方法について説明します。
seo-title: Server-side logging for A4T data in Experience Platform Web SDK
seo-description: Learn how to enable server-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: a4t;target;web;sdk;platform；ログ；
feature: Implementation
exl-id: 716f7343-69a6-44d7-baec-a0a0df1b6e1f
TQID: https://experienceleague.adobe.com/I7-G2VO2AN3qFsgkk4JX2Pg6WJfZq0HZkcGL4XQNoWg
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 161
ht-degree: 0%

---

# [!DNL Experience Platform Web SDK]のA4T データのサーバーサイドのログ

[!DNL Adobe Experience Platform Web SDK]を使用すると、[!UICONTROL Adobe Analytics Edge Network]で[!UICONTROL Experience Platform for Target] （A4T）機能を実装できます。 サーバーサイドのログ記録が有効になっている場合、Edge Networkを介して送信されたすべての[!DNL Analytics] ヒットは、ヒットのステッチプロセスを経由することなく、サーバーサイドの[!DNL Target]詳細で強化されます。

[!DNL Analytics]のサーバー側ログ記録は、データストリーム設定で[!DNL Analytics]が有効になっている場合に有効になります。

![Analytics データストリーム設定が有効になりました](/help/dev/implement/a4t/assets/enable-analytics-datastream.png)

次の図は、サーバーサイド [!DNL Analytics] ログが有効になっている場合のシステム内でのデータの流れを示しています。

![ サーバーサイドのログフロー](/help/dev/implement/a4t/assets/analytics-server-side-logging.png)

## 次のステップ

このガイドでは、Web SDKでのA4T データのサーバーサイドのログ記録について説明しました。 クライアントサイドでのA4T データの処理方法について詳しくは、[ クライアントサイドのログ記録](/help/dev/implement/a4t/client-side-logging.md)に関するガイドを参照してください。
