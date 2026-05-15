---
title: プロファイルを更新
description: Adobe Target Profile APIを使用して訪問者データを [!DNL Target]に送信する方法を説明します。
contributors: https://github.com/icaraps
exl-id: 482a4175-1d02-47e9-a5c0-dd00e8560773
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/jclCuF4pe3JwAN-2RhQ9NfA5KtEfKUuawlmrE3aS0bQ
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 217
ht-degree: 3%

---

# プロファイルを更新

ユーザープロファイルには、年齢、性別、購入した製品、最後に訪問した時間など、web ページ訪問者のデモグラフィック情報と行動情報が含まれます。 [!DNL Adobe Target]は、この情報を使用して、各訪問者に提供するコンテンツをパーソナライズします。

各訪問者のプロファイル情報は、Cookieまたはサードパーティのアプリに保存されます。

Web ページで[!DNL Target] コード （[at.js](/help/dev/implement/client-side/atjs/how-atjs-works/how-atjs-works.md)または[Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk/aep-web-sdk-overview.md)）が実装されている場合、Cookieのプロファイル情報はプロファイルパラメーターを使用して[!DNL Target]に渡されます。 [!DNL Target]は、訪問者のCookieに生成される`pcID`を通じて、各訪問者を一意に識別します。 ただし、`mbox3rdPartyIds`を使用して、外部アプリのプロファイルパラメーターをmbox呼び出しを通じて渡すことができます。

[!DNL Target]とのページベース統合の一環として送信できない、または送信したくない[!DNL Target]に訪問者に関するプロファイルデータがある場合は、[!DNL Adobe Target] プロファイル APIを使用します。 CRM （顧客関係管理）システムやPOS （販売時点情報管理）システムのデータがページ上にない場合があります。 あるいは、このデータは、より機密性の高い性質のもので、ページに渡す意味がないかもしれません。

APIを使用してプロファイルを更新するには、次の2つの方法があります。

* [単一プロファイル更新 API](/help/dev/administer/profile-api/profile-single-api.md)
* [バッチによるプロファイルの一括更新](/help/dev/administer/profile-api/profile-bulk-api.md)
