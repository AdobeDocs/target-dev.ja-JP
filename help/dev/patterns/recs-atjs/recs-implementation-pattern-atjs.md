---
title: at.js を使用したRecommendations実装パターン
description: at.js でRecommendationsの実装パターンを使用する方法を説明します。
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: 752c52c0db5173f49fd828c297fa7afd7c53c6ce
workflow-type: tm+mt
source-wordcount: '154'
ht-degree: 0%

---

# [!DNL Recommendations] at.js を使用した実装パターンの概要

この実装パターンは、 [!DNL Adobe Target Recommendations] を使用する場合の実装 [at.js JavaScript ライブラリ](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md).

画像をクリックすると、全画面表示に拡大します。

![Adobe Targetのアーキテクチャ図](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

画像内の数字は操作の順序を示していません。

1. 用のクライアント側 SDK [!DNL Adobe Target] および [!DNL Experience Cloud ID Service]
1. [!DNL Target Delivery API] 呼び出し
1. [!UICONTROL Experience CloudID] (ECID) 獲得呼び出し
1. プロファイル一括更新 API および [!DNL Customer Attributes] (CA) サービス
1. 顧客のデータソースからへのプロファイルデータ取り込み [!DNL Target] プロファイルストア
1. プロファイルと行動データの収集と、訪問者に表示するエクスペリエンスの決定
1. エクスペリエンスがページ上にレンダリングされる
1. at.js によってページ上のエクスペリエンスがレンダリングされます。

各パターンは異なるパーツで構成され、各パーツはお客様の重要な実装要件に対応しています。 [!DNL Target] 実装。

各パーツについては、このガイドの別の記事で説明します。

* [SDK を初期化](/help/dev/patterns/recs-atjs/initialize-sdk.md)
* [データ収集の設定](/help/dev/patterns/recs-atjs/data-collection.md)
* [エクスペリエンスをレンダリング](/help/dev/patterns/recs-atjs/render-experiences.md)
* [ターゲットに通知](/help/dev/patterns/recs-atjs/notify-target.md)

