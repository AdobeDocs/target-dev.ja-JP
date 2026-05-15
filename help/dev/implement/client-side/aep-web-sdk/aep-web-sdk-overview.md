---
keywords: Adobe Experience Platform Web SDK、aep web sdk、web sdk、sdk、adobe experience cloud、platform edge network、adobe experience platform edge network、edge network、aep edge network、Adobe Experience Platform Web SDK0
description: '[!UICONTROL Adobe Experience Platform Web SDK]を使用して、[!UICONTROL Adobe Experience Cloud]から[!UICONTROL AEP Edge Network]までの様々なサービスを操作する方法を説明します。'
title: '[!UICONTROL Experience Platform Web SDK]を実装するにはどうすればよいですか？'
feature: AEP Web SDK
exl-id: 35ee60d2-3d6d-4169-9f22-b2aef4c6548b
TQID: https://experienceleague.adobe.com/j3-KSuCkcyyTB2KG4Icm2E7xpAfcuPkaOlhxitd5q-4
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: adee20bd-51f4-461d-b9db-d215f8756eebid: c93393a4-e558-47e1-992e-c91ed4d480ceid: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2: id: df62f171-ac37-440f-8f0f-f41a72ebdd34id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: d095671a-1355-40aa-8b5f-06c33c68080bid: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 714
ht-degree: 11%

---

# [!UICONTROL Adobe Experience Platform Web SDK]

[!UICONTROL Adobe Experience Platform Web SDK] （AEP Web SDK）は、[!UICONTROL Adobe Experience Cloud]のお客様が[!DNL Adobe Experience Cloud] （[!DNL Target]を含む）から[!UICONTROL Adobe Experience Platform Edge Network]までの様々なサービスと対話できるようにする、クライアントサイドのJavaScript ライブラリです。 JavaScript ライブラリに加えて、Web SDK設定に役立つ[!UICONTROL Adobe Experience Platform]拡張機能があります。

詳しくは、*[!UICONTROL Adobe Experience Platform Web SDK]* ヘルプの次のリンクを参照してください。

* 包括的な情報：[[!UICONTROL Adobe Experience Platform Web SDK]](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja)とは
* [!DNL Target]に固有の情報：[[!DNL Target] 概要](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/target-overview.html?lang=ja)

## チュートリアル

次のチュートリアルは、実装に役立ちます。

### [!DNL Adobe Experience Cloud]を[!DNL Platform Web SDK]で実装

[このチュートリアル ](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ja)で[!DNL Adobe Experience Platform Web SDK]を使用して[!DNL Experience Cloud]個のアプリケーションを実装する方法について説明します。 [!DNL Target]について詳しくは、「[Platform Web SDKを使用した設定 [!DNL Target]  チュートリアル」の節を参照してください。](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html)

### [!DNL Target]をat.js 2.*x*&#x200B;から[!DNL Platform Web SDK]に移行

[!DNL Target]実装をat.js 2.*x*&#x200B;から[!DNL Adobe Experience Platform Web SDK]に[このチュートリアル ](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=ja)と共に移行する方法について説明します。

## 推奨ドキュメント

上記の[!UICONTROL Platform Web SDK] ドキュメントに加えて、このガイドのトピックには、[!DNL Target]の機能に関連する[!UICONTROL Platform Web SDK]に固有の情報もあります。

| 機能 | 説明/リンク |
| --- | --- |
| [アクティビティ QA](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) | [!DNL Target]のQA URLを使用すると、変更のないプレビューリンク、オプションのオーディエンスターゲティング、ライブアクティビティデータからセグメント化されたままのQA レポートを使用して、簡単なエンドツーエンドのアクティビティ QAを実行できます。 アクティビティ QAを使用すると、アクティビティを公開する前に[!DNL Target]個のアクティビティを完全にテストできます。<p>[Target JavaScript ライブラリ QA モードの互換性](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#compatibility)および[ プレビューURL](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#preview)を参照してください。 |
| [[!UICONTROL Analytics for Target] （A4T） ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) | [!UICONTROL Adobe Analytics for Target]（A4T）は、[!DNL Analytics] のコンバージョン指標とオーディエンスセグメントに基づいてアクティビティを作成できるクロスソリューション統合環境です。 A4T統合では、Analytics レポートを使用して結果を調べることができます。<p>Adobe Experience Platform Web SDK実装の[ サポートされているアクティビティタイプ ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html#section_F487896214BF4803AF78C552EF1669AA)および[実装手順](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html#platform)を参照してください。 |
| [オーディエンス](https://experienceleague.adobe.com/docs/target/using/audiences/target.html) | ターゲットを絞ったアクティビティでコンテンツとエクスペリエンスを見るユーザーは、[!DNL Target]のオーディエンスによって決まります。<p>[ オーディエンスリストを使用](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#use-list)および[複数のオーディエンスを組み合わせる](https://experienceleague.adobe.com/docs/target/using/audiences/combining-multiple-audiences.html)を参照してください。 |
| [オーディエンスの作成](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=ja) | [!DNL Adobe Experience Platform]で作成されたオーディエンスを使用すると、よりインパクトのあるパーソナライゼーションにつながる豊富な顧客データが得られます。<p>[Adobe Experience Platformのオーディエンスを使用](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#aep)を参照してください。 |
| [ オファー決定](https://experienceleague.adobe.com/docs/target/using/integrate/ajo/offer-decision.html) | [!DNL Adobe Journey Optimizer]で作成されたオファー決定を[!DNL Target] アクティビティ（手動A/B テストまたはエクスペリエンスのターゲット設定）に追加して、webおよびモバイル上の訪問者に対する次善のオファーを決定して配信します。 |
| [リダイレクトオファー - A4T FAQ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html) | リダイレクトオファーにより、訪問者のブラウザーが新しいページにリダイレクトされます。<p>[A4Tのリダイレクト オファーは[!UICONTROL Adobe Experience Platform Web SDK]でサポートされますか？](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html#platform)を参照してください |
| [レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) | Response トークンを使用すると、[!DNL Target] データをGoogle Analyticsやその他の3rd パーティ統合に送信できます。<p>このタスクを実行する方法のコードサンプルについては、[Platform Web SDK](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html#sending-data-to-google-analytics-via-platform-web-sdk)を介したGoogle Analyticsへのデータ送信を参照してください。 |
| *[!UICONTROL Platform Web SDK]概要* ガイドの[単一ページアプリケーション実装](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/spa-implementation.html)。 | [!UICONTROL Adobe Experience Platform Web SDK]には、シングルページアプリケーション （SPA）などの次世代のクライアントサイド技術でパーソナライゼーションを実行するための豊富な機能が用意されています。 |
| [TLS（Transport Layer Security）暗号化の変更](/help/dev/before-implement/tls-transport-layer-security-encryption.md) | TLS （Transport Layer Security）は、最高レベルのセキュリティ標準を維持し、顧客データの安全性を向上させるのに役立ちます。 |
