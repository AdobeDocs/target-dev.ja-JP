---
keywords: Adobe Experience Platform Web SDK, aep web sdk, web sdk, sdk, adobe experience cloud, platform edge network, adobe experience platform edge network, edge network, aep edge network, Adobe Experience Platform Web SDK0
description: '[!UICONTROL Adobe Experience Platform Web SDK] を使用して、[!UICONTROL AEP Edge Network] を通じて [!UICONTROL Adobe Experience Cloud] の様々なサービスとやり取りする方法を説明します。'
title: '[!UICONTROL Experience Platform Web SDK] を使用してを実装する方法'
feature: AEP Web SDK
exl-id: 35ee60d2-3d6d-4169-9f22-b2aef4c6548b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 8%

---

# [!UICONTROL Adobe Experience Platform Web SDK]

[!UICONTROL Adobe Experience Platform Web SDK] （AEP Web SDK）は、[!UICONTROL Adobe Experience Cloud] のユーザーが [!UICONTROL Adobe Experience Platform Edge Network] を介して [!DNL Adobe Experience Cloud] の様々なサービス（[!DNL Target] など）を操作できるようにする、クライアントサイド JavaScript ライブラリです。 JavaScript ライブラリに加えて、Web SDK 設定に役立つ [!UICONTROL Adobe Experience Platform] 拡張機能があります。

詳しくは、*[!UICONTROL Adobe Experience Platform Web SDK]* ヘルプの次のリンクを参照してください。

* 包括的な情報を得る場合：[[!UICONTROL Adobe Experience Platform Web SDK]](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja) とは
* [!DNL Target] に固有の情報：[[!DNL Target]  概要 ](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/target-overview.html?lang=ja)

## チュートリアル

次のチュートリアルは、実装に役立ちます。

### [!DNL Platform Web SDK] での [!DNL Adobe Experience Cloud] の実装

[!DNL Adobe Experience Platform Web SDK] を使用して [!DNL Experience Cloud] アプリケーションを実装する方法について説明します [ このチュートリアル ](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ja)。 [!DNL Target] について詳しくは、チュートリアルの [Platform Web SDK の設定  [!DNL Target]  を参照してください ](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html?lang=ja)。

### [!DNL Target] を at.js 2.*x* ～ [!DNL Platform Web SDK]

[!DNL Target] 実装を at.js 2.*x* [ このチュートリアル ](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=ja) を使用して [!DNL Adobe Experience Platform Web SDK] に移動します。

## 推奨ドキュメント

上記の [!UICONTROL Platform Web SDK] ドキュメントに加えて、このガイドのトピックには、[!DNL Target] の機能に関連する、[!UICONTROL Platform Web SDK] に固有の情報も含まれています。

| 機能 | 説明/リンク |
| --- | --- |
| [アクティビティ QA](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=ja) | [!DNL Target] の QA URL を使用すると、変更のないプレビューリンク、オプションのオーディエンスのターゲット設定、ライブアクティビティデータからセグメント化されたままの QA レポートで、エンドツーエンドの簡単なアクティビティ QA を実行できます。 アクティビティ QA を使用すると、[!DNL Target] のアクティビティを、実稼動環境に移行する前に完全にテストできます。<p>[Target JavaScript ライブラリ QA モードの互換性 ](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=ja#compatibility) および [ プレビュー URL](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=ja#preview) を参照してください。 |
| [[!UICONTROL Analytics for Target] （A4T） ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja) | [!UICONTROL Adobe Analytics for Target]（A4T）は、[!DNL Analytics] のコンバージョン指標とオーディエンスセグメントに基づいてアクティビティを作成できるクロスソリューション統合環境です。A4T 統合では、Analytics レポートを使用して結果を確認できます。<p>[ サポートされるアクティビティタイプ ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja#section_F487896214BF4803AF78C552EF1669AA) および [Adobe Experience Platform Web SDK 実装の実装手順 ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html?lang=ja#platform) を参照してください。 |
| [オーディエンス](https://experienceleague.adobe.com/docs/target/using/audiences/target.html?lang=ja) | [!DNL Target] のオーディエンスは、ターゲットアクティビティで、コンテンツとエクスペリエンスをどのユーザーに表示するかを決定します。<p>[ オーディエンスリストの使用 ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=ja#use-list) および [ 複数のオーディエンスを組み合わせる ](https://experienceleague.adobe.com/docs/target/using/audiences/combining-multiple-audiences.html?lang=ja) を参照してください。 |
| [オーディエンスの作成](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=ja) | [!DNL Adobe Experience Platform] で作成されたオーディエンスを使用すると、よりインパクトのあるパーソナライゼーションにつながる豊富な顧客データが提供されます。<p>[Adobe Experience Platformのオーディエンスの使用 ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=ja#aep) を参照してください。 |
| [ オファーの決定 ](https://experienceleague.adobe.com/docs/target/using/integrate/ajo/offer-decision.html?lang=ja) | [!DNL Adobe Journey Optimizer] で作成したオファーの決定を [!DNL Target] アクティビティ（手動の A/B テストまたはエクスペリエンスのターゲット設定）に追加して、web やモバイルでの訪問者に次に最適なオファーを決定および配信します。 |
| [リダイレクトオファー - A4T FAQ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html?lang=ja) | リダイレクトオファーを使用すると、訪問者のブラウザーが新しいページにリダイレクトされます。<p>[[!UICONTROL Adobe Experience Platform Web SDK] は A4T のリダイレクトオファーをサポートしていますか？](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html?lang=ja#platform) を参照してください。 |
| [レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=ja) | レスポンストークンを使用すると、Google Analyticsや他のサードパーティ統合環境に [!DNL Target] データを送信できます。<p>このタスクを実行する方法のコードサンプルについては、[Platform Web SDK を使用したGoogle Analyticsへのデータの送信 ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=ja#sending-data-to-google-analytics-via-platform-web-sdk) を参照してください。 |
| *[!UICONTROL Platform Web SDK]の概要ガイドの [ シングルページアプリケーションの実装 ](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/spa-implementation.html?lang=ja) を参照* てください。 | [!UICONTROL Adobe Experience Platform Web SDK] は、シングルページアプリケーション（SPA）などの次世代のクライアントサイドテクノロジーでパーソナライゼーションを実行できる豊富な機能を提供します。 |
| [TLS（Transport Layer Security）暗号化の変更](../../before-implement/tls-transport-layer-security-encryption.md) | TLS （Transport Layer Security）は、最高のセキュリティ標準を維持し、顧客データの安全性を高めるのに役立ちます。 |
