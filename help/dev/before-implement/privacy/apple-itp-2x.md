---
keywords: apple, ITP，インテリジェントトラッキング防止， experience cloud id, ecid, itp
description: 詳細 [!DNL Adobe Target] Safari ユーザーのプライバシーを保護しようとするApple Intelligent Tracking Prevention(ITP) イニシアチブの影響について説明します。
title: 方法 [!DNL Target] Apple ITP のサポートを処理しますか？
feature: Privacy & Security
exl-id: 6deee03b-df86-4d0d-999c-b11855ddfda5
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '604'
ht-degree: 32%

---

# Apple Intelligent Tracking Prevention（ITP）2.x

Intelligent Tracking Prevention(ITP) は、Safari ユーザーのプライバシーを保護するためのAppleの取り組みです。 ITP の最初のリリース（2017 年）では、サードパーティ Cookie の使用を対象としていました。実際、Apple は、サードパーティ Cookie を完全にブロックしましたが、サードパーティ Cookie は、訪問者の追跡および訪問者データの収集に一般的に使用されていたので、広告技術およびマーケティング技術会社にとって深刻な頭痛の種となりました。現在、Apple は、Safari 内でのファーストパーティ Cookie の使用方法に制限や制約を課すことに移行しています。

ITP のこれらのバージョンには、次の制限が含まれています。

| バージョン | 詳細 |
| --- | --- |
| [ITP 2.1](https://webkit.org/blog/8613/intelligent-tracking-prevention-2-1/) | `document.cookie` API を使用してブラウザーに配置されたクライアント側 Cookie の有効期限の上限は 7 日間になりました。<br />2019 年 2 月 21 日にリリースされました。 |
| [ITP 2.2](https://webkit.org/blog/8828/intelligent-tracking-prevention-2-2/) | 7 日間の有効期限の上限が 1 日に大幅に短縮されました。<br />2019 年 4 月 24 日にリリースされました。 |
| [ITP 2.3](https://webkit.org/blog/9521/intelligent-tracking-prevention-3-2/) | localStorage の使用や JavaScript の使用など、いくつかの回避策を排除 `Document.referrer property`.<br />2019 年 9 月 23 日にリリースされました。<br />Safari 14、macOS Big Sur、Catalina、Mojave、iOS 14 および iPadOS 14 でリリースされた、ITP に対する CNAME クローキングの防御機能。 サードパーティの CNAME クロークされた HTTP 応答で作成されたすべての cookie は、7 日後に期限切れに設定されます。<br />2020 年 11 月 12 日に発表。 |

## Adobe As a にとっての影響 [!DNL Target] お客様？

Target は、をページにデプロイするための JavaScript ライブラリを提供し、 [!DNL Target] は、訪問者に対してリアルタイムのパーソナライゼーションを提供します。 3 つあります [!DNL Target] JavaScript ライブラリ at.js 1.*x*、at.js 2.*x*、 [!DNL Adobe Experience Cloud Web SDK] クライアント側の [!DNL Target] を介して訪問者のブラウザーに表示される cookie `document.cookie` API. その結果、 [!DNL Target] cookie は、Appleの ITP 2.1、2.2 および 2.3 の影響を受け、7 日後 (ITP 2.1) または 1 日後（ITP 2.2 および ITP 2.3）に期限が切れます。

Apple ITP 2.x の影響 [!DNL Target] 次の領域では、

| 影響 | 詳細 |
| --- | --- |
| 個別訪問者数が増加する可能性 | 有効期限が 7 日間 (ITP 2.1) および 1 日間（ITP 2.2 および ITP 2.3）に設定されているので、Safari ブラウザーからの個別訪問者が増加している可能性があります。 訪問者が 7 日後 (ITP 2.1) または 1 日後（ITP 2.2 および ITP 2.3）にドメインに再訪問した場合、 [!DNL Target] は、新しい [!DNL Target] cookie の有効期限が切れた Cookie の代わりに、ドメイン上で cookie を使用するように設定します。 新しい [!DNL Target] Cookie は、ユーザーが同じであっても、新しい個別訪問者と解釈されます。 |
| [!DNL Target] アクティビティのルックバック期間の短縮 | [!DNL Target] アクティビティの訪問者プロファイルは、決定のためのルックバック期間が短縮した可能性があります。[!DNL Target] Cookie は、訪問者を特定するために活用され、パーソナライゼーション用にユーザープロファイル属性を格納します。それを考えれば [!DNL Target] Cookie は、Safari 上で 7 日後 (ITP 2.1) または 1 日後（ITP 2.2 および 2.3）に期限切れになる場合があります（ITP 2.3 および 2.3）。パージされた [!DNL Target] cookie は判定には使用できません。 |
| サードパーティ ID に基づくプロファイルスクリプト | 有効期限が 7 日間 (ITP 2.1) および 1 日間（ITP 2.2 および ITP 2.3）に設定されていることにより、 [プロファイルスクリプト](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html) 3rdPartyID cookie に基づく Cookie は、有効期限が切れると機能しなくなります。 |
| iOS デバイスでの QA／プレビュー URL | 有効期限が 7 日間 (ITP 2.1) および 1 日間（ITP 2.2 および ITP 2.3）に設定されていることにより、 [QA/プレビュー URL](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html) URL が 3rdPartyID cookie に基づいているので、有効期限が切れると、の機能が停止します。 |

## 現在の [!DNL Target] の実装は影響を受けますか？

Experience CloudID(ECID) ライブラリを使用している場合は、 [!DNL Target] JavaScript ライブラリの場合、実装はこの記事に示す方法の影響を受けます。 [Safari ITP 2.1 がAdobe Experience CloudおよびExperience Platformのお客様に与える影響](https://medium.com/adobetech/safari-itp-2-1-impact-on-adobe-experience-cloud-customers-9439cecb55ac).
