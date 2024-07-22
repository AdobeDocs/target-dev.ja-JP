---
keywords: 実装，実装，設定，設定，顧客属性
description: 顧客属性を使用してデ  [!DNL Target]  タをに取り込みます。
title: 顧客属性を使用してデータをに取  [!DNL Target]  込むにはどうすればよいですか？
feature: Implementation
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 15%

---

# 顧客属性

顧客属性を使用すると、FTP 経由で訪問者プロファイルデータを [!DNL Adobe Experience Cloud] にアップロードできます。 アップロードが完了したら、データを [!DNL Adobe Analytics] と [!DNL Adobe Target] で使用できます。

Target Standardのお客様は 5 つの属性を適用できます [!DNL Target Premium]、お客様は 200 の属性を適用できます。

## 形式

[!DNL Experience Cloud] ID （ECID）と属性名/値のペアを含んだ.csv ファイルが、FTP を使用して、またはExperience CloudUI で手動でアップロードされます。

## 使用例

CRM やその他の内部システムには、[!DNL Target] や [!DNL Analytics] など、[!DNL Adobe Experience Cloud] と共有したい貴重な情報が保存されています。

## メソッドの利点

顧客データをアップロードすると、その訪問者のプロファイルエントリが Target に作成されます（まだ訪問者が表示され [!DNL Target] いない場合も含む）。

[!DNL Target] と [!DNL Analytics] では、同じデータを自動的に使用できます。

FTP は、API よりもシンプルな実装方法です。

## 注意事項

Target Standardのお客様は 5 つの属性を適用できます [!DNL Target Premium]、200 個の属性を適用できます

ページではなく、顧客属性でのみ値を更新できます。

Experience Cloud ID（ECID）の実装が必要です。

## コードの例

詳しくは、[ 顧客属性ソースの作成とデータファイルのアップロード ](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=ja) を参照してください。

### 関連情報へのリンク

[ 顧客属性ソースの作成とデータファイルのアップロード ](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=ja)。
