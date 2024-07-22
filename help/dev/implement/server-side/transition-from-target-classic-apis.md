---
keywords: api、adobe i/o、classic、adobe developer console
description: ' [!DNL Adobe Developer Console] で  [!DNL Adobe Target Classic] API から  [!DNL Target] API に移行する方法を説明します。'
title: ' [!DNL Adobe Developer Console] で API を  [!DNL Target Classic] API から  [!DNL Target] API に移行する方法'
feature: APIs/SDKs
exl-id: b84e3767-89ad-4e2d-9bb4-7e31bffbc285
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '493'
ht-degree: 38%

---

# [!DNL Target Classic] API から [!DNL Adobe Developer Console] 上の [!DNL Target] API への移行

[[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home) 上で [!DNL Target Classic] API から [!DNL Target] API への移行に役立つ情報です。

[!DNL Adobe Target Classic] の廃止措置により、[!DNL Target Classic] アカウントに接続されている API も使用できなくなりました。 この記事は、[!DNL Target Classic] API ベースの統合を、[[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home) を使用した [!DNL Target] API に移行する際に役立ちます。

[!DNL Target] API について詳しくは、[[!DNL Target] API](/help/dev/before-administer/target-api-overview.md) を参照してください。 [!DNL Target] SDK について詳しくは、[[!DNL Target]  サーバーサイド実装 ](/help/dev/implement/server-side/server-side-overview.md) を参照してください

## 用語

| 用語 | 説明 |
|--- |--- |
| クラシック API | [!DNL Target Classic] アカウントにリンクされている API。 これらの API の呼び出しは、ユーザー名とパスワードによる認証に基づいており、ホスト名は `testandtarget.omniture.com` を使用します。API 呼び出しにリクエスト URL にユーザー名とパスワードが含まれている場合は、[!DNL Adobe Developer Console] の API に移行する必要があります。 |
| [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home) | [!DNL Adobe Developer Console] は、[!DNL Target] API のゲートウェイです。 これらの API は [!DNL Target Standard/Premium] アカウントに接続されています。 [!DNL Adobe Developer Console] 上の [!DNL Target] API は、安全なエンタープライズ API の業界標準である [JWT ベースの認証 ](../../before-administer/configure-authentication.md) を使用しています。 |

## 今後の予定

[!DNL Target Classic] が廃止されると、次の API は廃止されました。

| 日付 | 詳細 |
|--- |--- |
| 2017 年 10 月 18 日 | 書き込み操作（`saveCampaign`、`copyCampaign`、`saveHTMLOfferContent`、`setCampaignState`）を実行する API メソッドがすべて廃止されました。<P>これは、すべてのユーザーアカウント [!DNL Target Classic] 読み取り専用ステータスに設定された日付と同じです。 |
| 2017 年 11 月 15 日 | 残りの API が廃止されました。[!DNL Target Classic] ユーザーインターフェイスが廃止された日付です |

[!DNL Recommendations Classic] API は、このタイムラインの影響を受けませんでした。

## 対応するメソッド

次の表に、クラシック API メソッドに対応する [!DNL Adobe Developer Console] API メソッドを示します。 [!DNL Adobe Developer Console] API は JSON を返し、Classic API は XML を返します。

[!DNL Adobe Developer Console] の API メソッドは、API ドキュメントサイトの対応するセクションにリンクされています。 それぞれの API メソッドのサンプルを確認できます。また、[!DNL Adobe Developer Console] 上のすべてのAdobeAPI メソッドのサンプル API 呼び出しを含む [!DNL Target] Admin API Postman コレクションを使用することもできます。

| グループ | クラシック API メソッド | [!DNL Adobe Developer Console] API メソッド | メモ |
|--- |--- |--- |--- |
| キャンペーン／アクティビティ | キャンペーンの作成 | [AB アクティビティの作成](https://developers.adobetarget.com/api/#create-ab-activity)<P>[XT アクティビティの作成](https://developers.adobetarget.com/api/#create-xt-activity) | 新しい API では、AB と XT で別々の作成メソッドを使用します。 |
|  | キャンペーンの更新 | [AB アクティビティの更新](https://developers.adobetarget.com/api/#update-ab-activity)<P>[XT アクティビティの更新](https://developers.adobetarget.com/api/#update-xt-activity) |  |
|  | キャンペーンのコピー | 該当なし | アクティビティの作成 API を使用します。 |
|  | キャンペーンリスト | [アクティビティのリスト](https://developers.adobetarget.com/api/#list-activities) |  |
|  | キャンペーン状態 | [アクティビティ状態の更新](https://developers.adobetarget.com/api/#update-activity-state) |  |
|  | キャンペーン表示 | [ID での AB アクティビティの取得](https://developers.adobetarget.com/api/#get-ab-activity-by-id)<P>[ID での XT アクティビティの取得](https://developers.adobetarget.com/api/#get-xt-activity-by-id) |  |
|  | サードパーティキャンペーン ID | 該当なし | thirdpartyID を使用する場合は、該当のアクティビティメソッドを使用できます。 |
| オファー | オファー作成 | [オファーの作成](https://developers.adobetarget.com/api/#create-offer) |  |
|  | オファー取得 | [ID でのオファーの取得](https://developers.adobetarget.com/api/#get-offer-by-id) |  |
|  | オファーリスト | [オファーのリスト](https://developers.adobetarget.com/api/#list-offers) |  |
|  | フォルダーリスト | 該当なし | [!DNL Target Standard/Premium] ではフォルダーはサポートされていません |
| レポート | キャンペーンのパフォーマンスレポート | [AB パフォーマンスレポートの取得](https://developers.adobetarget.com/api/#get-ab-performance-report)<P>[XT パフォーマンスレポートの取得](https://developers.adobetarget.com/api/#get-xt-performance-report) |  |
|  | 監査レポート | [ 監査レポートの取得 ](https://developers.adobetarget.com/api/#get-audit-report) |  |
|  | 1:1 コンテンツレポート | [AP パフォーマンスレポートの取得](https://developers.adobetarget.com/api/#get-ap-activity-performance-report) |  |
| アカウントの設定 | ホストグループの取得 | [環境のリスト](https://developers.adobetarget.com/api/#list-environments) |  |

## 例外

例外の適用が必要な場合は、担当のカスタマーサクセスマネージャーまでお問い合わせください。

## ヘルプ

質問がある場合や、[!DNL Adobe Developer Console] 上でクラシック API から [!DNL Target] API に移行する際にサポートが必要な場合は、[!DNL Adobe Target Client Care] （tt-support@adobe.com）にお問い合わせください。
