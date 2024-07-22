---
keywords: サーバーサイド，サーバーサイド，api, sdk, node.js, nodejs, node js, recommendations api, api, api, サーバーサイド 1
description: サーバーサ  [!DNL Adobe Target]  ド配信 API、SDK および API につい  [!DNL Target Recommendations]  説明します。
title: サーバーサ  [!DNL Target]  ド配信 API および SDK について
feature: Implement Server-side
exl-id: 3eb0a789-cf1a-4d02-acf7-3c895bcb662f
source-git-commit: 75af30045684b95d5989b0a1f877ba95bb8cd883
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 13%

---

# サーバー側：[!DNL Target] の実装

[!DNL Adobe Target] サーバーサイド配信 API、SDK および [!DNL Target Recommendations] API に関する情報です。

>[!NOTE]
>
>実装で at.js と [!DNL AppMeasurement] をクライアントサイドで使用する場合は、以下で説明する [!UICONTROL Target Delivery API] およびサーバーサイド SDK を使用する必要があります。
>
>実装で [!UICONTROL Adobe Experience Platform Web SDK] を使用する場合は、[[!UICONTROL Adobe Experience Platform] [!UICONTROL Edge Network Server API]](https://experienceleague.adobe.com/en/docs/experience-platform/edge-network-server-api/overview){target=_blank} を使用する必要があります。

以下の処理は、[!DNL Target] のサーバー側実装で発生します。

1. クライアントデバイスがサーバーを通じてエクスペリエンスのリクエストをおこないます。
1. サーバーは、そのリクエストを [!DNL Target] に送信します。
1. [!DNL Target] は、サーバーに応答を送り返します。
1. サーバーは、レンダリングするエクスペリエンスをクライアントデバイスに配信するかどうかの決定を行います。

エクスペリエンスは、ブラウザーに表示する必要はありません。 エクスペリエンスは、電子メールやキオスクに表示したり、音声アシスタントを使用したり、非視覚的なエクスペリエンスや非ブラウザーベースのデバイスを使用したりして、表示できます。 サーバーはクライアントと [!DNL Target] の間に位置するので、より優れたコントロールおよびセキュリティが必要であったり、サーバーで実行したい複雑なバックエンド処理がある場合、このタイプの実装も理想的です。

>[!NOTE]
>
>初回訪問者は、クライアントサイドでのみ初期化できます。 初回の訪問者 *初期化できません* をサーバーサイドで行うことはできません。 これは ECID に起因します。この ECID はサードパーティの demdex cookie に依存するので、ブラウザーが関係する実装で訪問者 API.js を使用して初期化する必要があります。

次の節では、様々なサーバーサイド API と SDK について詳しく説明します。

## Server Side Delivery API

リンク：[ サーバーサイド配信 API](/help/dev/implement/delivery-api/overview.md)

`/rest/v1/delivery`

[!DNL Target] 配信 API を使用すると、次のことができます。

* SPAやモバイルチャネルを含む web 全体および接続された TV、キオスク、店舗のデジタルスクリーンなどの非ブラウザーベースの IoT デバイスにわたってエクスペリエンスを提供します。
* HTTP/s 呼び出しを行えるサーバーサイドプラットフォームまたはアプリケーションからエクスペリエンスを配信します。
* 訪問者がビジネスへのエンゲージメントに使用したチャネルやデバイスに関係なく、訪問者に対して一貫性のあるパーソナライズされたエクスペリエンスを提供します。
* サーバー上のセッション内の訪問者のエクスペリエンスをキャッシュして、複数の API 呼び出しを回避し、パフォーマンスを向上させます。
* Adobe Analytics、Adobe Audience Manager（AAM）などのAdobe Experience Cloud製品、Experience CloudID サービスとサーバーサイドからシームレスに統合します。

## サーバーサイド SDK

[!DNL Adobe Target] サーバーサイド SDK のドキュメントは、選択した言語でサーバーに [!DNL Target] を実装するのに役立ちます。

* [Node.js](node-js/overview.md)
* [Java](java/overview.md)
* [.NET](net/overview.md)
* [Python](python/overview.md)

[!DNL Adobe Target] のサーバーサイド SDK を使用すると、次のことができます。

* **機能フラグ設定**、**ロールアウト** および **A/B 実験** を **ほぼゼロの待ち時間** で実行および実行します。
* **2}SPA** や **モバイルチャネル** などの **web 全体でエクスペリエンスを提供できるだけでなく、ブラウザー以外のベースの** IoT （Internet of Things）デバイス **接続テレビ、キオスク、店舗のデジタル画面など）でもエクスペリエンスを提供できます。**
* ユーザーがビジネスに関与したチャネルやデバイスに関係なく、**機械学習（ML）駆動のパーソナライズされたエクスペリエンス** をユーザーに提供します。
* **Adobe Experience Cloudとシームレスに統合** サーバー側から **Adobe Analytics**、**Adobe Audience Manager**、**Experience CloudID サービス** などの製品を利用できます。

[ オンデバイス判定 ](sdk-guides/getting-started/getting-started.md) を使用して、シンプルな機能フラグ設定のユースケースを実行する方法については、[ はじめに ](sdk-guides/on-device-decisioning/overview.md) ページを参照してください。

楽しく遊ぶには、[ サンプルアプリ ](sdk-guides/sample-apps/sample-apps.md) をチェックしてください。

## [!DNL Target Recommendations] API

リンク：[Target Recommendations API](https://developers.adobetarget.com/api/recommendations) および [Adobe Recommendations API の概要 ](../../before-administer/recs-api/overview.md)。

Recommendations API を使用すると、[!DNL Target] Recommendations サーバーとプログラムによってやり取りできます。 これらの API は様々なアプリケーションスタックと統合して、通常 [!DNL Target] ユーザーインターフェイスからおこなわれる関数を実行できます。
