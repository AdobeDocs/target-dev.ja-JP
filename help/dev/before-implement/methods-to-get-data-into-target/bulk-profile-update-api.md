---
keywords: 実装，実装，設定，セットアップ，一括プロファイル更新 api
description: データをに取り込む [!DNL Target] の使用 [!UICONTROL プロファイル一括更新 API].
title: データをに取り込む方法 [!DNL Target] の使用 [!UICONTROL プロファイル一括更新 API]?
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 43f4fb8345a77ccb0e112fe196e7e0944cc468c9
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 4%

---

# プロファイル一括更新 API

The [!DNL Adobe Target] [!UICONTROL プロファイル一括更新 API] バッチファイルを使用して、1 つの web サイトに対する複数の訪問者のユーザープロファイルを一括で更新できます。

の使用 [!UICONTROL プロファイル一括更新 API]を使用すると、多くのユーザーが以下の操作をおこなうために、詳細な訪問者プロファイルデータをプロファイルパラメーターの形式で簡単に送信できます。 [!DNL Target] 任意の外部ソースから。 外部ソースには、顧客関係管理 (CRM) システムや POS(Point of Sale) システムが含まれますが、これらは Web ページでは通常利用できません。

## [!UICONTROL 顧客属性] 対 [!UICONTROL プロファイル一括更新 API]

このオプションは、 [!UICONTROL 顧客属性] には、いくつかの違いがあります。

* [!UICONTROL 顧客属性] FTP アップロードを使用します。The [!UICONTROL Target 一括プロファイル更新 API] は HTTPPOSTAPI を使用します。
* [!UICONTROL 顧客属性] データはと共有できます。 [!DNL Analytics]. The [!UICONTROL プロファイルの一括更新] はでのみ使用できます [!DNL Target].
* [!UICONTROL 顧客属性] ユーザーのプロファイルの作成をサポート [!DNL Target] まだ表示されていません。
   * [!UICONTROL プロファイル一括更新 API] v2：すべてのパラメーター値を指定する必要はありません `pcId`. プロファイルは、 `pcId` または `mbox3rdPartyId` それは、 [!DNL Target].
   * [!UICONTROL プロファイル一括更新 API] v1: [!UICONTROL プロファイル一括更新 API] 既存の更新 [!DNL Target] プロファイルのみ。 v1 を使用している場合、プロファイルは欠落用に作成されません `pcIds` または `mbox3rdPartyIds`.
* [!UICONTROL 顧客属性] ～を使う必要がある [!UICONTROL Experience CloudID] (ECID) と、ソース ID の使用（CRM ID やロイヤルティ ID など）。
* The [!UICONTROL プロファイル一括更新 API] には、TNT ID または `mbox3rdPartyId`.
* 送信する `mbox3rdPartyID` には、プラス記号（+）とスラッシュ（/）を含めることはできません。

## リソース

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)