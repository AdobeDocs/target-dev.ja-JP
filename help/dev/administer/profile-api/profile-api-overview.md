---
title: Adobe Target Profile API の概要
description: Adobe Target Profile API を使用して訪問者データをに送信する方法を説明します。 [!DNL Target].
contributors: https://github.com/icaraps
exl-id: 482a4175-1d02-47e9-a5c0-dd00e8560773
feature: APIs/SDKs
source-git-commit: 1a1c3d96cf6ef5c337a63fdec8c700da695ff5d1
workflow-type: tm+mt
source-wordcount: '233'
ht-degree: 1%

---

# [!DNL Adobe Target Profile APIs overview]

ユーザープロファイルには、年齢、性別、購入した製品、最終訪問時刻など、Web ページ訪問者の人口統計情報と行動情報が含まれます。 [!DNL Adobe Target] は、この情報を使用して、各訪問者に提供するコンテンツをパーソナライズします。

各訪問者のプロファイル情報は、Cookie またはサードパーティアプリに保存されます。

Web ページで [!DNL Target] コード ([at.js](/help/dev/implement/client-side/atjs/how-atjs-works/overview.md) または [Adobe Experience Platform Web SDK](/help/dev/implement/client-side/aep-web-sdk.md)) の場合、Cookie からのプロファイル情報がに渡されます。 [!DNL Target] プロファイルパラメーターを使用する。 [!DNL Target] は、 `pcID` 訪問者の cookie で生成される値です。 ただし、 `mbox3rdPartyIds`.

以下を使用します。 [!DNL Adobe Target] プロファイル API を使用します（訪問者に関するプロファイルデータがある場合）。 [!DNL Target] をページベースのとの統合の一部として送信できない、または送信したくない [!DNL Target]. これは、顧客関係管理 (CRM) または POS(POS) システムからのデータで、ページ上で使用できない場合があります。 また、このデータは、ページで渡す意味のない、より繊細な性質を持つ場合もあります。

API を使用してプロファイルを更新するには、次の 2 つの方法があります。

* [単一プロファイル更新 API](/help/dev/administer/profile-api/profile-single-api.md)
* [一括更新によるプロファイルの一括更新](/help/dev/administer/profile-api/profile-bulk-api.md)

従来のプロファイル API のドキュメントについては、次の場所を参照してください。 [https://developers.adobetarget.com/api/#profiles](https://developers.adobetarget.com/api/#profiles){target=_blank}
