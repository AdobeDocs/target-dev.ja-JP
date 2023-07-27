---
keywords: 実装，実装，設定，設定，一括プロファイルの更新
description: データをに取り込む [!DNL Target] 一括プロファイル更新 API を使用する。
title: データをに取り込む方法 [!DNL Target] プロファイル一括更新 API を使用している場合
feature: Implementation
exl-id: 654b13b7-1683-4c44-80e6-7557b9d29f66
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '387'
ht-degree: 60%

---

# プロファイル一括更新 API

API を使用して、.csv ファイルをに送信します。 [!DNL Adobe Target] 多くの訪問者に対する訪問者プロファイルの更新が含まれています。 各訪問者プロファイルでは、1 回の呼び出しで複数のページ内プロファイル属性を更新できます。

このオプションは顧客属性に似ていますが、次のように少し違いがあります。

* 顧客属性は、 [!UICONTROL Target 一括プロファイル更新 API] は HTTPPOSTAPI を使用します。
* 顧客属性データはと共有できます [!DNL Analytics]. [!UICONTROL プロファイルの一括更新は Target のみで使用できます。]
* 顧客属性は、ユーザーのプロファイルの作成をサポートします [!DNL Target] まだ表示されていません。 プロファイル一括更新 API は既存の [!DNL Target] プロファイルのみ。
* 顧客属性には、Experience CloudID(ECID) と、CRM ID やロイヤルティ ID などのソース ID を使用する必要があります。
* プロファイル一括更新 API では、TNT ID または `mbox3rdPartyId` が必要です。
* 送信する `mbox3rdPartyID` には、プラス記号（+）とスラッシュ（/）を含めることはできません。

## 形式

.csv ファイルでは、各訪問者を [!DNL Target] PCID または `mbox3rdPartyId`. Experience Cloud ID（ECID）には対応していません。すべてのプロファイル属性／値は、API を介して作成および更新されます。形式の詳細については、API ドキュメントを参照してください。

## 使用例

ページの実装でプロファイルデータを公開することなく、CRM やその他の社内システムに、一貫して更新して Target に送信したい有用な訪問者データを保管します。

## 方法の利点

プロファイル属性の数に上限がありません。

サイトを介して送信されるプロファイル属性を、API を介して更新できます（その逆も可能です）。

## 注意事項

バッチファイルの容量は 50 MB 未満にする必要があります。また、1 回にアップロードできる行数は 50 万行までです。

以降のバッチで、24 時間以内にアップロードできる回数や行数に上限はありません。ただし、他のプロセスを効率的に実行するために、営業時間中は取り込みプロセスが調整される場合があります。

合間に mbox を呼び出すことなく、連続する [V2 一括更新呼び出し（](https://developers.adobetarget.com/api/#updating-profiles)）を同じ thirdPartyIds に対しておこなうと、最初の一括更新呼び出しで更新されたプロパティは上書きされます。

## コードの例

[プロファイルの更新](https://developers.adobetarget.com/api/#updating-profiles)を参照してください。

### 関連情報へのリンク

[プロファイルの更新](https://developers.adobetarget.com/api/#updating-profiles)
