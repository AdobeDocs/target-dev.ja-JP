---
keywords: 実装，実装，at.js, adobe experience platform web sdk, aep web sdk
description: 実装方法を学ぶ [!DNL Adobe Target] を使用したクライアントサイド web の場合 [!DNL Adobe Experience Platform Web SDK] （AEP Web SDK）または at.js JavaScript ライブラリ。
title: 実装方法 [!DNL Target] クライアント側 Web の場合
feature: at.js
exl-id: b3a850ff-ace0-4eea-955a-aa71dfad256f
source-git-commit: 2d2a593df661c7e6c6e6384af6042e8aa4575fdb
workflow-type: tm+mt
source-wordcount: '207'
ht-degree: 28%

---

# 概要：の実装 [!DNL Target] クライアント側 web の場合

クライアント側での [!DNL Adobe Target] の実装では、[!DNL Target] アクティビティに関連付けられたエクスペリエンスをクライアントブラウザーに直接配信します。ブラウザーは、表示するエクスペリエンスを決定して表示します。クライアント側の実装では、WYSIWYG エディタ、[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) （VEC）、または非視覚的インタフェースである[フォームベースの Experience Composer ](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)を使用して、アクティビティとパーソナライズされたエクスペリエンスを作成できます。

を実装するには [!DNL Target] クライアント側では、次のいずれかの JavaScript ライブラリを使用する必要があります。

* [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md)

  この [!UICONTROL Adobe Experience Platform Web SDK] では、で各種サービスを操作できます [!DNL Adobe Experience Cloud] （次を含む） [!DNL Target]）に含まれます。 [!UICONTROL Adobe Experience Edge Network]. に移行する場合 [!UICONTROL Adobe Experience Platform Web SDK]を参照してください [について [!UICONTROL Adobe Experience Platform Web SDK]](/help/dev/implement/client-side/aep-web-sdk.md).

* [[!DNL Target] at.js JavaScript ライブラリ](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)

  at.js JavaScript ライブラリは、web 実装のページ読み込み時間を改善し、セキュリティを向上し、単一ページアプリケーション向けのより優れた実装オプションを提供します。 at.js に移行する場合は、を参照してください。 [At.js の仕組み](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) および [[!DNL Adobe Target] スキルビルダー：開発者チャット、Adobe Targetの mbox.js を at.js に移行€](https://seminars.adobeconnect.com/ptdo6mfo6qn6/?proto=true).


参照： [at.js ライブラリと Web SDK の比較](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/adobe-target/web-sdk-atjs-comparison){target=_blank} 2 つの実装方法の違いについて説明します。