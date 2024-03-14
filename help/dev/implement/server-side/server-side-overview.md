---
keywords: サーバー側，サーバー側， api, sdk, node.js, nodejs, node js, recommendations api, api, api，サーバー側 1
description: 詳しくは、 [!DNL Adobe Target] サーバー側配信 API、SDK、 [!DNL Target Recommendations] API
title: どこで学べばよいか [!DNL Target] サーバー側配信 API および SDK
feature: Implement Server-side
exl-id: 3eb0a789-cf1a-4d02-acf7-3c895bcb662f
source-git-commit: 75af30045684b95d5989b0a1f877ba95bb8cd883
workflow-type: tm+mt
source-wordcount: '569'
ht-degree: 13%

---

# サーバー側：実装 [!DNL Target]

次に関する情報： [!DNL Adobe Target] サーバー側配信 API、SDK、 [!DNL Target Recommendations] API

>[!NOTE]
>
>実装で at.js および [!DNL AppMeasurement] クライアント側では、 [!UICONTROL Target Delivery API] およびサーバー側 SDK については、以下で説明します。
>
>実装で [!UICONTROL Adobe Experience Platform Web SDK]を使用する場合、 [[!UICONTROL Adobe Experience Platform] [!UICONTROL Edge Network Server API]](https://experienceleague.adobe.com/en/docs/experience-platform/edge-network-server-api/overview){target=_blank}.

以下の処理は、[!DNL Target] のサーバー側実装で発生します。

1. クライアントデバイスがサーバーを通じてエクスペリエンスのリクエストをおこないます。
1. サーバーは、そのリクエストを [!DNL Target] に送信します。
1. [!DNL Target] は、サーバーに応答を送り返します。
1. サーバーは、エクスペリエンスをレンダリングするクライアントデバイスに対して、どのエクスペリエンスを配信するかを決定します。

エクスペリエンスをブラウザーに表示する必要はありません。 エクスペリエンスは、E メールやキオスク、音声アシスタント、または他の視覚的でないエクスペリエンスやブラウザーベース以外のデバイスを通じて表示できます。 サーバーはクライアントと [!DNL Target] の間に位置するので、より優れたコントロールおよびセキュリティが必要であったり、サーバーで実行したい複雑なバックエンド処理がある場合、このタイプの実装も理想的です。

>[!NOTE]
>
>初回訪問者は、クライアント側でのみ初期化できます。 初回訪問者 *できません* をサーバー側で初期化する。 これは、ECID（サードパーティの demdex Cookie に依存するため、ブラウザーが関与する実装で Visitor API.js を使用して初期化する必要があります）が原因です。

以下の節では、様々なサーバー側 API および SDK の詳細を説明します。

## Server Side Delivery API

リンク： [Server Side Delivery API](/help/dev/implement/delivery-api/overview.md)

`/rest/v1/delivery`

を通じて [!DNL Target] Delivery API では、次の操作を実行できます。

* SPAやモバイルチャネルを含む Web をまたいでエクスペリエンスを提供します。また、接続された TV、キオスク、店内のデジタル画面など、ブラウザーベース以外の IoT デバイスも提供します。
* HTTP/s 呼び出しをおこなうことのできる任意のサーバー側プラットフォームまたはアプリケーションからエクスペリエンスを提供します。
* 訪問者がビジネスに関与するために使用したチャネルやデバイスに関係なく、一貫性のあるパーソナライズされたエクスペリエンスを訪問者に提供します。
* サーバー上のセッション内で訪問者のエクスペリエンスをキャッシュして、複数の API 呼び出しを回避できるようにします。これにより、パフォーマンスが向上します。
* Adobe Analytics、Adobe Audience Manager(AAM)、Adobe Experience Cloud ID サービスなどのExperience Cloud製品とサーバー側からシームレスに統合できます。

## Server Side SDK

The [!DNL Adobe Target] 実装に役立つサーバー側 SDK のドキュメント [!DNL Target] を選択した言語のサーバーで使用します。

* [Node.js](node-js/overview.md)
* [Java](java/overview.md)
* [.NET](net/overview.md)
* [Python](python/overview.md)

～ [!DNL Adobe Target]のサーバー側 SDK では、次のことが可能です。

* 実行 **機能フラグ付け**, **ロールアウト**、および **A/B 実験** 時刻 **ほぼゼロの遅延**.
* エクスペリエンスを次の期間にわたって提供 **web**&#x200B;を含む **SPA**、および **モバイルチャネル**、およびブラウザーベース以外 **IoT（モノのインターネット）デバイス** （接続されたテレビ、キオスク、店内のデジタル画面など）。
* 配信 **機械学習 (ML) 駆動のパーソナライズされたエクスペリエンス** ユーザーがビジネスに関与したチャネルやデバイスに関係なく、ユーザーに割り当てられます。
* **Adobe Experience Cloudとシームレスに統合** 次のような製品： **Adobe Analytics**, **Adobe Audience Manager**、および **Experience CloudID サービス** をサーバー側から削除します。

詳しくは、 [はじめに](sdk-guides/getting-started/getting-started.md) シンプルな機能フラグ設定の使用例をで実行する方法について説明します [オンデバイス判定](sdk-guides/on-device-decisioning/overview.md).

以下をご確認ください。 [サンプルアプリ](sdk-guides/sample-apps/sample-apps.md) 楽しみ、遊びまわしたい！

## [!DNL Target Recommendations] API

リンク： [Target Recommendations API](https://developers.adobetarget.com/api/recommendations) および [Adobe Recommendations API の概要](../../before-administer/recs-api/overview.md).

Recommendations API を使用すると、プログラムによる操作が可能です [!DNL Target] recommendations サーバー。 これらの API を様々なアプリケーションスタックと統合して、通常は [!DNL Target] ユーザーインターフェイス。
