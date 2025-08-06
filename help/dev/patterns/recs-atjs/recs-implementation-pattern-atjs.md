---
title: at.js を使用した Recommendations 実装パターン
description: at.js を使用した Recommendations の実装パターンの使用方法について
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: d568cd1d-acc3-42e0-ae2c-5787e6f361f8
source-git-commit: 3b0bc0b67800ed4b1da6ba2bfa05c677147a78ba
workflow-type: tm+mt
source-wordcount: '151'
ht-degree: 0%

---

# at.js を使用した [!DNL Recommendations] 実装パターンの概要

この実装パターンは、[!DNL Adobe Target Recommendations]at.js JavaScript library[ を使用する際に ](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md) 実装を理解し、作成するのに役立ちます。

「画像」をクリックすると、フルスクリーンに展開できます。

![Adobe Targetのアーキテクチャ図 ](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

画像内の数値は、操作のシーケンスを示すものではありません。

1. [!DNL Adobe Target] および [!DNL Experience Cloud ID Service] 用のクライアントサイド SDK
1. [!DNL Target Delivery API] 呼び出し
1. [!UICONTROL Experience Cloud ID] （ECID）獲得コール
1. プロファイル一括更新 API および [!DNL Customer Attributes] （CA）サービス
1. 顧客のデータソースからプロファイルストアへのプロファイルデータ [!DNL Target] 取り込み
1. プロファイルと行動データを収集し、訪問者に表示するエクスペリエンスを決定する
1. エクスペリエンスがページでレンダリングされる
1. at.js は、ページ上のエクスペリエンスをレンダリングします

各パターンは、様 [!DNL Target] な実装の重要な実装要件に対応する様々なパーツで構成されます。

このガイドの各パートについては、個別の記事で説明します。

* [SDK の初期化](/help/dev/patterns/recs-atjs/initialize-sdk.md)
* [データ収集の設定](/help/dev/patterns/recs-atjs/data-collection.md)
* [エクスペリエンスのレンダリング](/help/dev/patterns/recs-atjs/render-experiences.md)
* [Notify Target](/help/dev/patterns/recs-atjs/notify-target.md)
