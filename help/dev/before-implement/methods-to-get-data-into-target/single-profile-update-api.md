---
keywords: 実装，実装，設定，設定，単一プロファイルの更新
description: データをに取り込む [!DNL Target] 単一のプロファイル更新 API を使用する。
title: データをに取り込む方法 [!DNL Target] の使用 [!UICONTROL 単一プロファイル更新 API]?
feature: Implementation
exl-id: e6c394cb-74a3-4991-b656-5ae601f2d5e2
source-git-commit: 43f4fb8345a77ccb0e112fe196e7e0944cc468c9
workflow-type: tm+mt
source-wordcount: '150'
ht-degree: 0%

---

# [!UICONTROL 単一プロファイル更新 API]

The [!DNL Adobe Target] [!UICONTROL 単一プロファイル更新 API] を使用すると、1 人のユーザーに対するプロファイルの更新を送信できます。 The [!UICONTROL 単一プロファイル更新 API] は、 [!UICONTROL プロファイル一括更新 API]に変更された場合でも、訪問者プロファイルは一度に 1 つずつ更新され、 .cvs ファイルではなく API 呼び出しでインラインで更新されます。

The [!UICONTROL 単一プロファイル更新 API] およびは、通常、実装されていないチャネルで発生するトランザクションに関して更新が必要な場合に使用されます [!DNL Target]. 例えば、何らかのオフラインアクションを実行する 1 人の訪問者のプロファイルを更新するとします。 アクションには、コールセンターへのリーチ、ローンの資金提供、店舗内のロイヤルティカードを使用したキオスクへのアクセスなどが含まれます。

## リソース:

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)
