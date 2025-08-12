---
title: Experience Platform Web SDKでの A4T データのサーバーサイドログ
description: Experience Platform web SDKを使用して、Adobe Analytics for Target （A4T）のサーバーサイドログを有効にする方法を説明します。
seo-title: Server-side logging for A4T data in Experience Platform Web SDK
seo-description: Learn how to enable server-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: a4t;target;web;sdk;platform；ログ；
feature: Implementation
source-git-commit: b1b0424bfe61fb8b4e88723e6bb2c565d75f8351
workflow-type: tm+mt
source-wordcount: '144'
ht-degree: 0%

---

# [!DNL Experience Platform Web SDK] での A4T データのサーバーサイドログ

[!DNL Adobe Experience Platform Web SDK] を使用すると、[!UICONTROL Adobe Analytics for Target] に [!UICONTROL Experience Platform Edge Network] （A4T）機能を実装できます。 サーバーサイドログが有効な場合は、Edge Networkを介して送信される [!DNL Analytics] のヒットすべてが、ヒットのステッチプロセスを経ることなく、サーバーサイドの [!DNL Target] 細で拡張されます。

[!DNL Analytics] のサーバーサイドログは、データストリーム設定で [!DNL Analytics] が有効になっている場合に有効になります。

![Analytics データストリーム設定が有効 ](/help/dev/implement/a4t/assets/enable-analytics-datastream.png)

次の図は、サーバーサイド [!DNL Analytics] ログが有効な場合のシステム内でのデータのフローを示しています。

![ サーバーサイドログフロー ](/help/dev/implement/a4t/assets/analytics-server-side-logging.png)

## 次の手順

このガイドでは、Web SDKでの A4T データのサーバーサイドログについて説明しました。 クライアントサイドでの A4T データの処理方法について詳しくは、[ クライアントサイドログ ](/help/dev/implement/a4t/client-side-logging.md) に関するガイドを参照してください。