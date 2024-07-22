---
title: Adobe Recommendations API とは
description: このガイドでは、Adobe Target Recommendations API を使用したRecommendations カタログとカスタム条件の設定と管理および配信 API を使用したレコメンデーションコンテンツの取得に関する実践について開発者に順を追って説明します。
feature: APIs/SDKs, Recommendations, Administration & Configuration, Overview
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: 0d03c650-0b00-44b8-a794-10e5d738e42c
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 2%

---

# Adobe Recommendations API の概要

Recommendationsに関連する API には、次の操作を可能にする [ 管理 API](../../before-administer/target-api-overview.md) が含まれています。

* お勧めの製品やコンテンツのカタログを管理する
* Recommendationsのアルゴリズムとアクティビティの管理

Target [ 配信 API](../../implement/delivery-api/overview.md) をRecommendationsと併用すると、次の操作も可能です。

* Web、モバイル、メール、Internet of Things （IOT）などのチャネルに表示できるように、レコメンデーションを JSON、HTML、XML オブジェクトで取得します。

## 説明

Recommendations API に関するこのガイドでは、Recommendations API を使用してRecommendations カタログとカスタム条件を設定および管理したり、配信 API を使用してレコメンデーションコンテンツを取得したりするための実践的なプラクティスについて説明します。 最終的には、次のことが可能になります。

* Recommendations API を使用したエンティティの設定と管理
* Recommendations API を使用したカスタム条件の設定と管理
* Recommendationsと配信 API を使用して、レコメンデーションの結果をHTML以外のデバイスで使用する方法を説明します

## オーディエンス

このガイドは、Target API またはRecommendations API を初めて使用する開発者向けです。

## 前提条件 {#prerequisites}

Target 管理 API には、[Adobe認証の設定 ](../configure-authentication.md) が必要です。 Recommendations API を使用する前に、これが設定されていることを確認してください。

## リソース

このガイドを理解して正しく実行するために必要な、次のリソースに注意してください。

| リソース | 詳細 |
| --- | --- |
| Postman | お使いのオペレーティングシステム用の ](https://www.postman.com/downloads/)0}Postman アプリ } を入手します。 [Postman basic は無料でアカウントを作成できます。 Adobe Target API を一般的に使用するために必要なものではありませんが、Postmanを使用すると API ワークフローが容易になり、Adobe Targetには API の実行と操作方法の学習に役立つPostman コレクションが複数用意されています。 このガイドの残りの部分では、Postmanに関する実務知識を前提としています。 サポートについては、[Postmanのドキュメント ](https://learning.getpostman.com/) を参照してください。 |
| 参照 | このガイドの残りの部分では、次のリソースに慣れていることを前提としています。<UL><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[Target 管理者およびプロファイル API のドキュメント ](../../administer/admin-api/admin-api-overview-new.md)</li><li>[Recommendations API ドキュメント ](https://developer.adobe.com/target/administer/recommendations-api/)</li></UL> |
