---
keywords: 実装，実装，設定，設定，単一プロファイルの更新
description: 単一プロファイル更新 API を使用して  [!DNL Target]  データをに取り込みます。
title: '[!UICONTROL Single Profile Update API] を使用してデータをに取  [!DNL Target]  込むにはどうすればよいですか？'
feature: Implementation
exl-id: e6c394cb-74a3-4991-b656-5ae601f2d5e2
source-git-commit: 946e9431e6bde30f564b4ba1a4cf0a78d8c5c6bf
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 3%

---

# [!UICONTROL Single Profile Update API]

[!DNL Adobe Target] [!UICONTROL Single Profile Update API] を使用すると、1 人のユーザーのプロファイルの更新を送信できます。 [!UICONTROL Single Profile Update API] は [!UICONTROL Bulk Profile Update API] とほぼ同じですが、.cvs ファイルではなく API 呼び出しを使用してインラインで一度に 1 つの訪問者プロファイルが更新されます。

[!UICONTROL Single Profile Update API] とは、通常、[!DNL Target] を実装していないチャネルで発生したトランザクションに関連して更新を行う必要がある場合に使用されます。 例えば、何らかのオフラインアクションを実行する 1 人の訪問者のプロファイルを更新するとします。 コールセンターへのアクセス、ローンの支払い、店舗でのロイヤルティカードの使用、キオスクへのアクセスなどがアクションの例になります。

[!UICONTROL Single Profile Update API] と [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md) を対比しなさい。

## リソース

詳しくは、次を参照してください。

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)
