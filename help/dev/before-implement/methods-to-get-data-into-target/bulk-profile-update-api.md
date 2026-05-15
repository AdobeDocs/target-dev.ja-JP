---
keywords: 実装、実装、設定、設定、一括プロファイル更新api
description: '[!UICONTROL Bulk Profile Update API]を使用して [!DNL Target] にデータを取り込みます。'
title: '[!UICONTROL Bulk Profile Update API]を使用して [!DNL Target] にデータを取り込むにはどうすればよいですか？'
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
TQID: https://experienceleague.adobe.com/vBcIsBR6wwYr7MbRh7UJvt71ULDEq6iXVZ5spZlif-0
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 221
ht-degree: 7%

---

# プロファイルの一括更新API

[!DNL Adobe Target] [!UICONTROL Bulk Profile Update API]では、バッチファイルを使用して、複数の訪問者のユーザープロファイルをWeb サイトに一括で更新できます。

[!UICONTROL Bulk Profile Update API]を使用すると、多くのユーザーのプロファイルパラメーターの形式で詳細な訪問者プロファイルデータを任意の外部ソースから[!DNL Target]に簡単に送信できます。 外部ソースには、顧客関係管理（CRM）システムや販売時点情報管理（POS）システムなどがあります。これらのシステムは、web ページでは通常は利用できません。

[!UICONTROL Bulk Profile Update API]と[[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)を対比します。

## [!UICONTROL Customer attributes]対[!UICONTROL Bulk Profile Update API]

このオプションは、[[!UICONTROL customer attributes]](/help/dev/before-implement/methods-to-get-data-into-target/customer-attributes.md)と似ていますが、いくつかの違いがあります。

* [!UICONTROL Customer attributes]はFTP アップロードを使用します。 [!UICONTROL Target Bulk Profile Update API]はHTTP POST APIを使用しています。
* [!UICONTROL Customer attribute] データは[!DNL Analytics]と共有できます。 [!UICONTROL Bulk Profile Update]は[!DNL Target]でのみ使用できます。
* ユーザー[!DNL Target]のプロファイルの作成に関する[!UICONTROL Customer attributes]のサポートはまだ表示されていません。
   * [!UICONTROL Bulk Profile Update API] v2：各`pcId`に対してすべてのパラメーター値を指定する必要はありません。 プロファイルは、[!DNL Target]に見つからない`pcId`または`mbox3rdPartyId`に対して作成されます。
   * [!UICONTROL Bulk Profile Update API] v1: [!UICONTROL Bulk Profile Update API]は既存の[!DNL Target] プロファイルのみを更新します。 v1を使用している場合、`pcIds`または`mbox3rdPartyIds`が見つからないプロファイルは作成されません。
* [!UICONTROL Customer attributes]では、[!UICONTROL Experience Cloud ID] （ECID）の使用と、CRM IDやロイヤルティ IDなどのソース IDの使用が必要です。
* [!UICONTROL Bulk Profile Update API]には、TNT IDまたは`mbox3rdPartyId`のいずれかが必要です。
* 送信する `mbox3rdPartyID` には、プラス記号（+）とスラッシュ（/）を含めることはできません。

## リソース

詳しくは、次を参照してください。

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)