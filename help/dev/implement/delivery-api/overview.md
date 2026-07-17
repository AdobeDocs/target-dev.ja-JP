---
title: Adobe Target Delivery APIの概要
description: Adobe Target Delivery APIの概要
keywords: Delivery API
exl-id: e760bddc-b1ae-4b7b-bff2-aba81c6b6d34
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/gPXGax6ccvZZPklT3jnZbqyOj3mCClEfSpdufAFPtSs
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: b6b447ccb88925a8efb6ff6a80ae475c8780dbc8
workflow-type: tm+mt
source-wordcount: 244
ht-degree: 0%

---

# 配信APIの概要

[!DNL Adobe Target Delivery API]はRESTに基づいています。 このドキュメントでは、[!DNL Adobe Target] [!DNL Delivery API]を構成するリソースについて説明します。 HTTP メソッドは、これらのリソースに対する操作を実行するために使用されます。

>[!IMPORTANT]
>
>ここで説明する[!DNL Delivery API]は、[!DNL at.js]および直接サーバーサイド実装を目的としています。 [!DNL Adobe Experience Platform Web SDK]を使用して[!DNL Target]を実装する場合は、[!DNL Delivery API]を直接呼び出す代わりに、[!UICONTROL Experience Platform Edge Network]経由で`sendEvent` コマンドを使用してアクセスするInteract APIを使用します。 詳しくは、[Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)および[at.js ライブラリとExperience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/web-sdk-atjs-comparison.md)の比較を参照してください。

[!UICONTROL Adobe Targetの配信API]を使用すると、次のことが可能になります。

* SPAやモバイルチャネルを含むwebはもちろん、コネクテッド TV、キオスク、実店舗のデジタルスクリーンなど、ブラウザー以外のIoT デバイスにもエクスペリエンスを提供できます。
* HTTP/S呼び出しを行うことができる、任意のサーバーサイドプラットフォームまたはアプリケーションからエクスペリエンスを配信します。
* 利用者がどのチャネルやデバイスを利用したとしても、一貫性のあるパーソナライズされた体験を利用者に提供できます。
* サーバー内のセッション内にユーザーのエクスペリエンスをキャッシュして、複数のAPI呼び出しを回避し、結果としてパフォーマンスを向上させることができます。
* サーバーサイドから[!DNL Adobe Analytics]、[!DNL Adobe Audience Manager]、[!DNL Experience Cloud ID Service]などの[!DNL Adobe Experience Cloud]製品とシームレスに統合できます。

>[!NOTE]
>
>引き続き、[従来の/v1/mboxおよび/v2/batchmbox API ドキュメント &#x200B;](https://developers.adobetarget.com/api/legacy-api/index.html)にアクセスできます。 ただし、機能は（ここに記載されているように）配信APIで開発され、従来のAPIにはバックレポートされません。
