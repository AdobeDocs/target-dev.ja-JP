---
title: 実装パターンの概要
description: 実装パターンの使用方法の理解
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: e15513f5c52240536ccf41f16ba7f4dc6dbf9a04
workflow-type: tm+mt
source-wordcount: '152'
ht-degree: 0%

---

# 実装パターンの概要

[!DNL Adobe Target] 実装パターンは、 [!DNL Target] 実装。

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

各パーツについては、このガイドの別のトピックで説明します。 例えば、 [[!DNL Recommendations] at.js を使用した実装パターン](/help/dev/patterns/recs-atjs/recs-implementation-pattern-atjs.md) には、次のトピックが含まれています。

* SDK を初期化
* データ収集の設定
* エクスペリエンスをレンダリング
* 通知 [!DNL Target]

