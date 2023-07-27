---
keywords: 実装，実装， at.js, adobe experience platform web sdk, aep web sdk
description: の実装方法を学ぶ [!DNL Adobe Target] を使用してクライアント側 Web の場合 [!DNL Adobe Experience Platform Web SDK] (AEP Web SDK) または at.js JavaScript ライブラリ。
title: 実装方法 [!DNL Target] （クライアント側 Web 用）
feature: at.js
exl-id: b3a850ff-ace0-4eea-955a-aa71dfad256f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '225'
ht-degree: 30%

---

# 概要：実装 [!DNL Target] クライアント側 Web の場合

クライアント側での [!DNL Adobe Target] の実装では、[!DNL Target] アクティビティに関連付けられたエクスペリエンスをクライアントブラウザーに直接配信します。ブラウザーは、表示するエクスペリエンスを決定して表示します。クライアント側の実装では、WYSIWYG エディタ、[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) （VEC）、または非視覚的インタフェースである[フォームベースの Experience Composer ](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)を使用して、アクティビティとパーソナライズされたエクスペリエンスを作成できます。

実装するには [!DNL Target] クライアント側では、次の JavaScript ライブラリのいずれかを使用する必要があります。

* [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md)

  The [!UICONTROL Adobe Experience Platform Web SDK] を使用すると、 [!DNL Adobe Experience Cloud] （次を含む） [!DNL Target]) 経由で [!UICONTROL Adobe Experience Edge Network]. 移行先の [!UICONTROL Adobe Experience Platform Web SDK]を参照してください。 [説明 [!UICONTROL Adobe Experience Platform Web SDK]](/help/dev/implement/client-side/aep-web-sdk.md).

* [[!DNL Target] at.js JavaScript ライブラリ](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md)

  at.js JavaScript ライブラリは、Web 実装のページ読み込み時間を改善し、セキュリティを強化して、シングルページアプリケーション向けのより優れた実装オプションを提供します。 at.js に移行する場合は、 [At.js の仕組み](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) および [[!DNL Adobe Target] スキルビルダー：開発者チャット、Adobe Targetを使用して mbox.js を at.js に移行](https://seminars.adobeconnect.com/ptdo6mfo6qn6/?proto=true).
