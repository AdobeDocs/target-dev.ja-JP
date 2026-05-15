---
title: at.jsを使用したレコメンデーションの実装パターン
description: at.jsを使用したRecommendationsの実装パターンの使用方法について説明します
feature: APIs/SDKs
level: Experienced
role: Developer
exl-id: d568cd1d-acc3-42e0-ae2c-5787e6f361f8
TQID: https://experienceleague.adobe.com/uYNa5mL-8ffPS-ddjnQzILnPFMbjNJqKZDmQT8qFeGA
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c4147b6e-073b-4d3c-9ab1-d60f2f4434efid: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 152
ht-degree: 0%

---

# at.jsの概要を使用した[!DNL Recommendations]実装パターン

この実装パターンは、[at.js JavaScript ライブラリ ](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)を使用する際に、[!DNL Adobe Target Recommendations]実装を理解し、作成するのに役立ちます。

「画像」をクリックしてフルスクリーンに展開します。

![Adobe Target アーキテクチャ ダイアグラム ](/help/dev/patterns/assets/architecture-chart.png){width="600" zoomable="yes"}

画像内の数値は、操作の順序を示していないことに注意してください。

1. [!DNL Adobe Target]および[!DNL Experience Cloud ID Service]のクライアントサイド SDK
1. [!DNL Target Delivery API]件の呼び出し
1. [!UICONTROL Experience Cloud ID] （ECID）取得コール
1. プロファイルの一括更新APIと[!DNL Customer Attributes] （CA） サービス
1. 顧客のデータソースから[!DNL Target] プロファイルストアへのプロファイルデータの取り込み
1. プロファイルや行動データを収集し、訪問者に提供する体験を決定する
1. ページ上にエクスペリエンスが表示される
1. at.jsは、ページ上のエクスペリエンスを

各パターンは異なる部分で構成され、各部分は[!DNL Target]実装の重要な実装要件に対応しています。

各部分については、このガイドの別の記事で説明します。

* [SDKの初期化](/help/dev/patterns/recs-atjs/initialize-sdk.md)
* [データ収集の設定](/help/dev/patterns/recs-atjs/data-collection.md)
* [エクスペリエンスをレンダリング](/help/dev/patterns/recs-atjs/render-experiences.md)
* [ターゲットに通知](/help/dev/patterns/recs-atjs/notify-target.md)
