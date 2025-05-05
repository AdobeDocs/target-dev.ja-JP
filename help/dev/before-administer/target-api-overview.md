---
title: Adobe Target API の概要
description: 配信 api、レポート api、管理 api、プロファイル api、recommendations api、postman コレクションへのリンクなど、様々なAdobe Target API の概要です。
exl-id: bf886103-36af-4061-b8be-2fe645f45ff3
feature: APIs/SDKs
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 0%

---

# Target API の概要

この記事では、管理 API とプロファイル API に固有の要件に焦点を当てる前に、様々な Target API 全般について説明します。 UI を使用して Target を管理する場合は、*Adobe Target ビジネスユーザーガイドの [ 管理*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=ja) の節を参照してください。

## API タイプ

Adobe Target API は、Adobe RecommendationsなどのAdobe Target製品を強化する API のコレクションです。 これらの API を使用すると、データの操作や統合に使用できるデータリッチなユーザーインターフェイスを作成できます。

Adobe Target API は、管理者、プロファイル、配信、レポートのタイプに従ってグループ化できます。

>[!NOTE]
>
>管理 API とプロファイル API は多くの場合、「管理 API とプロファイル API」と総称されますが、別に「管理 API」と「プロファイル API」とも呼ばれることもあります。 Recommendations API は、Target 管理 API の具体的な実装です。

| API タイプ | の機能 | ダウンロードリンク | その他の役立つリンク |
| --- | --- | --- |--- |
| [管理者](../administer/admin-api/admin-api-overview-new.md) | アクティビティ、オーディエンス、オファーおよびその他のオブジェクト（Recommendationsのエンティティ、条件、デザインなど）を作成、変更、削除する。 Recommendations API は管理 API の一種です）。 | <UL><li>[Target Admin API Postman コレクション ](https://developers.adobetarget.com/api/#admin-postman-collection)</li><li>[Recommendations API Postman コレクション ](https://developer.adobe.com/target/administer/recommendations-api/#section/Postman)</li></UL> | [Recommendations API の使用 ](../before-administer/recs-api/overview.md) |
| プロファイル | Adobe Targetに保存されているユーザープロファイルを取得および変更します。 | [Target プロファイル API Postman コレクション ](https://developers.adobetarget.com/api/#profiles) |  |
| [配信](../implement/delivery-api/overview.md) | エンドユーザーに配信するために、最適化されパーソナライズされたコンテンツを Target から取得します。 | [Target 配信 API Postman コレクション ](/help/dev/before-implement/delivery-api-overview/getting-started.md#postman) |  |
| [レポート](../administer/admin-api/admin-api-overview-new.md) | アクティビティ結果およびその他のレポート結果をエクスポートします。 | レポート API は、[Target Admin API Postman コレクション ](https://developers.adobetarget.com/api/#admin-postman-collection) に含まれています。 |  |
| [ モデル ](../administer/models-api/models-api-overview.md) | Target で機械学習モデル（「ブロックリスト」）から除外する機能のリストを管理します。 Models API は管理 API の一種ですが、UI からアクセスできないオブジェクト（ブロックリスト）に対する一意の操作なので、ここに個別に表示されます。 |  |  |

## API の違い

Target 管理 API （Recommendations API を含む）と Target 配信 API には、重要な違いがあります。

* 管理 API を使用すると、Target の様々な側面を設定でき、Target UI でも設定できます。 ただし、Models API では、UI で使用できない Target の側面を設定できます。 いずれにせよ、**すべての管理 API には認証が必要です**。

* 配信 API を使用すると、コンテンツを取得できます。 配信 API には認証は必要ありません。

Target 管理 API を使用するには、[Adobe Developer Console](https://developer.adobe.com/console/home) を使用して認証を設定する必要があります。 詳しくは、[ 認証の設定方法 ](../before-administer/configure-authentication.md) を参照してください。
