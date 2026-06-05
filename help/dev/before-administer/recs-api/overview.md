---
title: Adobe Recommendations APIとは何ですか？
description: このガイドでは、Adobe Target Recommendations APIを使用して、Recommendations カタログとカスタム条件を設定および管理し、Delivery APIを使用してレコメンデーションコンテンツを取得する実践的な方法について、開発者に説明します。
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 0d03c650-0b00-44b8-a794-10e5d738e42c
TQID: https://experienceleague.adobe.com/-bWsxWNZK7LXp0VvKZmsZc68jXcit57v7Wki9hR3wH4
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 343
ht-degree: 3%

---

# Adobe Recommendations APIの概要

Recommendationsに関連するAPIには、[管理者API](../../before-administer/target-api-overview.md)が含まれており、次のことが可能です。

* 商品カタログやコンテンツのレコメンデーションを管理
* レコメンデーションのアルゴリズムとアクティビティの管理

RecommendationsでTarget [配信API](../../implement/delivery-api/overview.md)を使用すると、次の操作も実行できます。

* レコメンデーションをJSON、HTML、XML オブジェクトで取得し、web、モバイル、電子メール、IOT （モノのインターネット）などのチャネルで表示できます。

## 説明

Recommendations APIに関するこのガイドでは、Recommendations APIを使用してRecommendations カタログとカスタム条件を設定および管理し、Delivery APIを使用してレコメンデーションコンテンツを取得する実習を開発者に説明します。 最終的には、次のことが可能になります。

* Recommendations APIを使用したエンティティの設定と管理
* Recommendations APIを使用したカスタム条件の設定と管理
* HTML以外のデバイスでRecommendations APIを使用してRecommendationsの結果を使用する方法について説明します

## オーディエンス

このガイドは、Target APIまたはRecommendations APIを初めて使用する開発者を対象としています。

## 前提条件 {#prerequisites}

Target管理APIには[Adobe認証の設定](../configure-authentication.md)が必要です。 Recommendations APIを使用する前に、これが設定されていることを確認してください。

## リソース

このガイドを理解し、それに従うために必要な次のリソースに注意してください。

| リソース | 詳細 |
| --- | --- |
| Postman | お使いのオペレーティング システム用の[Postman アプリ &#x200B;](https://www.postman.com/downloads/)を入手します。 Postman basicはアカウント作成機能を無料で利用できます。 Adobe Target APIを一般的に使用する場合は不要ですが、PostmanではAPI ワークフローが簡単になり、Adobe TargetにはAPIの実行と動作の学習に役立つPostman コレクションがいくつか用意されています。 このガイドの残りの部分では、Postmanに関する実務的な知識を前提としています。 サポートが必要な場合は、[Postman ドキュメント &#x200B;](https://learning.getpostman.com/)を参照してください。 |
| 参照 | このガイドの残りの部分では、次のリソースについて理解していることを前提としています。<UL><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[Target管理者とプロファイル API ドキュメント &#x200B;](../../administer/admin-api/admin-api-overview-new.md)</li><li>[Recommendations API ドキュメント &#x200B;](https://developer.adobe.com/target/administer/recommendations-api/)</li></UL> |
