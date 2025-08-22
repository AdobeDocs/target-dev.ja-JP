---
title: Adobe Target配信 API の概要
description: Adobe Target配信 API の概要
keywords: 配信 api
exl-id: e760bddc-b1ae-4b7b-bff2-aba81c6b6d34
feature: APIs/SDKs
source-git-commit: ccc27e66207e58dcd33865e5d28a51644e8e1931
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 0%

---

# 配信 API の概要

[!DNL Adobe Target Delivery API] は REST に基づいています。 このドキュメントでは、[!DNL Adobe Target] [!DNL Delivery API] を構成するリソースについて説明します。 HTTP メソッドは、これらのリソースに対する操作を実行するために使用されます。

[!UICONTROL Adobe Target's Delivery API] を使用すると、次のことができます。

* SPA やモバイルチャネルを含む web 全体および接続された TV、キオスク、店舗のデジタルスクリーンなどの非ブラウザーベースの IoT デバイスにわたってエクスペリエンスを提供します。
* HTTP/s 呼び出しを行うことができるサーバーサイドプラットフォームまたはアプリケーションからエクスペリエンスを配信します。
* ユーザーがビジネスに関与したチャネルやデバイスに関係なく、一貫したパーソナライズされたエクスペリエンスをユーザーに提供します。
* サーバーのセッション内のユーザーのエクスペリエンスをキャッシュして、複数の API 呼び出しを回避し、その結果、パフォーマンスを向上させることができます。
* サーバー側から [!DNL Adobe Experience Cloud]、[!DNL Adobe Analytics]、[!DNL Adobe Audience Manager] などの [!DNL Experience Cloud ID Service] 製品とシームレスに統合します。

>[!IMPORTANT]
>
>[!DNL Recommendations] から [!UICONTROL Catalog] [!DNL Delivery API] を更新する場合は注意が必要です。 [!DNL Delivery API] は公開されているので、Recommendations カタログのクリック可能な項目への入力には使用しないでください。 無効なコンテンツを追加すると、カタログが汚染される可能性があります。
>
>ベストプラクティス：
>
>[!DNL Delivery API] は、次のようなカタログ属性の更新にのみ使用します。
>* 頻繁に変更する（価格、株価など）。
>* Web サイトで簡単に検証できる事前定義済みの形式に従います。
>* クリック可能な項目やその他の未検証のコンテンツの追加や変更には使用しないでください。
>
>必要に応じて、配信 API を使用してカタログの更新を無効にするようカスタマーサポートにリクエストできます。

詳しくは、[[!UICONTROL Adobe Target Delivery API]](https://developer.adobe.com/target/implement/delivery-api/){target=_blank} ドキュメントを参照してください。

>[!NOTE]
>
>引き続き [ 従来の/v1/mbox および/v2/batchmbox API ドキュメント ](https://developers.adobetarget.com/api/legacy-api/index.html) にアクセスできます。 ただし、機能は（ここで説明しているように）配信 API で開発され、レガシー API には移植されません。
