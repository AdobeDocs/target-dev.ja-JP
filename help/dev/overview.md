---
keywords: target 開発者ガイド；概要；ホーム
title: Adobe Target デベロッパーガイド
description: ' [!DNL Adobe Target]  を実装および管理し、その API および SDK を操作する方法'
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: 655cff9b-fc04-45cf-9068-5c6c32b70d79
source-git-commit: 1d834edf4da94614e3a4be665ebd97399439ec98
workflow-type: tm+mt
source-wordcount: '480'
ht-degree: 12%

---

# [!DNL Adobe Target] 開発者用ガイド

**([表示 [!DNL Target] ドキュメントの更新](https://experienceleague.adobe.com/docs/target/using/release-notes/doc-change.html){target=_blank})**

この *[!DNL Adobe Target]開発者ガイド* は、次のリソースとガイドを提供します。 [!DNL Target] 開発者（実装および管理する API や SDK のドキュメントを含む） [!DNL Target].

>[!NOTE]
>
>このガイドに加えて、次の [!DNL Adobe Target] ガイドも使用できます。
>
>* [*[!DNL Adobe Target] 実務者ガイド&#x200B;*](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=ja){target=_blank}
>
>* [*[!DNL Adobe Target] Tutorials *](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html?lang=ja){target=_blank}
>
>リリース情報については、 [Target リリースノート（現行）](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html){target=_blank} （内） *[!DNL Adobe Target]実務者ガイド*.

## 実装の手引き

**[実装する前に](/help/dev/before-implement/considerations-before-you-implement-target.md)**：実装の前に対処すべき考慮事項 [!DNL Adobe Target].

## クライアント側実装

[**Adobe Experience Platform Web SDK**](/help/dev/implement/client-side/aep-web-sdk.md): [!DNL Adobe Experience Platform Web SDK] を使用すると、 [!DNL Experience Cloud] （次を含む） [!DNL Target]) 経由で [!UICONTROL AdobeExperience Edge ネットワーク].

[**Target at.js JavaScript ライブラリ**](/help/dev/implement/client-side/overview.md):at.js JavaScript ライブラリは、Web 実装のページ読み込み時間を改善し、セキュリティを強化して、シングルページアプリケーション向けのより優れた実装オプションを提供します。

## サーバー側実装

[**Target SDK の概要**](implement/server-side/server-side-overview.md)：はじめに [!DNL Adobe Target] SDK（On-device decisioning を含む）。

[**Node.js SDK**](implement/server-side/node-js/overview.md)：の使用方法 [!DNL Target] Node.js SDK.

[**Java SDK**](implement/server-side/java/overview.md)：の使用方法 [!DNL Target] Java SDK。

[**.NET SDK**](implement/server-side/net/overview.md)：の使用方法 [!DNL Target] .NET SDK.

[**Python SDK**](implement/server-side/python/overview.md)：の使用方法 [!DNL Target] Python SDK。

## ハイブリッド実装

[**ハイブリッドデプロイメント**](implement/hybrid/hybrid-overview.md)：の実装 [!DNL Target] クライアント側とサーバー側の実装の組み合わせを使用する。

## Recommendations実装

[**Recommendations実装**](implement/recommendations/recommendations.md)：計画と実装 [!DNL Adobe Target Recommendations].

## モバイルアプリの実装

[**AEP Mobile SDK の概要**](implement/mobile/overview.md)：実装方法の概要 [!DNL Adobe Target] 次を使用 [!DNL Adobe Experience Platform] モバイル SDK。

[**AEP Mobile SDK リファレンス**](https://developer.adobe.com/client-sdks/documentation/)：の実装 [!DNL Adobe Target] 次を使用 [!DNL Adobe Experience Platform] モバイル SDK。

## メールの実装

[**E メールの概要**](implement/email/overview.md)：実装方法の概要 [!DNL Adobe Target] 電子メール内。

## API ガイド

[**はじめに**](before-administer/target-api-overview.md)：の概要 [!DNL Adobe Target] API

[**[!DNL Target Delivery API]**](/help/dev/implement/delivery-api/overview.md)：を使用します。 [!DNL Adobe Target] Web チャネルとモバイルチャネルをまたいでエクスペリエンスを配信する Delivery API と、接続された TV、キオスク、店内のデジタル画面など、ブラウザーベース以外の IoT デバイス。

[**[!DNL Target Admin API]**](administer/admin-api/admin-api-overview-new.md)：を使用します。 [!DNL Adobe Target] プロパティ、アクティビティ、オーディエンス、オファー、プロパティ、レポート、mbox、ホスト、環境などを管理するための管理 API。

[**[!DNL Target Profile API]**](https://developers.adobetarget.com/api/#profiles)：取得 [!DNL Adobe Target] ユーザープロファイル情報。

[**[!DNL Target Reporting API]**](https://developer.adobe.com/target/administer/admin-api/#tag/Reports)：取得 [!UICONTROL A/B テスト] および [!UICONTROL Automated Personalization] アクティビティレポートデータ。

[**[!DNL Target Recommendations API]**](http://developers.adobetarget.com/api/recommendations/)：を使用します。 [!DNL Target Recommendations] API.

[**[!DNL Target Models API]**](administer/models-api/models-api-overview.md)：を管ブロックリストに加える理して、 [!DNL Target] 機械学習モデル。

[**Admin ConsoleAPI**](https://developer.adobe.com/umapi/)：ユーザーユーザー管理 API とユーザー同期 API を使用して、Adobeと製品の使用権限を管理します。

[**[!DNL Adobe Experience Platform Edge Network Server API]**](https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/overview.html)：を使用します。 [!DNL Adobe Experience Platform Edge Network Server] 様々なデータ収集、パーソナライゼーション、広告、マーケティングの使用例に対応する API です。

## リソース

* [Adobeオープンソースリポジトリ](https://github.com/adobe)
* [Target Node JS SDK のソース](https://github.com/adobe/target-nodejs-sdk)
* [Target Node JS SDK の例リポジトリ](https://github.com/adobe/target-nodejs-sdk-samples)
* [Target Java SDK のソース](https://github.com/adobe/target-java-sdk)
* [Target Java SDK の例リポジトリ](https://github.com/adobe/target-java-sdk-samples)
* [Target の実装](./before-implement/prepare-to-implement-target.md)
* [ターゲット管理](./before-administer/target-api-overview.md)
* [Adobe Target開発ドキュメント GitHub リポジトリ](https://github.com/AdobeDocs/target-developers)
* [Adobe Targetリリースノート](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html)
* [Adobe Target Business ユーザーガイド](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=ja)

