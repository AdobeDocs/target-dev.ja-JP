---
title: 実装パターンの概要
description: 実装パターンの使用方法の理解
feature: APIs/SDKs
level: Experienced
role: Developer
hide: true
hidefromtoc: true
source-git-commit: c4b9dfed19e5e4a56bfeae26c4310b997a2d617e
workflow-type: tm+mt
source-wordcount: '145'
ht-degree: 0%

---

# 実装パターンの概要

[!DNL Adobe Target] 実装パターンは、 [!DNL Target] 実装。

画像をクリックすると、全画面表示に拡大します。

![Adobe Targetのアーキテクチャ図](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

画像内の数字は操作の順序を示していません。

1. 用のクライアント側 SDK [!DNL Adobe Target] および [!DNL Experience Cloud ID Service]
1. [!DNL Target Delivery API] 呼び出し
1. ECID 獲得呼び出し
1. プロファイル一括更新 API および [!DNL Customer Attributes] (CA) サービス
1. 顧客のデータソースからへのプロファイルデータ取り込み [!DNL Target] プロファイルストア
1. プロファイル/行動データを収集し、エンドユーザーに表示するエクスペリエンスを決定する
1. エクスペリエンスがページ上にレンダリングされる
1. at.js によってページ上のエクスペリエンスがレンダリングされます。

各パターンは異なるパーツで構成されます。 各パーツは、 [!DNL Target] 実装。

各部分については、このガイドの別のページで説明します。 例えば、 [!DNL Target] 実装パターンには、次のページが含まれます。

* SDK を初期化
* プロファイルを強化
* エクスペリエンスをレンダリング
* 通知 [!DNL Target]

