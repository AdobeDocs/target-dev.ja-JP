---
keywords: target デベロッパーガイド；概要；ホーム
title: Adobe Target開発者ガイド
description: を実装および管理  [!DNL Adobe Target] 、その API および SDK を操作する方法
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: 655cff9b-fc04-45cf-9068-5c6c32b70d79
source-git-commit: dadc3804da4592dba4ad88b8c5c9f804c56e232b
workflow-type: tm+mt
source-wordcount: '398'
ht-degree: 6%

---

# [!DNL Adobe Target] 開発者ガイド

**（[ 表示  [!DNL Target]  ドキュメントのアップデート ](https://experienceleague.adobe.com/docs/target/using/release-notes/doc-change.html?lang=ja){target=_blank}）**

この *[!DNL Adobe Target]Developer Guide では*[!DNL Target] を実装および管理するための API およびSDK ドキュメントなど、[!DNL Target] 開発者向けのリソースとガイドを提供します。

>[!NOTE]
>
>このガイドに加えて、次の [!DNL Adobe Target] ガイドも使用できます。
>
>* [*[!DNL Adobe Target] 実務担当者ガイド *](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=ja){target=_blank}
>
>* [*[!DNL Adobe Target] チュートリアル *](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html?lang=ja){target=_blank}
>
>リリース情報については、[*[!DNL Adobe Target]Business Practitioner Guide」の Target リリースノート（最新） ](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html?lang=ja){target=_blank} を参照してください*。

## 実装の概要

**[実装する前に](/help/dev/before-implement/considerations-before-you-implement-target.md)**:[!DNL Adobe Target] を実装する前に対処すべき考慮事項。

## クライアントサイドの実装

[**Adobe Experience Platform Web SDK**](/help/dev/implement/client-side/aep-web-sdk.md)：この [!DNL Adobe Experience Platform Web SDK] を使用すると、[!UICONTROL Adobe Experience Edge Network] を介して、[!DNL Experience Cloud] の様々なサービス（[!DNL Target] など）を操作できます。

[**Target at.js JavaScript ライブラリ**](/help/dev/implement/client-side/overview.md):at.js JavaScript ライブラリは、web 実装のページ読み込み時間を改善し、セキュリティを向上し、シングルページアプリケーション向けのより優れた実装オプションを提供します。

## サーバーサイドの実装

[**Target SDKの概要**](implement/server-side/server-side-overview.md)：オンデバイス判定を含む、[!DNL Adobe Target] SDK の基本を学びます。

[**Node.js SDK**](implement/server-side/node-js/overview.md):[!DNL Target] Node.js SDKの使用方法。

[**Java SDK**](implement/server-side/java/overview.md):[!DNL Target] Java SDKの使用方法。

[**.NET SDK**](implement/server-side/net/overview.md): [!DNL Target] .NET SDKの使用方法。

[**Python SDK**](implement/server-side/python/overview.md):[!DNL Target] Python SDKの使用方法。

## ハイブリッド実装

[**ハイブリッドデプロイメント**](implement/hybrid/hybrid-overview.md)：クライアントサイド実装とサーバーサイド実装を組み合わせて、[!DNL Target] を実装します。

## Recommendations の実装

[**Recommendations の実装**](implement/recommendations/recommendations.md):[!DNL Adobe Target Recommendations] の計画と実装。

## モバイルアプリの実装

[**AEP Mobile SDKの概要**](implement/mobile/overview.md):[!DNL Adobe Experience Platform] Mobile SDK を使用して [!DNL Adobe Target] を実装する方法の概要。

[**AEP Mobile SDK リファレンス**](https://developer.adobe.com/client-sdks/documentation/):[!DNL Adobe Experience Platform] Mobile SDK を使用して [!DNL Adobe Target] を実装します。

## メールの実装

[**メールの概要**](implement/email/overview.md)：メールに [!DNL Adobe Target] を実装する方法の概要。

## API ガイド

[**概要**](before-administer/target-api-overview.md):[!DNL Adobe Target] API の概要。

[**[!DNL Target Delivery API]**](/help/dev/implement/delivery-api/overview.md): [!DNL Adobe Target] 配信 API を使用すると、web チャネルやモバイルチャネルをまたいで、接続された TV、キオスク、店舗のデジタル画面などのブラウザー以外のベースの IoT デバイスでもエクスペリエンスを配信できます。

[**[!DNL Target Admin API]**](administer/admin-api/admin-api-overview-new.md):[!DNL Adobe Target] 管理 API を使用して、プロパティ、アクティビティ、オーディエンス、オファー、プロパティ、レポート、mbox、ホスト、環境などを管理します。

[**[!DNL Target Profile API]**](/help/dev/administer/profile-api/profiles-api.md)：ユーザープロファイル情報 [!DNL Adobe Target] 取得します。

[**[!DNL Target Reporting API]**](https://developer.adobe.com/target/administer/admin-api/#tag/Reports): [!UICONTROL A/B Test] および [!UICONTROL Automated Personalization] アクティビティレポートのデータを取得します。

[**[!DNL Target Recommendations API]**](https://developer.adobe.com/target/administer/recommendations-api/):[!DNL Target Recommendations] API を使用します。

[**[!DNL Target Models API]**](administer/models-api/models-api-overview.md)：機械学習モデルで使用される機能を定義するブロックリスト[!DNL Target] 管理します。

[**Admin Console API**](https://developer.adobe.com/umapi/): Adobe User Management および User Sync API を使用して、ユーザーおよび製品の使用権限を管理します。

[**[!DNL Adobe Experience Platform Edge Network Server API]**](https://experienceleague.adobe.com/docs/experience-platform/edge-network-server-api/overview.html?lang=ja)：様々なデータ収集、パーソナライゼーション、広告およびマーケティングのユースケースに [!DNL Adobe Experience Platform Edge Network Server] API を使用します。

## リソース

* [Adobe オープンソースリポジトリ ](https://github.com/adobe)
* [Target Node JS SDK Source](https://github.com/adobe/target-nodejs-sdk)
* [Target Node JS SDKのサンプルリポジトリ ](https://github.com/adobe/target-nodejs-sdk-samples)
* [Target Java SDK Source](https://github.com/adobe/target-java-sdk)
* [Target Java SDKのサンプルリポジトリ ](https://github.com/adobe/target-java-sdk-samples)
* [Target の実装](./before-implement/prepare-to-implement-target.md)
* [Target 管理](./before-administer/target-api-overview.md)
* [Adobe Target開発ドキュメント GitHub リポジトリ ](https://github.com/AdobeDocs/target-developers)
* [Adobe Target リリースノート ](https://experienceleague.adobe.com/docs/target/using/release-notes/release-notes.html?lang=ja)
* [Adobe Target ビジネスユーザーガイド ](https://experienceleague.adobe.com/docs/target/using/target-home.html?lang=ja)

