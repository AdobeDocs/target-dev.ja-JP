---
keywords: 実装，実装，設定，セットアップ，一括プロファイル更新 api
description: データをに取り込む [!DNL Target] の使用 [!UICONTROL プロファイル一括更新 API].
title: データをに取り込む方法 [!DNL Target] の使用 [!UICONTROL プロファイル一括更新 API]?
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: af9db32d59bdf32f2b9fade267922803250377dd
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 23%

---

# プロファイル一括更新 API

API を使用して、.csv ファイルをに送信します。 [!DNL Adobe Target] 多くの訪問者に対する訪問者プロファイルの更新が含まれています。 各訪問者プロファイルでは、1 回の呼び出しで複数のページ内プロファイル属性を更新できます。

このオプションは、 [!UICONTROL 顧客属性] には、いくつかの違いがあります。

* [!UICONTROL 顧客属性] FTP アップロードを使用します。The [!UICONTROL Target 一括プロファイル更新 API] は HTTPPOSTAPI を使用します。
* [!UICONTROL 顧客属性] データはと共有できます。 [!DNL Analytics]. [!UICONTROL プロファイルの一括更新] はでのみ使用できます [!DNL Target].
* [!UICONTROL 顧客属性] ユーザーのプロファイルの作成をサポート [!DNL Target] まだ表示されていません。
   * [!UICONTROL プロファイル一括更新 API] v2：すべてのパラメーター値を指定する必要はありません `pcId`. プロファイルは、 `pcId` または `mbox3rdPartyId` それは、 [!DNL Target].
   * [!UICONTROL プロファイル一括更新 API] v1: [!UICONTROL プロファイル一括更新 API] 既存の更新 [!DNL Target] プロファイルのみ。 v1 を使用している場合、プロファイルは欠落用に作成されません `pcIds` または `mbox3rdPartyIds`.
* [!UICONTROL 顧客属性] ～を使う必要がある [!UICONTROL Experience CloudID] (ECID) と、ソース ID の使用（CRM ID やロイヤルティ ID など）。
* The [!UICONTROL プロファイル一括更新 API] には、TNT ID または `mbox3rdPartyId`.
* 送信する `mbox3rdPartyID` には、プラス記号（+）とスラッシュ（/）を含めることはできません。

## 形式

.csv ファイルでは、各訪問者を [!DNL Target] PCID または `mbox3rdPartyId`. The [!UICONTROL Experience CloudID] (ECID) はサポートされていません。 すべてのプロファイル属性／値は、API を介して作成および更新されます。形式の詳細については、API ドキュメントを参照してください。

## 使用例

CRM やその他の内部システムは、常に更新しておきたい訪問者に関する貴重なデータを保存します [!DNL Target]（ページ実装でプロファイルデータを公開しないで）

## 方法の利点

* プロファイル属性の数に上限がありません。
* サイトを介して送信されるプロファイル属性は、API を介して更新することも、逆の方法で更新することもできます。

## 注意事項

* バッチファイルの容量は 50 MB 未満にする必要があります。また、1 回にアップロードできる行数は 50 万行までです。
* 通常、更新は 1 時間以内におこなわれますが、反映されるまでに 24 時間かかる場合があります。
* 後続のバッチで 24 時間以内にアップロードできる行数に制限はありません。 ただし、他のプロセスを効率的に実行するために、営業時間中は取り込みプロセスが調整される場合があります。
* 同じに対する、その間に mbox を呼び出さない、連続する V2 一括更新呼び出し。 `thirdPartyIds` は、最初の一括更新呼び出しで更新されたプロパティを上書きします。

## リソース

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)