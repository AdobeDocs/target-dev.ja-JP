---
keywords: 実装，実装，設定，設定，顧客属性
description: データをに取り込む [!DNL Target] 顧客属性を使用する。
title: データをに取り込む方法 [!DNL Target] 顧客属性を使用している場合
feature: Implementation
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '208'
ht-degree: 25%

---

# 顧客属性

顧客属性を使用すると、FTP を使用して訪問者プロファイルデータを [!DNL Adobe Experience Cloud]. アップロード後は、 [!DNL Adobe Analytics] および [!DNL Adobe Target].

Target Standard のお客様は、次の 5 つの属性を適用できます。 [!DNL Target Premium] のお客様は 200 個の属性を適用できます。

## 形式

を含む.csv ファイル [!DNL Experience Cloud] ID(ECID) と属性名/値のペアは、FTP を使用して、またはExperience CloudUI で手動でアップロードされます。

## 使用例

CRM または他の社内システムに、共有したい貴重な情報が保存されます [!DNL Adobe Experience Cloud]を含む [!DNL Target] および [!DNL Analytics].

## 方法の利点

顧客データをアップロードすると、Target でその訪問者のプロファイルエントリが作成されます ( たとえ、 [!DNL Target] はまだ訪問者を見ていません。

で同じデータを自動的に使用できます。 [!DNL Target] および [!DNL Analytics].

FTP は、API よりもシンプルな実装方法です。

## 注意事項

Target Standard のお客様は、次の 5 つの属性を適用できます。 [!DNL Target Premium] のお客様は 200 個の属性を適用できます。

ページではなく、顧客属性でのみ値を更新できます。

Experience Cloud ID（ECID）の実装が必要です。

## コードの例

詳しくは、 [顧客属性ソースの作成とデータファイルのアップロード](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=ja).

### 関連情報へのリンク

[顧客属性ソースの作成とデータファイルのアップロード](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=ja).
