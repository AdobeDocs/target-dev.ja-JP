---
keywords: Adobe Experience Platform Web SDK、aep web sdk、web sdk、sdk、adobe experience cloud、platform edge network、adobe experience platform edge network、edge network、aep edge network、Adobe Experience Platform Web SDK0
description: '[!UICONTROL Adobe Experience Platform Web SDK]を使用して、[!UICONTROL Adobe Experience Cloud]から[!UICONTROL AEP Edge Network]の様々なサービスを操作する方法について説明します。'
title: '[!UICONTROL Experience Platform Web SDK]を実装するにはどうすればよいですか？'
feature: AEP Web SDK
exl-id: 35ee60d2-3d6d-4169-9f22-b2aef4c6548b
TQID: https://experienceleague.adobe.com/j3-KSuCkcyyTB2KG4Icm2E7xpAfcuPkaOlhxitd5q-4
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2:
  - id: df62f171-ac37-440f-8f0f-f41a72ebdd34
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: b6b447ccb88925a8efb6ff6a80ae475c8780dbc8
workflow-type: tm+mt
source-wordcount: 844
ht-degree: 8%

---

# [!UICONTROL Adobe Experience Platform Web SDK]

[!UICONTROL Adobe Experience Platform Web SDK] （AEP Web SDK）は、[!UICONTROL Adobe Experience Cloud]のお客様が[!DNL Adobe Experience Cloud]の様々なサービス（[!DNL Target]を含む）と[!UICONTROL Adobe Experience Platform Edge Network]を操作できるようにする、クライアントサイドのJavaScript ライブラリです。 JavaScript ライブラリに加えて、Web SDK設定に役立つ[!UICONTROL Adobe Experience Platform]拡張機能があります。

>[!IMPORTANT]
>
>[!DNL Target]を[!UICONTROL Adobe Experience Platform Web SDK]で実装する場合、リクエストと応答は、[!DNL Target] [配信API](/help/dev/implement/delivery-api/overview.md)ではなく、`sendEvent` コマンドを使用して[!UICONTROL Experience Platform Edge Network]）経由でInteract APIを経由します。 [!DNL Delivery API]は、[!DNL at.js]および直接サーバーサイド実装のみを対象としています。 [at.js ライブラリとExperience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/web-sdk-atjs-comparison.md)を比較して、2つのアプローチがどのように異なるかを理解してください。

詳しくは、*[!UICONTROL Adobe Experience Platform Web SDK]* ヘルプの次のリンクを参照してください。

* 包括的な情報：[Adobe Experience Platform Web SDKとは](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja)
* [!DNL Target]に固有の情報：[[!DNL Target] 概要](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/target-overview.html?lang=ja)

## チュートリアル

次のチュートリアルは、実装に役立ちます。

### [!DNL Adobe Experience Cloud]を[!DNL Platform Web SDK]で実装

[このチュートリアル &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ja)で[!DNL Adobe Experience Platform Web SDK]を使用して[!DNL Experience Cloud]個のアプリケーションを実装する方法について説明します。 [!DNL Target]について詳しくは、「[Platform Web SDKを使用した設定 [!DNL Target]  チュートリアル」の節を参照してください。](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html?lang=ja)

### [!DNL Target]をat.js 2.*x*&#x200B;から[!DNL Platform Web SDK]に移行

[!DNL Target]実装をat.js 2.*x*&#x200B;から[!DNL Adobe Experience Platform Web SDK]に[このチュートリアル &#x200B;](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=ja)と共に移行する方法について説明します。

## 推奨ドキュメント

上記の[!UICONTROL Platform Web SDK] ドキュメントに加えて、このガイドのトピックには、[!DNL Target]の機能に関連する[!UICONTROL Platform Web SDK]に固有の情報もあります。

| 機能 | 説明/リンク |
| --- | --- |
| [アクティビティ QA](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=ja) | [!DNL Target]のQA URLを使用すると、変更のないプレビューリンク、オプションのオーディエンスターゲティング、ライブアクティビティデータからセグメント化されたままのQA レポートを使用して、簡単なエンドツーエンドのアクティビティ QAを実行できます。 アクティビティ QAを使用すると、アクティビティを公開する前に[!DNL Target]個のアクティビティを完全にテストできます。<p>[Target JavaScript ライブラリ QA モードの互換性](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=ja#compatibility)および[&#x200B; プレビューURL](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=ja#preview)を参照してください。 |
| [[!UICONTROL &#x200B; ターゲットの分析] （A4T） &#x200B;](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja) | [!UICONTROL Target用Adobe Analytics] （A4T）は、[!DNL Analytics]個のコンバージョン指標とオーディエンスセグメントに基づいてアクティビティを作成できるソリューション間の統合機能です。 A4T統合では、Analytics レポートを使用して結果を調べることができます。<p>Adobe Experience Platform Web SDK実装の[&#x200B; サポートされているアクティビティタイプ &#x200B;](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja#section_F487896214BF4803AF78C552EF1669AA)および[実装手順](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html?lang=ja#platform)を参照してください。 |
| [オーディエンス](https://experienceleague.adobe.com/docs/target/using/audiences/target.html?lang=ja) | ターゲットを絞ったアクティビティでコンテンツとエクスペリエンスを見るユーザーは、[!DNL Target]のオーディエンスによって決まります。<p>[&#x200B; オーディエンスリストを使用](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=ja#use-list)および[複数のオーディエンスを組み合わせる](https://experienceleague.adobe.com/docs/target/using/audiences/combining-multiple-audiences.html?lang=ja)を参照してください。 |
| [オーディエンスの作成](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=ja) | [!DNL Adobe Experience Platform]で作成されたオーディエンスを使用すると、よりインパクトのあるパーソナライゼーションにつながる豊富な顧客データが得られます。<p>[Adobe Experience Platformのオーディエンスを使用](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=ja#aep)を参照してください。 |
| [&#x200B; オファー決定](https://experienceleague.adobe.com/docs/target/using/integrate/ajo/offer-decision.html?lang=ja) | [!DNL Adobe Journey Optimizer]で作成されたオファー決定を[!DNL Target] アクティビティ（手動A/B テストまたはエクスペリエンスのターゲット設定）に追加して、webおよびモバイル上の訪問者に対する次善のオファーを決定して配信します。 |
| [リダイレクトオファー - A4T FAQ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html?lang=ja) | リダイレクトオファーにより、訪問者のブラウザーが新しいページにリダイレクトされます。<p>[Adobe Experience Platform Web SDK]は、A4Tのリダイレクト オファーをサポートしていますか？(https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html?lang=ja#platform)を参照してください。 |
| [レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=ja) | Response トークンを使用すると、[!DNL Target] データをGoogle Analyticsやその他の3rd パーティ統合に送信できます。<p>このタスクを実行する方法のコードサンプルについては、[Platform Web SDK](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=ja#sending-data-to-google-analytics-via-platform-web-sdk)を介したGoogle Analyticsへのデータ送信を参照してください。 |
| *[!UICONTROL Platform Web SDK]の概要* ガイドの[&#x200B; シングルページアプリケーション実装](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/spa-implementation.html?lang=ja)。 | [!UICONTROL Adobe Experience Platform Web SDK]には、シングルページアプリケーション（SPA）などの次世代のクライアントサイド技術でパーソナライゼーションを実行するための豊富な機能が用意されています。 |
| [TLS（Transport Layer Security）暗号化の変更](/help/dev/before-implement/tls-transport-layer-security-encryption.md) | TLS （Transport Layer Security）は、最高レベルのセキュリティ標準を維持し、顧客データの安全性を向上させるのに役立ちます。 |
