---
keywords: サーバーサイド、サーバーサイド、api、sdk、node.js、nodejs、node js、recommendations api、api、api、server side1
description: ' [!DNL Adobe Target]  サーバーサイド配信API、SDK、および [!DNL Target Recommendations] APIについて説明します。'
title: ' [!DNL Target]  サーバーサイド配信APIとSDKについて学ぶにはどうすればよいですか？'
feature: Implement Server-side
exl-id: 3eb0a789-cf1a-4d02-acf7-3c895bcb662f
TQID: https://experienceleague.adobe.com/x5WKb9Eenz2bw-idOnxlpWdtiivTx05n38sNXEt3DNc
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: b050e0cd-2ddd-42cd-a71b-5d9e1fdf75e0id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: a6cc21b9-1a36-4fa6-9c61-4acd04d9c88cid: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c2be0313-b3ae-45e0-b454-d20bf54b23f2id: d095671a-1355-40aa-8b5f-06c33c68080bid: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 603
ht-degree: 12%

---

# サーバーサイド：[!DNL Target]を実装

[!DNL Adobe Target]のサーバーサイド配信API、SDK、および[!DNL Target Recommendations]個のAPIに関する情報。

>[!NOTE]
>
>実装でat.jsと[!DNL AppMeasurement]をクライアントサイドで使用する場合は、以下で説明する[!UICONTROL Target Delivery API]とサーバーサイド SDKを使用する必要があります。
>
>実装で[!UICONTROL Adobe Experience Platform Web SDK]を使用している場合は、[[!UICONTROL Adobe Experience Platform] [!UICONTROL Edge Network Server API]](https://experienceleague.adobe.com/en/docs/experience-platform/edge-network-server-api/overview){target=_blank}を使用する必要があります。

以下の処理は、[!DNL Target] のサーバー側実装で発生します。

1. クライアントデバイスがサーバーを通じてエクスペリエンスのリクエストをおこないます。
1. サーバーは、そのリクエストを [!DNL Target] に送信します。
1. [!DNL Target] は、サーバーに応答を送り返します。
1. サーバーは、クライアントデバイスに配信するエクスペリエンスをレンダリングするエクスペリエンスを決定します。

エクスペリエンスをブラウザーに表示する必要はありません。 エクスペリエンスは、電子メールやキオスク、音声アシスタント、その他の視覚的ではないエクスペリエンスやブラウザー以外のデバイスで表示できます。 サーバーはクライアントと [!DNL Target] の間に位置するので、より優れたコントロールおよびセキュリティが必要であったり、サーバーで実行したい複雑なバックエンド処理がある場合、このタイプの実装も理想的です。

>[!NOTE]
>
>初回訪問者は、クライアントサイドでのみ初期化できます。 初回訪問者&#x200B;*はサーバーサイドで初期化できません*。 これはECIDによるもので、これはサードパーティのdemdex cookieに依存するため、ブラウザーが関与する実装でVisitor API.jsを介して初期化する必要があります。

以下の節では、様々なサーバーサイド APIとSDKについて詳しく説明します。

## Server Side Delivery API

リンク：[ サーバーサイド配信API](/help/dev/implement/delivery-api/overview.md)

`/rest/v1/delivery`

[!DNL Target] Delivery APIを使用すると、次のことが可能になります。

* SPAやモバイルチャネルを含むwebはもちろん、コネクテッド TV、キオスク、実店舗のデジタルスクリーンなど、ブラウザー以外のIoT デバイスにもエクスペリエンスを提供できます。
* HTTP/S呼び出しを行うことができる、任意のサーバーサイドプラットフォームまたはアプリケーションから、エクスペリエンスを配信します。
* 訪問者がどのチャネルやデバイスを利用してビジネスに関与したとしても、訪問者に一貫性のあるパーソナライズされた体験を提供します。
* 訪問者のエクスペリエンスをサーバー上のセッション内にキャッシュして、複数のAPI呼び出しを回避できるようにすることで、パフォーマンスを向上させます。
* サーバーサイドから、Adobe Analytics、Adobe Audience Manager（AAM）、Experience Cloud ID ServiceなどのAdobe Experience Cloud製品とシームレスに連携できます。

## サーバーサイド SDK

[!DNL Adobe Target] サーバーサイド SDKのドキュメントは、お使いの言語でサーバーに[!DNL Target]を実装するのに役立ちます。

* [Node.js](node-js/overview.md)
* [Java](java/overview.md)
* [.NET](net/overview.md)
* [Python](python/overview.md)

[!DNL Adobe Target]のサーバーサイド SDKを使用すると、次のことが可能になります。

* **機能のフラグ付け**、**ロールアウト**、および&#x200B;**A/B実験**&#x200B;を&#x200B;**ほぼゼロの遅延**&#x200B;で実行して実行します。
* **SPA**&#x200B;と&#x200B;**モバイルチャネル**&#x200B;を含む&#x200B;**web**&#x200B;全体、および接続されたテレビ、キオスク、実店舗のデジタル画面などのブラウザー以外の&#x200B;**モノのインターネット（IoT）デバイス**&#x200B;を含むエクスペリエンスを配信します。
* ユーザーがどのチャネルやデバイスとエンゲージしたかに関係なく、**機械学習（ML）を活用したパーソナライズされたエクスペリエンス**&#x200B;をユーザーに提供します。
* **サーバーサイドから** Adobe Experience Cloud **、** Adobe Analytics **、** Adobe Audience Manager ID サービス **などの**&#x200B;製品とシームレスに連携できます。

[ デバイス上の決定](sdk-guides/on-device-decisioning/overview.md)を介してシンプルな機能フラグ付けユースケースを実行する方法については、[はじめに](sdk-guides/getting-started/getting-started.md) ページを参照してください。

[ サンプルアプリ ](sdk-guides/sample-apps/sample-apps.md)をチェックして、楽しく遊びましょう。

## [!DNL Target Recommendations] API

リンク：[Target Recommendations API](https://developers.adobetarget.com/api/recommendations)および[Adobe Recommendations APIの概要](../../before-administer/recs-api/overview.md)。

Recommendations APIを使用すると、[!DNL Target]個のRecommendations サーバーとプログラムで対話できます。 これらのAPIは、通常[!DNL Target] ユーザーインターフェイスを介して実行する機能を実行するために、さまざまなアプリケーションスタックと統合できます。
