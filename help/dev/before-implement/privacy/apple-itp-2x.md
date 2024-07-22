---
keywords: apple, ITP, インテリジェントなトラッキング防止，experience cloud id, ecid, itp
description: Safari ユーザー  [!DNL Adobe Target]  プライバシーの保護を目指すAppleの Intelligent Tracking Prevention （ITP）イニシアチブの概要と影響について説明します。
title: はApple ITP のサポ  [!DNL Target]  トをどのように処理しますか？
feature: Privacy & Security
exl-id: 6deee03b-df86-4d0d-999c-b11855ddfda5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 30%

---

# Apple Intelligent Tracking Prevention（ITP）2.x

Intelligent Tracking Prevention （ITP）は、Safari ユーザーのプライバシーを保護するためのAppleの取り組みです。 ITP の最初のリリース（2017 年）では、サードパーティ Cookie の使用を対象としていました。実際、Apple は、サードパーティ Cookie を完全にブロックしましたが、サードパーティ Cookie は、訪問者の追跡および訪問者データの収集に一般的に使用されていたので、広告技術およびマーケティング技術会社にとって深刻な頭痛の種となりました。現在、Apple は、Safari 内でのファーストパーティ Cookie の使用方法に制限や制約を課すことに移行しています。

ITP のこれらのバージョンには、次の制限が含まれています。

| バージョン | 詳細 |
| --- | --- |
| [ITP 2.1](https://webkit.org/blog/8613/intelligent-tracking-prevention-2-1/) | `document.cookie` API を使用してブラウザーに配置されたクライアント側 Cookie の有効期限の上限は 7 日間になりました。<br />2019 年 2 月 21 日にリリースされました。 |
| [ITP 2.2](https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/) | 7 日間の有効期限の上限が 1 日に大幅に短縮されました。<br />2019 年 4 月 24 日にリリースされました。 |
| [ITP 2.3](https://webkit.org/blog/9521/intelligent-tracking-prevention-2-3/) | localStorage の使用やJavaScript `Document.referrer property` の使用など、いくつかの回避策を排除しました。<br />2019 年 9 月 23 日（PT）にリリースされました。<br />ITP に対する CNAME クローク防御機能が、Safari 14、macOS ビッグサー、Catalina、Mojave、iOS 14、iPadOS 14 でリリースされました。 サードパーティの CNAME で保護された HTTP 応答によって作成されたすべての Cookie は、7 日後に有効期限が切れるように設定されます。<br />2020 年 11 月 12 日（PT）お知らせ。 |

## [!DNL Target] 顧客としての私への影響は何ですか？

Target は、JavaScript ライブラリを提供し、ページにデプロイして、訪問者にリアルタイムのパーソナライゼーションを提供で [!DNL Target] るようにします。 JavaScript ライブラリには at.js 1[!DNL Target]3 つあります。*x*、at.js 2.*x*:`document.cookie` API を介して訪問者のブラウザーにクライアントサイドの [!DNL Target] Cookie を配置する [!DNL Adobe Experience Cloud Web SDK]。 その結果、[!DNL Target] cookie はAppleの ITP 2.1、2.2 および 2.3 の影響を受け、7 日後（ITP 2.1 の場合）、1 日後（ITP 2.2 および ITP 2.3 の場合）に有効期限が切れます。

Apple ITP 2.x は、次の領域の [!DNL Target] に影響します。

| 影響 | 詳細 |
| --- | --- |
| 個別訪問者数が増加する可能性 | 有効期限が 7 日（ITP 2.1 の場合）と 1 日（ITP 2.2 および ITP 2.3 の場合）に設定されているので、Safari ブラウザーからのユニーク訪問者が増加する可能性があります。 訪問者が 7 日後（ITP 2.1）または 1 日後（ITP 2.2 および ITP 2.3）にドメインを再訪問した場合、[!DNL Target] は、期限切れの cookie の代わりに新しい [!DNL Target] cookie をドメインに配置することを余儀なくされます。 新しい [!DNL Target] Cookie は、ユーザーが同じであっても、新しい個別訪問者と解釈されます。 |
| [!DNL Target] アクティビティのルックバック期間の短縮 | [!DNL Target] アクティビティの訪問者プロファイルは、決定のためのルックバック期間が短縮した可能性があります。[!DNL Target] Cookie は、訪問者を特定するために活用され、パーソナライゼーション用にユーザープロファイル属性を格納します。Safari では、[!DNL Target] cookie が 7 日間（ITP 2.1）または 1 日（ITP 2.2 および 2.3）後に期限切れになる可能性があるので、パージされた [!DNL Target] cookie に関連付けられたユーザープロファイルデータを決定に使用することはできません。 |
| サードパーティ ID に基づくプロファイルスクリプト | 有効期限が 7 日（ITP 2.1 を使用）と 1 日（ITP 2.2 と ITP 2.3 を使用）に設定されているため、サードパーティ ID cookie に基づく [ プロファイルスクリプト ](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html) は、有効期限が切れると機能しなくなります。 |
| iOS デバイスでの QA／プレビュー URL | 有効期限は 7 日（ITP 2.1 を使用）に設定され、1 日（ITP 2.2 および ITP 2.3 を使用）であるため、URL がサードパーティ ID cookie に基づいているので、有効期限が切れると [QA/プレビュー URL](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) は機能しなくなります。 |

## 現在の [!DNL Target] の実装は影響を受けますか？

[!DNL Target] JavaScript ライブラリに加えてExperience CloudID （ECID）ライブラリを使用している場合、実装は、次の記事に示す方法で影響を受けます。[Safari ITP 2.1 はAdobe Experience CloudおよびExperience Platformのお客様への影響 ](https://medium.com/adobetech/safari-itp-2-1-impact-on-adobe-experience-cloud-customers-9439cecb55ac)。
