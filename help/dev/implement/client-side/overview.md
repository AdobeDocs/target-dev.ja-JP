---
keywords: 実装，実装，at.js, adobe experience platform web sdk, aep web sdk
description: ' [!DNL Adobe Target]  （AEP Web SDK）または at.js JavaScript ライブラリを使用してクライアントサイド Web に  [!DNL Adobe Experience Platform Web SDK]  を実装する方法を説明します。'
title: クライアント側 Web [!DNL Target]  実装方法
feature: at.js
exl-id: b3a850ff-ace0-4eea-955a-aa71dfad256f
source-git-commit: 7e2f1620c839393051432485192a45ddda2390a0
workflow-type: tm+mt
source-wordcount: '206'
ht-degree: 28%

---

# 概要：[!DNL Target] をクライアント側 web に実装する

クライアント側での [!DNL Adobe Target] の実装では、[!DNL Target] アクティビティに関連付けられたエクスペリエンスをクライアントブラウザーに直接配信します。ブラウザーは、表示するエクスペリエンスを決定して表示します。クライアント側の実装では、WYSIWYG エディタ、[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=ja) （VEC）、または非視覚的インタフェースである[フォームベースの Experience Composer ](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=ja)を使用して、アクティビティとパーソナライズされたエクスペリエンスを作成できます。

クライアント側 [!DNL Target] 実装するには、次のJavaScript ライブラリのいずれかを使用する必要があります。

* [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)

  [!UICONTROL Adobe Experience Platform Web SDK] を使用すると、[!DNL Adobe Experience Cloud] を介して、[!DNL Target] の様々なサービス（[!UICONTROL Adobe Experience Edge Network] など）を操作できます。 [!UICONTROL Adobe Experience Platform Web SDK] への移行を選択する場合は、[ 移行とは [!UICONTROL Adobe Experience Platform Web SDK]](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md) を参照してください。

* [[!DNL Target] at.js JavaScript ライブラリ](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)

  at.js JavaScript ライブラリは、web 実装におけるページの読み込み時間の向上、セキュリティの向上、シングルページアプリケーション向けのより優れた実装オプションの提供を実現します。 at.js に移行する場合は、[At.js の仕組み ](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) および [[!DNL Adobe Target]  スキルビルダー：Developer chat, migrate Adobe Targetの mbox.js から at.js への移行 ](https://seminars.adobeconnect.com/ptdo6mfo6qn6/?proto=true) を参照してください。


2 つの実装方法の違いについて詳しくは、[at.js ライブラリと web SDKの比較 ](/help/dev/implement/client-side/aep-web-sdk/web-sdk-atjs-comparison.md) を参照してください。