---
title: Adobe Target APIの概要
description: 配信api、レポート api、管理api、プロファイル api、レコメンデーション api、postman コレクションへのリンクなど、様々なAdobe Target APIの概要。
exl-id: bf886103-36af-4061-b8be-2fe645f45ff3
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/GbrWhrZxH-sTtpxotpJGbr-sHuIXrX7rZFQhju76-vM
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: adee20bd-51f4-461d-b9db-d215f8756eebid: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 448
ht-degree: 0%

---

# Target APIの概要

この記事では、Admin APIとProfile APIに固有の要件に焦点を当てる前に、一般的に様々なTarget APIについて説明します。 UIを使用してAdobe Targetを管理する場合は、*Adobe Target Business ユーザーガイド*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=en)の[管理セクションを参照してください。

## API タイプ

Adobe Target APIは、Adobe RecommendationsなどのAdobe Target製品を強化するAPIのコレクションです。 これらのAPIを使用すると、データを操作および統合するために使用できる、豊富なデータを備えたユーザーインターフェイスを作成できます。

Adobe Target APIは、管理者、プロファイル、配信、レポートのタイプに応じてグループ化できます。

>[!NOTE]
>
>Admin APIとProfile APIは、多くの場合、まとめて参照されますが（「Admin and Profile API」）、個別に参照することもできます（「Admin API」と「Profile API」）。 Recommendations APIは、Target Admin APIの特定の実装です。

| API タイプ | 顧客の期待に応える体験を | ダウンロードリンク | その他の役立つリンク |
| --- | --- | --- |--- |
| [管理者](../administer/admin-api/admin-api-overview-new.md) | アクティビティ、オーディエンス、オファー、その他のオブジェクト（Recommendations エンティティ、条件、デザインなど）を作成、変更、削除します。 Recommendations APIは、Admin APIの一種です）。 | <UL><li>[Target Admin API Postman Collection](https://developers.adobetarget.com/api/#admin-postman-collection)</li><li>[Recommendations API Postman Collection](https://developer.adobe.com/target/administer/recommendations-api/#section/Postman)</li></UL> | [Recommendations APIを使用](../before-administer/recs-api/overview.md) |
| プロファイル | Adobe Targetに保存されているユーザープロファイルを取得して変更します。 | [Target Profile API Postman Collection](https://developers.adobetarget.com/api/#profiles) |  |
| [配信](../implement/delivery-api/overview.md) | エンドユーザーへの配信のために、Targetから最適化およびパーソナライズされたコンテンツを取得します。 | [Target Delivery API Postman Collection](/help/dev/before-implement/delivery-api-overview/getting-started.md#postman) |  |
| [レポート](../administer/admin-api/admin-api-overview-new.md) | アクティビティの結果やその他のレポート結果の書き出し： | レポート APIは、[Target Admin API Postman コレクション ](https://developers.adobetarget.com/api/#admin-postman-collection)に含まれています。 |  |
| [ モデル ](../administer/models-api/models-api-overview.md) | Targetでマシンラーニングモデルから除外する機能のリストを管理します（「ブロックリスト」）。 Models APIはAdmin APIの一種ですが、UIを介してアクセスできないオブジェクト（ヘルプブロックリスト）に対する一意の操作のため、ここに個別にリストされています。 |  |  |

## APIの違い

Target管理API （Recommendations APIを含む）とTarget配信APIには重要な違いがあります。

* Admin APIを使用すると、Targetの様々な側面を設定し、Target UIでも設定できます。 これに対する例外は、UIで使用できないTargetの側面を設定できるModels APIです。 関係なく、**すべての管理者APIには認証が必要です。**

* 配信APIにより、コンテンツを取得できます。 配信APIは認証を必要としません。

Target Admin APIを使用するには、[Adobe Developer Console](https://developer.adobe.com/console/home)を使用して認証を設定する必要があります。 詳しくは、[認証の設定方法](../before-administer/configure-authentication.md)を参照してください。
