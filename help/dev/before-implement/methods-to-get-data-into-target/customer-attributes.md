---
keywords: 実装、実装、設定、設定、顧客属性
description: 顧客属性を使用して [!DNL Target] にデータを取り込みます。
title: 顧客属性を使用して [!DNL Target] にデータを取り込むにはどうすればよいですか？
feature: Implementation
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
TQID: https://experienceleague.adobe.com/bzK915y7fvjfZjTkSK2QWHDzmIN9SdAQiEguiUlc-r8
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 216
ht-degree: 23%

---

# 顧客属性

顧客属性を使用すると、FTP経由で訪問者プロファイルデータを[!DNL Adobe Experience Cloud]にアップロードできます。 アップロードしたら、[!DNL Adobe Analytics]と[!DNL Adobe Target]のデータを使用します。

Target Standardのお客様は5つの属性を適用でき、[!DNL Target Premium]のお客様は200の属性を適用できます。

## 形式

[!DNL Experience Cloud] ID （ECID）と属性名と値のペアを持つ.csv ファイルは、FTP経由または手動でExperience Cloud UIにアップロードされます。

## 使用例

CRMまたはその他の社内システムには、[!DNL Target]や[!DNL Analytics]など、[!DNL Adobe Experience Cloud]と共有する貴重な情報が保存されています。

## メソッドの利点

顧客データをアップロードすると、[!DNL Target]が訪問者をまだ表示していない場合でも、Targetでその訪問者のプロファイルエントリが作成されます。

[!DNL Target]と[!DNL Analytics]で同じデータが自動的に利用できます。

FTP は、API よりもシンプルな実装方法です。

## 注意事項

Target Standardのお客様は5つの属性を適用でき、[!DNL Target Premium]のお客様は200の属性を適用できます

ページではなく、顧客属性でのみ値を更新できます。

Experience Cloud ID（ECID）の実装が必要です。

## コード例

詳細については、[顧客属性ソースの作成とデータファイルのアップロード ](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=ja)を参照してください。

### 関連情報へのリンク

[顧客属性ソースを作成し、データファイル ](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html?lang=ja)をアップロードします。
