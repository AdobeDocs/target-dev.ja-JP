---
keywords: api, adobe i/o, classic, adobe developer console
description: 次のページから移行する方法を説明します： [!DNL Adobe Target Classic] への API [!DNL Target] の API [!DNL Adobe Developer Console].
title: 移行元 [!DNL Target Classic] への API [!DNL Target] の API [!DNL Adobe Developer Console]?
feature: APIs/SDKs
exl-id: b84e3767-89ad-4e2d-9bb4-7e31bffbc285
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 34%

---

# 遷移元 [!DNL Target Classic] への API [!DNL Target] の API [!DNL Adobe Developer Console]

次のページから移行する際に役立つ情報： [!DNL Target Classic] への API [!DNL Target] の API [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home).

の廃止に伴い [!DNL Adobe Target Classic]、API が [!DNL Target Classic] アカウントも使用できなくなりました。 この記事は、 [!DNL Target Classic] への API ベースの統合 [!DNL Target] を利用した API [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home).

詳しくは、 [!DNL Target] API（を参照） [[!DNL Target] API](/help/dev/before-administer/target-api-overview.md). 詳しくは、 [!DNL Target] SDK については、 [[!DNL Target] サーバー側実装](/help/dev/implement/server-side/server-side-overview.md)

## 用語

| 用語 | 説明 |
|--- |--- |
| クラシック API | リンクされている API [!DNL Target Classic] アカウント。 これらの API の呼び出しは、ユーザー名とパスワードによる認証に基づいており、ホスト名は `testandtarget.omniture.com` を使用します。API 呼び出しでリクエスト URL にユーザー名とパスワードが含まれている場合、 [!DNL Adobe Developer Console] API |
| [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home) | The [!DNL Adobe Developer Console] は、 [!DNL Target] API これらの API は、 [!DNL Target Standard/Premium] アカウント。 The [!DNL Target] の API [!DNL Adobe Developer Console] を使用する [JWT ベースの認証](../../before-administer/configure-authentication.md)：セキュアなエンタープライズ API の業界標準です。 |

## 今後の予定

次の API は、 [!DNL Target Classic] は廃止されました：

| 日付 | 詳細 |
|--- |--- |
| 2017 年 10 月 18 日 | 書き込み操作（`saveCampaign`、`copyCampaign`、`saveHTMLOfferContent`、`setCampaignState`）を実行する API メソッドがすべて廃止されました。<P>これは、すべての [!DNL Target Classic] ユーザーアカウントのステータスが読み取り専用に設定されました。 |
| 2017 年 11 月 15 日 | 残りの API が廃止されました。これは、 [!DNL Target Classic] ユーザーインターフェイスが廃止されました |

[!DNL Recommendations Classic] API はこのタイムラインの影響を受けませんでした。

## 対応するメソッド

次の表に、同等の [!DNL Adobe Developer Console] クラシック API メソッドの API メソッド。 The [!DNL Adobe Developer Console] API は JSON を返し、クラシック API は XML を返します。

The [!DNL Adobe Developer Console] API メソッドは、API ドキュメントサイトの対応する節にリンクされています。 それぞれの API メソッドのサンプルを確認できます。また、 [!DNL Target] すべてのAdobeAPI メソッドのサンプル API 呼び出しを含む管理 API Postmanコレクション [!DNL Adobe Developer Console].

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
|  | フォルダーリスト | 該当なし | ではフォルダーはサポートされていません。 [!DNL Target Standard/Premium] |
| レポート | キャンペーンのパフォーマンスレポート | [AB パフォーマンスレポートの取得](https://developers.adobetarget.com/api/#get-ab-performance-report)<P>[XT パフォーマンスレポートの取得](https://developers.adobetarget.com/api/#get-xt-performance-report) |  |
|  | 監査レポート | [監査レポートの取得](https://developers.adobetarget.com/api/#get-audit-report) |  |
|  | 1:1 コンテンツレポート | [AP パフォーマンスレポートの取得](https://developers.adobetarget.com/api/#get-ap-activity-performance-report) |  |
| アカウントの設定 | ホストグループの取得 | [環境のリスト](https://developers.adobetarget.com/api/#list-environments) |  |

## 例外

例外の適用が必要な場合は、担当のカスタマーサクセスマネージャーまでお問い合わせください。

## ヘルプ

お問い合わせください [!DNL Adobe Target Client Care] (tt-support@adobe.com) クラシック API からに移行するためのヘルプが必要な場合に、 [!DNL Target] の API [!DNL Adobe Developer Console].
