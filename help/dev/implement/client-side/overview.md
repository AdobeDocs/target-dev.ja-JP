---
keywords: 実装、実装、at.js、adobe experience platform web sdk、aep web sdk
description: ' [!DNL Adobe Experience Platform Web SDK]  （AEP Web SDK）またはat.js JavaScript ライブラリを使用して、クライアントサイド web用に [!DNL Adobe Target] を実装する方法について説明します。'
title: クライアントサイド Web用に [!DNL Target] を実装する方法
feature: at.js
exl-id: b3a850ff-ace0-4eea-955a-aa71dfad256f
TQID: https://experienceleague.adobe.com/KgJyhvTguS8EXbwELaApI1mcs5egnEKHKpnxVYGqT4I
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 929e1f10bc5dd0741f0fe28cd46435e680a4a308
workflow-type: tm+mt
source-wordcount: 252
ht-degree: 26%

---

# 概要：クライアントサイド web用に[!DNL Target]を実装する

クライアント側での [!DNL Adobe Target] の実装では、[!DNL Target] アクティビティに関連付けられたエクスペリエンスをクライアントブラウザーに直接配信します。 ブラウザーは、表示するエクスペリエンスを決定して表示します。 クライアント側の実装では、WYSIWYG エディタ、[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=ja) （VEC）、または非視覚的インタフェースである[フォームベースの Experience Composer &#x200B;](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=ja)を使用して、アクティビティとパーソナライズされたエクスペリエンスを作成できます。

[!DNL Target] クライアントサイドを実装するには、次のいずれかのJavaScript ライブラリを使用する必要があります。

* [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)

  [!UICONTROL Adobe Experience Platform Web SDK]を使用すると、[!UICONTROL Adobe Experience Edge Network]を通じて[!DNL Adobe Experience Cloud] （[!DNL Target]を含む）の様々なサービスを利用できます。 [!UICONTROL Adobe Experience Platform Web SDK]への移行を選択した場合は、[Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)の概要を参照してください。

* [[!DNL Target] at.js JavaScript library](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)

  at.js JavaScript ライブラリは、web実装のページ読み込み時間を短縮し、セキュリティを向上させ、シングルページアプリケーションのより優れた実装オプションを提供します。 at.jsへの移行を選択した場合は、[At.jsの仕組み](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)および[[!DNL Adobe Target] Skill Builder：開発者チャットを参照し、Adobe Targetのmbox.jsをat.js](https://seminars.adobeconnect.com/ptdo6mfo6qn6/?proto=true)に移行してください。


2つの実装アプローチの違いについて詳しくは、[at.js ライブラリとWeb SDKの比較](/help/dev/implement/client-side/aep-web-sdk/web-sdk-atjs-comparison.md)を参照してください。
