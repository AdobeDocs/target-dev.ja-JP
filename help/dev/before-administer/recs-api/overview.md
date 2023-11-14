---
title: Adobe Recommendations API とは
description: このガイドでは、Adobe Target Recommendations API を使用してRecommendationsカタログとカスタム条件の設定と管理を実践するほか、Delivery API を使用してレコメンデーションコンテンツを取得する方法について説明します。
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 0d03c650-0b00-44b8-a794-10e5d738e42c
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '339'
ht-degree: 1%

---

# Adobe Recommendations API の概要

Recommendationsに関連する API には以下が含まれます。 [管理 API](../../before-administer/target-api-overview.md) を使用して、次の操作を実行できます。

* レコメンデーション可能な製品またはコンテンツのカタログを管理
* Recommendationsのアルゴリズムとアクティビティの管理

ターゲットの使用 [配信 API](../../implement/delivery-api/overview.md) Recommendationsでは、次のこともできます。

* JSON、HTML、または XML オブジェクトでレコメンデーションを取得して、Web、モバイル、E メール、Internet of Things(IOT)、その他のチャネルで表示できるようにします。

## 説明

Recommendations API に関するこのガイドでは、Recommendations API を使用したRecommendationsカタログとカスタム条件の設定と管理、および Delivery API を使用した Recommendations コンテンツの取得に関する実践を、開発者に順を追って説明します。 最後に、次のことが可能になります。

* Recommendations API を使用したエンティティの設定と管理
* Recommendations API を使用したカスタム条件の設定と管理
* 非HTMLデバイスでのレコメンデーション結果を使用するためにRecommendationsを Delivery API と共に使用する方法を理解する

## オーディエンス

このガイドは、Target API またはRecommendations API を初めて使用する開発者を対象としています。

## 前提条件 {#prerequisites}

Target 管理 API にはが必要です。 [Adobe認証設定](../configure-authentication.md). Recommendations API を使用する前に、この設定をおこなっていることを確認してください。

## リソース

このガイドを理解し、ガイドに従うために必要な、以下のリソースに注意してください。

| リソース | 詳細 |
| --- | --- |
| Postman | を取得します [Postmanアプリ](https://www.postman.com/downloads/) ご使用のオペレーティングシステム用。 Postman basic は、アカウントの作成に自由に対応しています。 Adobe Target API を一般に使用するためには必須ではありませんが、Postmanでは API ワークフローが容易になります。Adobe Targetでは、API の実行とその動作方法の学習に役立つ、いくつかのPostmanコレクションが提供されています。 このガイドの残りの部分は、Postmanの作業に関する知識を前提としています。 不明な点は、 [Postmanドキュメント](https://learning.getpostman.com/). |
| 参照 | このガイドの残りの部分では、次のリソースに関する知識が前提となります。<UL><li>[Adobe I/OGithub](https://github.com/adobeio)</li><li>[Target 管理およびプロファイル API のドキュメント](../../administer/admin-api/admin-api-overview-new.md)</li><li>[Recommendations API ドキュメント](https://developer.adobe.com/target/administer/recommendations-api/)</li></UL> |
