---
keywords: 実装、実装、at.js、adobe experience platform web sdk、aep web sdk
description: ' [!DNL Adobe Experience Platform Web SDK]  （AEP Web SDK）またはat.js JavaScript ライブラリを使用して、クライアントサイド web用に [!DNL Adobe Target] を実装する方法について説明します。'
title: クライアントサイド Web用に [!DNL Target] を実装する方法
feature: at.js
exl-id: b3a850ff-ace0-4eea-955a-aa71dfad256f
source-git-commit: ca53593287a5a58e6e0b9fe02b6e8b28788f9ff9
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 28%

---

# 概要：クライアントサイド web用に[!DNL Target]を実装する

クライアント側での [!DNL Adobe Target] の実装では、[!DNL Target] アクティビティに関連付けられたエクスペリエンスをクライアントブラウザーに直接配信します。 ブラウザーは、表示するエクスペリエンスを決定して表示します。 クライアント側の実装では、WYSIWYG エディタ、[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) （VEC）、または非視覚的インタフェースである[フォームベースの Experience Composer &#x200B;](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)を使用して、アクティビティとパーソナライズされたエクスペリエンスを作成できます。

[!DNL Target] クライアントサイドを実装するには、次のいずれかのJavaScript ライブラリを使用する必要があります。

* [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)

  [!UICONTROL Adobe Experience Platform Web SDK]を使用すると、[!DNL Adobe Experience Cloud] （[!DNL Target]を含む）の様々なサービスを[!UICONTROL Adobe Experience Edge Network]経由で操作できます。 [!UICONTROL Adobe Experience Platform Web SDK]への移行を選択した場合は、[概要[!UICONTROL Adobe Experience Platform Web SDK]](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)を参照してください。

* [[!DNL Target] at.js JavaScript library](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)

  at.js JavaScript ライブラリは、web実装のページ読み込み時間を短縮し、セキュリティを向上させ、シングルページアプリケーションのより優れた実装オプションを提供します。 at.jsへの移行を選択した場合は、[At.jsの仕組み](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)および[[!DNL Adobe Target] Skill Builder：開発者チャットを参照し、Adobe Targetのmbox.jsをat.js](https://seminars.adobeconnect.com/ptdo6mfo6qn6/?proto=true)に移行してください。


2つの実装アプローチの違いについて詳しくは、[at.js ライブラリとWeb SDKの比較](/help/dev/implement/client-side/aep-web-sdk/web-sdk-atjs-comparison.md)を参照してください。
