---
keywords: apple、ITP、インテリジェントトラッキング防止、experience cloud id、ecid、itp
description: Safari ユーザーのプライバシー保護を目的としたApple Intelligent Tracking Prevention （ITP）イニシアチブの影響と [!DNL Adobe Target] について説明します。
title: ' [!DNL Target] では、Apple ITP サポートをどのように処理しますか？'
feature: Privacy & Security
exl-id: 6deee03b-df86-4d0d-999c-b11855ddfda5
TQID: https://experienceleague.adobe.com/AvrlwiLa-soHwrGT1QMa8KgsiIwfwKaF-0LBxMjb8cs
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 681
ht-degree: 28%

---

# Apple Intelligent Tracking Prevention（ITP）2.x

Intelligent Tracking Prevention （ITP）は、Safari ユーザーのプライバシーを保護するためのAppleの取り組みです。 ITP の最初のリリース（2017 年）では、サードパーティ Cookie の使用を対象としていました。 実際、Apple は、サードパーティ Cookie を完全にブロックしましたが、サードパーティ Cookie は、訪問者の追跡および訪問者データの収集に一般的に使用されていたので、広告技術およびマーケティング技術会社にとって深刻な頭痛の種となりました。 現在、Apple は、Safari 内でのファーストパーティ Cookie の使用方法に制限や制約を課すことに移行しています。

ITP のこれらのバージョンには、次の制限が含まれています。

| バージョン | 詳細 |
| --- | --- |
| [ITP 2.1](https://webkit.org/blog/8613/intelligent-tracking-prevention-2-1/) | `document.cookie` API を使用してブラウザーに配置されたクライアントサイド Cookie の有効期限のキャップは 7 日間になりました。<br />2019 年 2 月 21 日にリリースされました。 |
| [ITP 2.2](https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/) | 有効期限のキャップが7 日から 1 日に大幅に短縮されました。<br />2019 年 4 月 24 日にリリースされました。 |
| [ITP 2.3](https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/) | localStorageの導入やJavaScript `Document.referrer property`の使用など、いくつかの回避策を廃止しました。<br />2019年9月23日にリリースされました。<br />CNAME クローキング防御機能は、Safari 14、macOS Big Sur、Catalina、Mojave、iOS 14、iPadOS 14でリリースされました。 サードパーティのCNAMEでクロークされたHTTP応答によって作成されたすべてのCookieは、7日以内に期限切れになるように設定されます。<br />2020年11月12日のお知らせ |

## [!DNL Target]のお客様としての私への影響は何ですか？

Targetでは、ページにデプロイできるJavaScript ライブラリを提供しています。これにより、[!DNL Target]が訪問者にリアルタイムのパーソナライゼーションを提供できます。 [!DNL Target]個のJavaScript ライブラリ at.js 1.*x*、at.js 2.*x*&#x200B;があります。これらは、`document.cookie` APIを介して訪問者のブラウザーに[!DNL Target] Cookieをクライアントサイドに配置する[!DNL Adobe Experience Cloud Web SDK]です。 その結果、[!DNL Target] CookieはAppleのITP 2.1、2.2、および2.3の影響を受け、7日後（ITP 2.1の場合）および1日後（ITP 2.2およびITP 2.3の場合）に有効期限が切れます。

Apple ITP 2.xは、次の領域で[!DNL Target]に影響を与えます。

| 影響 | 詳細 |
| --- | --- |
| ユニーク訪問者数が増加する可能性 | 有効期限が7日（ITP 2.1）と1日（ITP 2.2およびITP 2.3）に設定されているため、Safari ブラウザーからのユニーク訪問者が増加する可能性があります。 訪問者が7日後（ITP 2.1）または1日後（ITP 2.2およびITP 2.3）にドメインを再訪問した場合、[!DNL Target]は、期限切れのCookieの代わりに新しい[!DNL Target] Cookieをドメインに配置することを余儀なくされます。 新しい [!DNL Target] Cookie は、ユーザーが同じであっても、新しいユニーク訪問者と解釈されます。 |
| [!DNL Target] アクティビティのルックバック期間の短縮 | [!DNL Target] アクティビティの訪問者プロファイルは、決定のためのルックバック期間が短縮した可能性があります。 [!DNL Target] Cookie は、訪問者を特定するために活用され、パーソナライゼーション用にユーザープロファイル属性を格納します。 [!DNL Target]個のCookieは、7日間（ITP 2.1）または1日（ITP 2.2および2.3）後にSafariで有効期限が切れることを考慮すると、パージされた[!DNL Target]個のCookieに関連付けられたユーザープロファイルデータを意思決定に使用することはできません。 |
| サードパーティ ID に基づくプロファイルスクリプト | 有効期限が7日間（ITP 2.1）と1日（ITP 2.2およびITP 2.3）に設定されているため、3rdPartyID Cookieに基づく[&#x200B; プロファイルスクリプト &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html)は、有効期限が切れると機能しなくなります。 |
| iOS デバイスでの QA／プレビュー URL | 有効期限が7日間（ITP 2.1）と1日（ITP 2.2およびITP 2.3）に設定されているため、URLが3rdPartyID Cookieに基づいているため、[QA/Preview URL](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html)は有効期限が切れると機能しなくなります。 |

## 現在の [!DNL Target] の実装は影響を受けますか？

[!DNL Target] JavaScript ライブラリに加えてExperience Cloud ID （ECID） ライブラリを使用している場合、実装は次の記事に記載されている方法で影響を受けます。[Safari ITP 2.1 Adobe Experience CloudおよびExperience Platformのお客様への影響](https://medium.com/adobetech/safari-itp-2-1-impact-on-adobe-experience-cloud-customers-9439cecb55ac)。
