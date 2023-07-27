---
title: Adobe Target API の概要
description: 様々なAdobe Target API の概要（delivery api、reporting api、admin api、profile api、recommendations api、postman コレクションへのリンクを含む）。
exl-id: bf886103-36af-4061-b8be-2fe645f45ff3
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '439'
ht-degree: 1%

---

# Target API の概要

この記事では、管理 API とプロファイル API に固有の要件に重点を置く前に、一般的に様々な Target API について説明します。 UI を使用して Target を管理する場合は、 [管理セクション *Adobe Target Business ユーザーガイド*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=en).

## API タイプ

Adobe Target API は、Adobe RecommendationsなどAdobe Target製品を強化する API のコレクションです。 これらの API を使用すると、データの操作と統合に使用できる、データに富んだユーザーインターフェイスを作成できます。

Adobe Target API は、管理者、プロファイル、配信、レポートの各タイプに従ってグループ化できます。

>[!NOTE]
>
>Admin API とプロファイル API は、多くの場合、まとめて（「管理 API」と「プロファイル API」）と呼ばれますが、別々に（「管理 API」と「プロファイル API」）と呼ぶこともできます。 Recommendations API は、Target 管理 API の具体的な実装です。

| API タイプ | 何が可能か | ダウンロードリンク | その他の役立つリンク |
| --- | --- | --- |--- |
| [管理者](../administer/admin-api/admin-api-overview-new.md) | アクティビティ、オーディエンス、オファーおよびその他のオブジェクト (Recommendationsのエンティティ、条件、デザインなど ) を作成、変更および削除します。 Recommendations API は管理 API の一種です )。 | <UL><li>[Target 管理 API Postmanコレクション](https://developers.adobetarget.com/api/#admin-postman-collection)</li><li>[Recommendations API Postmanコレクション](https://developers.adobetarget.com/api/recommendations/#section/Postman)</li></UL> | [Recommendations API の使用](../before-administer/recs-api/overview.md) |
| プロファイル | Adobe Targetに保存されているユーザープロファイルを取得して変更します。 | [Target プロファイル API Postman Collection](https://developers.adobetarget.com/api/#profiles) |  |
| [配信](../implement/delivery-api/overview.md) | エンドユーザーに配信するために、最適化され、パーソナライズされたコンテンツを Target から取得します。 | [Target 配信 API Postmanコレクション](/help/dev/before-implement/delivery-api-overview/getting-started.md#postman) |  |
| [レポート](../administer/admin-api/admin-api-overview-new.md) | アクティビティの結果と他のレポート結果をエクスポートします。 | レポート API は、 [Target 管理 API Postmanコレクション](https://developers.adobetarget.com/api/#admin-postman-collection). |  |
| [モデル](../administer/models-api/models-api-overview.md) | Target で機械学習モデルから除外する機能のリスト (「ブロックリストに加える」) を管理します。 モデル API は Admin API の一種ですが、UI からアクセスできないオブジェクト () に対する独自の操作が原因で、個別にここに表示されまブロックリストに加えるす。 |  |  |

## API の違い

Target Admin API(Recommendations API を含む ) と Target Delivery API には、次のような重要な違いがあります。

* 管理 API を使用すると、Target の様々な側面を設定でき、Target UI でも設定できます。 ただし、モデル API は例外で、UI で使用できない Target の側面を設定できます。 関係なく、 **すべての管理 API に認証が必要です。**

* Delivery API を使用すると、コンテンツを取得できます。 配信 API には認証は必要ありません。

Target Admin API を使用するには、 [Adobe Developer Console](https://developer.adobe.com/console/home). 詳しくは、 [認証の設定方法](../before-administer/configure-authentication.md).
