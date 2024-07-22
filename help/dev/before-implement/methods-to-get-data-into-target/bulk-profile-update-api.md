---
keywords: 実装、実装、設定、設定、一括プロファイル更新 api
description: '[!UICONTROL Bulk Profile Update API] を使用してデ  [!DNL Target]  タをに取り込みます。'
title: '[!UICONTROL Bulk Profile Update API] を使用してデータをに取  [!DNL Target]  込むにはどうすればよいですか？'
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: 946e9431e6bde30f564b4ba1a4cf0a78d8c5c6bf
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 7%

---

# プロファイル一括更新 API

[!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] では、Web サイトへの複数の訪問者のユーザープロファイルをバッチファイルで一括更新できます。

[!UICONTROL Bulk Profile Update API] を使用すると、多くのユーザーが任意の外部ソースから [!DNL Target] 信できるように、プロファイルパラメーターの形式で詳細な訪問者プロファイルデータを簡単に送信できます。 外部ソースには顧客関係管理（CRM）システムや販売時点（POS）システムを含めますが、これらのシステムは通常、web ページでは使用できません。

[!UICONTROL Bulk Profile Update API] と [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md) を対比しなさい。

## [!UICONTROL Customer attributes] 対 [!UICONTROL Bulk Profile Update API]

このオプションは [[!UICONTROL customer attributes]](/help/dev/before-implement/methods-to-get-data-into-target/customer-attributes.md) に似ていますが、いくつかの違いがあります。

* FTP アップロードを使用で [!UICONTROL Customer attributes] ます。 [!UICONTROL Target Bulk Profile Update API] では、HTTPPOSTAPI が使用されます。
* [!UICONTROL Customer attribute] データは [!DNL Analytics] と共有できます。 [!UICONTROL Bulk Profile Update] は [!DNL Target] でのみ使用できます。
* [!UICONTROL Customer attributes] だ表示されていないユーザーのプロファイルの作成 [!DNL Target] サポートします。
   * [!UICONTROL Bulk Profile Update API] v2：各 `pcId` に対してすべてのパラメーター値を指定する必要はありません。 プロファイルは、[!DNL Target] に見つからない `pcId` または `mbox3rdPartyId` に対して作成されます。
   * [!UICONTROL Bulk Profile Update API] v1:[!UICONTROL Bulk Profile Update API] では、既存の [!DNL Target] プロファイルのみが更新されます。 v1 を使用している場合、`pcIds` または `mbox3rdPartyIds` が見つからないプロファイルは作成されません。
* [!UICONTROL Experience Cloud ID] （ECID）を使用し、ソース ID （CRM ID やロイヤルティ ID など）を使用する必要が [!UICONTROL Customer attributes] ります。
* [!UICONTROL Bulk Profile Update API] には、TNT ID または `mbox3rdPartyId` が必要です。
* 送信する `mbox3rdPartyID` には、プラス記号（+）とスラッシュ（/）を含めることはできません。

## リソース

詳しくは、次を参照してください。

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)