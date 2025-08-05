---
keywords: Adobe Experience Platform Web SDK, aep web sdk, web sdk, sdk, adobe experience cloud, platform edge network, adobe experience platform edge network, edge network, aep edge network, Adobe Experience Platform Web SDK0
description: '[!UICONTROL Adobe Experience Platform Web SDK] を使用して、[!UICONTROL Adobe Experience Cloud] を通じて [!UICONTROL AEP Edge Network] の様々なサービスとやり取りする方法を説明します。'
title: '[!UICONTROL Experience Platform Web SDK] を使用してを実装する方法'
feature: AEP Web SDK
exl-id: 35ee60d2-3d6d-4169-9f22-b2aef4c6548b
source-git-commit: ac03d5d15875ab4945b07b3e95037ce9ecde1044
workflow-type: tm+mt
source-wordcount: '503'
ht-degree: 8%

---

# [!UICONTROL Adobe Experience Platform Web SDK]

[!UICONTROL Adobe Experience Platform Web SDK] （AEP Web SDK）は、[!UICONTROL Adobe Experience Cloud] のお客様が [!DNL Adobe Experience Cloud] を通じて [!DNL Target] の様々なサービス（[!UICONTROL Adobe Experience Platform Edge Network] など）を操作できるようにする、クライアントサイド JavaScript ライブラリです。 JavaScript ライブラリに加えて、web SDK設定に役立つ [!UICONTROL Adobe Experience Platform] 拡張機能があります。

詳しくは、*[!UICONTROL Adobe Experience Platform Web SDK]* ヘルプの次のリンクを参照してください。

* 包括的な情報を得る場合：[[!UICONTROL Adobe Experience Platform Web SDK]](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja) とは
* [!DNL Target] に固有の情報：[[!DNL Target]  概要 ](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/target-overview.html?lang=ja)

## チュートリアル

次のチュートリアルは、実装に役立ちます。

### [!DNL Adobe Experience Cloud] での [!DNL Platform Web SDK] の実装

[!DNL Experience Cloud] を使用して [!DNL Adobe Experience Platform Web SDK] アプリケーションを実装する方法について説明します [ このチュートリアル ](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html)。 [!DNL Target] について詳しくは、チュートリアルの [Platform Web SDKの設定  [!DNL Target]  を参照してください ](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html)。

### [!DNL Target] を at.js 2.*x* ～ [!DNL Platform Web SDK]

[!DNL Target] 実装を at.js 2.*x* [!DNL Adobe Experience Platform Web SDK] このチュートリアル [ を使用して ](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=ja) に移動します。

## 推奨ドキュメント

上記の [!UICONTROL Platform Web SDK] ドキュメントに加えて、このガイドのトピックには、[!UICONTROL Platform Web SDK] の機能に関連する、[!DNL Target] に固有の情報も含まれています。

| 機能 | 説明/リンク |
| --- | --- |
| [アクティビティ QA](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) | [!DNL Target] の QA URL を使用すると、変更のないプレビューリンク、オプションのオーディエンスのターゲット設定、ライブアクティビティデータからセグメント化されたままの QA レポートで、エンドツーエンドの簡単なアクティビティ QA を実行できます。 アクティビティ QA を使用すると、[!DNL Target] のアクティビティを、実稼動環境に移行する前に完全にテストできます。<p>[Target JavaScript ライブラリ QA モードの互換性 ](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#compatibility) および [ プレビュー URL](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#preview) を参照してください。 |
| [[!UICONTROL Analytics for Target] （A4T） ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) | [!UICONTROL Adobe Analytics for Target]（A4T）は、[!DNL Analytics] のコンバージョン指標とオーディエンスセグメントに基づいてアクティビティを作成できるクロスソリューション統合環境です。A4T 統合では、Analytics レポートを使用して結果を確認できます。<p>[ サポートされるアクティビティタイプ ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html#section_F487896214BF4803AF78C552EF1669AA) および [Adobe Experience Platform Web SDK実装の実装手順 ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html#platform) を参照してください。 |
| [オーディエンス](https://experienceleague.adobe.com/docs/target/using/audiences/target.html) | [!DNL Target] のオーディエンスは、ターゲットアクティビティで、コンテンツとエクスペリエンスをどのユーザーに表示するかを決定します。<p>[ オーディエンスリストの使用 ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#use-list) および [ 複数のオーディエンスを組み合わせる ](https://experienceleague.adobe.com/docs/target/using/audiences/combining-multiple-audiences.html) を参照してください。 |
| [オーディエンスの作成](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=ja) | [!DNL Adobe Experience Platform] で作成されたオーディエンスを使用すると、よりインパクトのあるパーソナライゼーションにつながる豊富な顧客データが提供されます。<p>[Adobe Experience Platformのオーディエンスの使用 ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#aep) を参照してください。 |
| [ オファーの決定 ](https://experienceleague.adobe.com/docs/target/using/integrate/ajo/offer-decision.html) | [!DNL Adobe Journey Optimizer] で作成したオファーの決定を [!DNL Target] アクティビティ（手動の A/B テストまたはエクスペリエンスのターゲット設定）に追加して、web やモバイルでの訪問者に次に最適なオファーを決定および配信します。 |
| [リダイレクトオファー - A4T FAQ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html) | リダイレクトオファーを使用すると、訪問者のブラウザーが新しいページにリダイレクトされます。<p>[[!UICONTROL Adobe Experience Platform Web SDK] は A4T のリダイレクトオファーをサポートしていますか？](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html#platform) を参照してください。 |
| [レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) | レスポンストークンを使用すると、Google Analyticsや他のサードパーティ統合環境に [!DNL Target] データを送信できます。<p>このタスクを実行する方法のコードサンプルについては、[Platform Web SDKを使用したGoogle Analyticsへのデータの送信 ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html#sending-data-to-google-analytics-via-platform-web-sdk) を参照してください。 |
| [ の概要ガイドの ](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/spa-implementation.html) シングルページアプリケーションの実装 *[!UICONTROL Platform Web SDK]を参照* てください。 | [!UICONTROL Adobe Experience Platform Web SDK] は、単一ページアプリケーション（SPA）などの次世代のクライアントサイドテクノロジーでパーソナライゼーションを実行できる豊富な機能を提供します。 |
| [TLS（Transport Layer Security）暗号化の変更](/help/dev/before-implement/tls-transport-layer-security-encryption.md) | TLS （Transport Layer Security）は、最高のセキュリティ標準を維持し、顧客データの安全性を高めるのに役立ちます。 |
