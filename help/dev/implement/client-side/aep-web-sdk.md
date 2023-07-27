---
keywords: Adobe Experience Platform Web SDK, aep web sdk, web sdk, sdk, adobe experience cloud, platform edge network, adobe experience platform edge network, edge network, aep edge network, Adobe Experience Platform Web SDK0
description: の使用方法を学ぶ [!UICONTROL Adobe Experience Platform Web SDK] 様々なサービスを使用するには、 [!UICONTROL Adobe Experience Cloud] から [!UICONTROL AEP Edge Network].
title: を使用してを実装する方法 [!UICONTROL Experience PlatformWeb SDK]?
feature: AEP Web SDK
exl-id: 35ee60d2-3d6d-4169-9f22-b2aef4c6548b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '749'
ht-degree: 12%

---

# [!UICONTROL Adobe Experience Platform Web SDK]

[!UICONTROL Adobe Experience Platform Web SDK] (AEP Web SDK) は、 [!UICONTROL Adobe Experience Cloud] 様々なサービスを使用するには、 [!DNL Adobe Experience Cloud] （次を含む） [!DNL Target]) 経由で [!UICONTROL Adobe Experience Platform Edge Network]. JavaScript ライブラリに加えて、 [!UICONTROL Adobe Experience Platform] 拡張機能を使用して Web SDK の設定を支援します。

詳しくは、 *[!UICONTROL Adobe Experience Platform Web SDK]* ヘルプ：

* 包括的な情報については、次の手順を実行します。 [説明 [!UICONTROL Adobe Experience Platform Web SDK]](https://experienceleague.adobe.com/docs/experience-platform/edge/home.html?lang=ja)
* 次に特有の情報： [!DNL Target]: [[!DNL Target] 概要](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/target-overview.html?lang=ja)

## チュートリアル

以下のチュートリアルは、の実装に役立ちます。

### 実装方法 [!DNL Adobe Experience Cloud] 次を使用 [!DNL Platform Web SDK]

の実装方法を学ぶ [!DNL Experience Cloud] アプリケーションの使用 [!DNL Adobe Experience Platform Web SDK] 次を使用 [このチュートリアル](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html). 次に特有の情報： [!DNL Target]（というタイトルのチュートリアルセクションを参照） [設定 [!DNL Target] Platform Web SDK を使用](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html).

### 移行 [!DNL Target] at.js 2.*x* から [!DNL Platform Web SDK]

移行方法を学ぶ [!DNL Target] at.js 2.*x* から [!DNL Adobe Experience Platform Web SDK] 次を使用 [このチュートリアル](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=ja).

## 推奨ドキュメント

また、 [!UICONTROL Platform Web SDK] 前述のドキュメント。このガイドのトピックには、 [!UICONTROL Platform Web SDK] ～に関しては [!DNL Target] 機能と機能に関する情報です。

| 機能 | 説明/リンク |
| --- | --- |
| [アクティビティ QA](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) | での QA URL の使用 [!DNL Target] 変更されないプレビューリンク、オプションのオーディエンスターゲティング、実際のアクティビティデータからセグメント化された QA レポートを使用して、簡単にエンドツーエンドのアクティビティ QA を実行できます。 アクティビティ QA を使用すると、 [!DNL Target] アクティビティを開始する前に有効にしておく必要があります。<p>詳しくは、 [Target JavaScript ライブラリ QA モードの互換性](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#compatibility) および [プレビュー URL](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html#preview). |
| [[!UICONTROL Analytics for Target]（A4T）](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html) | [!UICONTROL Adobe Analytics for Target] (A4T) は、 [!DNL Analytics] コンバージョン指標およびオーディエンスセグメント。 この A4T 統合により、Analytics レポートを使用して結果を確認できます。<p>詳しくは、 [サポートされるアクティビティのタイプ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html#section_F487896214BF4803AF78C552EF1669AA) および [Adobe Experience Platform Web SDK 実装の実装手順](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html#platform). |
| [オーディエンス](https://experienceleague.adobe.com/docs/target/using/audiences/target.html) | のオーディエンス [!DNL Target] ターゲットアクティビティでコンテンツとエクスペリエンスを表示する対象を決定します。<p>詳しくは、 [オーディエンスリストの使用](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html#use-list) および [複数のオーディエンスの結合](https://experienceleague.adobe.com/docs/target/using/audiences/combining-multiple-audiences.html). |
| [オーディエンスの作成](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/audiences.html?lang=ja) | [!DNL Adobe Experience Platform] で作成されたオーディエンスを使用すると、よりインパクトのあるパーソナライゼーションにつながる豊富な顧客データがを取得できます。<p>詳しくは、 [Adobe Experience Platformのオーディエンスを使用](https://experienceleague.adobe.com/docs/?lang=jatarget/using/audiences/create-audiences/audiences.html#aep). |
| [オファーの決定](https://experienceleague.adobe.com/docs/target/using/integrate/ajo/offer-decision.html) | で作成したオファーの決定を追加 [!DNL Adobe Journey Optimizer] から [!DNL Target] アクティビティ（手動の A/B テストまたはエクスペリエンスのターゲット設定）を使用して、Web およびモバイルで訪問者にとって次に最適なオファーを決定し、配信します。 |
| [リダイレクトオファー - A4T FAQ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html) | リダイレクトオファーを使用すると、訪問者のブラウザーは新しいページにリダイレクトされます。<p>詳しくは、 [を実行します。 [!UICONTROL Adobe Experience Platform Web SDK] A4T のリダイレクトオファーをサポートしますか？](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html#platform) |
| [レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html) | レスポンストークンを使用して [!DNL Target] のデータをGoogle Analyticsやその他のサードパーティ統合に送信する方法について説明します。<p>詳しくは、 [Platform Web SDK を使用してGoogle Analyticsにデータを送信する](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html#sending-data-to-google-analytics-via-platform-web-sdk) を参照して、このタスクを実行する方法のコードサンプルを確認してください。 |
| [シングルページアプリケーションの実装](https://experienceleague.adobe.com/docs/experience-platform/edge/personalization/adobe-target/spa-implementation.html) （内） *[!UICONTROL Platform Web SDK] 概要* ガイド。 | [!UICONTROL Adobe Experience Platform Web SDK] は、シングルページアプリケーション (SPA) など、次世代のクライアントサイドテクノロジーでパーソナライゼーションを実行するための機能を提供します。 |
| [TLS（Transport Layer Security）暗号化の変更](../../before-implement/tls-transport-layer-security-encryption.md) | TLS(Transport Layer Security) は、最高のセキュリティ標準を維持し、顧客データの安全を促進するのに役立ちます。 |
