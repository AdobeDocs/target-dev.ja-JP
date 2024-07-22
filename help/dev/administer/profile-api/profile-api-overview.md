---
title: プロファイルを更新
description: Adobe Target Profile API を使用して訪問者データを  [!DNL Target] に送信する方法を説明します。
contributors: https://github.com/icaraps
exl-id: 482a4175-1d02-47e9-a5c0-dd00e8560773
feature: APIs/SDKs
source-git-commit: 289299a52e5611c0da341f313aa4a447fcf3666a
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 3%

---

# プロファイルを更新

ユーザープロファイルには、年齢、性別、購入した製品、最終訪問時間など、web ページ訪問者の人口統計情報と行動情報が含まれます。 [!DNL Adobe Target] はこの情報を使用して、各訪問者に提供するコンテンツをパーソナライズします。

各訪問者のプロファイル情報は、Cookie またはサードパーティアプリに保存されます。

Web ページで [!DNL Target] コード（[at.js](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) または [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md)）が実装されている場合、プロファイルパラメーターを使用して、cookie からのプロファイル情報が [!DNL Target] に渡されます。 [!DNL Target] は、訪問者の cookie で生成される `pcID` を通じて、各訪問者を一意に識別します。 ただし、`mbox3rdPartyIds` を使用して、外部アプリから mbox 呼び出しを通じてプロファイルパラメーターを渡すことができます。

[!DNL Adobe Target] プロファイル API は、訪問者に関するプロファイルデータがある場合に使用します。このデータは、[!DNL Target] とのページベースの統合の一環として送信できない、または送信したくない [!DNL Target] に送信します。 これは、ページで利用できない顧客関係管理（CRM）または販売時点（POS）システムのデータである可能性があります。 または、このデータは、ページに渡しても意味がない、より機密性の高い性質の場合があります。

API を使用してプロファイルを更新する方法は 2 つあります。

* [単一プロファイル更新 API](/help/dev/administer/profile-api/profile-single-api.md)
* [バッチを使用したプロファイルの一括更新](/help/dev/administer/profile-api/profile-bulk-api.md)
