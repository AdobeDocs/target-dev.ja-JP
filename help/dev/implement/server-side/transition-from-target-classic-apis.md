---
keywords: api、adobe i/o、classic、adobe developer console
description: ' [!DNL Adobe Target Classic] APIから [!DNL Adobe Developer Console]の [!DNL Target] APIに移行する方法について説明します。'
title: ' [!DNL Adobe Developer Console]で [!DNL Target Classic] APIから [!DNL Target] APIに移行するにはどうすればよいですか？'
feature: APIs/SDKs
exl-id: b84e3767-89ad-4e2d-9bb4-7e31bffbc285
TQID: https://experienceleague.adobe.com/cIWcraU0O9Ut1VBbD5ScKOyBrXniyIEM5XEVZMJvffk
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ceid: f7c7de77-382f-4f48-8b36-61a170f06d3d
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 595
ht-degree: 32%

---

# [!DNL Target Classic]個のAPIから[!DNL Adobe Developer Console]個の[!DNL Target]個のAPIに移行

[!DNL Target Classic] APIから[[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home)の[!DNL Target] APIへの移行に役立つ情報です。

[!DNL Adobe Target Classic]の廃止に伴い、[!DNL Target Classic] アカウントに接続されているAPIも使用できなくなります。 この記事では、[!DNL Target Classic] API ベースの統合を[[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home)によって提供される[!DNL Target] APIに移行する際に役立ちます。

[!DNL Target]個のAPIについて詳しくは、[[!DNL Target] 個のAPI](/help/dev/before-administer/target-api-overview.md)を参照してください。 [!DNL Target] SDKについて詳しくは、[[!DNL Target]  サーバーサイド実装](/help/dev/implement/server-side/server-side-overview.md)を参照してください

## 用語

| 用語 | 説明 |
|--- |--- |
| Classic API | [!DNL Target Classic] アカウントにリンクされているAPI。 これらの API の呼び出しは、ユーザー名とパスワードによる認証に基づいており、ホスト名は `testandtarget.omniture.com` を使用します。 API呼び出しにリクエスト URLにユーザー名とパスワードが含まれている場合は、[!DNL Adobe Developer Console] APIに移行する必要があります。 |
| [[!DNL Adobe Developer Console]](https://developer.adobe.com/console/home) | [!DNL Adobe Developer Console]は[!DNL Target] APIのゲートウェイです。 これらのAPIは[!DNL Target Standard/Premium] アカウントに接続されています。 [!DNL Adobe Developer Console]の[!DNL Target] APIでは、セキュアなエンタープライズ APIの業界標準である[JWT ベースの認証](../../before-administer/configure-authentication.md)を使用しています。 |

## タイムライン

次のAPIは、[!DNL Target Classic]が廃止されたときに廃止されました。

| 日付 | 詳細 |
|--- |--- |
| 2017 年 10 月 18 日 | 書き込み操作（`saveCampaign`、`copyCampaign`、`saveHTMLOfferContent`、`setCampaignState`）を実行する API メソッドがすべて廃止されました。<P>これは、すべての[!DNL Target Classic] ユーザーアカウントが読み取り専用ステータスに設定された日付と同じです。 |
| 2017 年 11 月 15 日 | 残りの API が廃止されました。 [!DNL Target Classic] ユーザーインターフェイスが廃止された日付です |

[!DNL Recommendations Classic]個のAPIがこのタイムラインの影響を受けませんでした。

## 対応するメソッド

次の表に、Classic API メソッドに対応する[!DNL Adobe Developer Console] API メソッドを示します。 [!DNL Adobe Developer Console] APIはJSONを返しますが、Classic APIはXMLを返します。

[!DNL Adobe Developer Console] API メソッドは、API ドキュメントサイトの対応するセクションにリンクされています。 それぞれの API メソッドのサンプルを確認できます。 [!DNL Adobe Developer Console]のすべてのPostman API メソッドのサンプル API呼び出しを含む[!DNL Target]管理API Adobe コレクションを使用することもできます。

| グループ | Classic API メソッド | [!DNL Adobe Developer Console] API メソッド | メモ |
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
|  | フォルダーリスト | 該当なし | フォルダーは[!DNL Target Standard/Premium]ではサポートされていません |
| レポート | キャンペーンのパフォーマンスレポート | [AB パフォーマンスレポートの取得](https://developers.adobetarget.com/api/#get-ab-performance-report)<P>[XT パフォーマンスレポートの取得](https://developers.adobetarget.com/api/#get-xt-performance-report) |  |
|  | 監査レポート | [監査レポートを取得](https://developers.adobetarget.com/api/#get-audit-report) |  |
|  | 1:1 コンテンツレポート | [AP パフォーマンスレポートの取得](https://developers.adobetarget.com/api/#get-ap-activity-performance-report) |  |
| アカウントの設定 | ホストグループの取得 | [環境のリスト](https://developers.adobetarget.com/api/#list-environments) |  |

## 例外

例外の適用が必要な場合は、担当のカスタマーサクセスマネージャーまでお問い合わせください。

## ヘルプ

ご不明な点がある場合や、Classic APIから[!DNL Adobe Developer Console]の[!DNL Target] APIへの移行に関するサポートが必要な場合は、[!DNL Adobe Target Client Care] （tt-support@adobe.com）にお問い合わせください。
