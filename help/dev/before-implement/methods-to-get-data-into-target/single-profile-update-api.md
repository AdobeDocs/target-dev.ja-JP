---
keywords: 実装、実装、設定、設定、単一プロファイルの更新
description: 単一のプロファイル更新APIを使用して [!DNL Target] にデータを取り込みます。
title: '[!UICONTROL Single Profile Update API]を使用して [!DNL Target] にデータを取り込むにはどうすればよいですか？'
feature: Implementation
exl-id: e6c394cb-74a3-4991-b656-5ae601f2d5e2
TQID: https://experienceleague.adobe.com/tkh7YEJ9Vr5eMynNYYxYKZZDOXZTVJNxwUSCxiwPfzI
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 132
ht-degree: 3%

---

# [!UICONTROL Single Profile Update API]

[!DNL Adobe Target] [!UICONTROL Single Profile Update API]では、1人のユーザーに対してプロファイル更新を送信できます。 [!UICONTROL Single Profile Update API]は[!UICONTROL Bulk Profile Update API]とほぼ同じですが、1人の訪問者プロファイルが一度に更新され、.cvs ファイルではなくAPI呼び出しでインライン化されます。

[!UICONTROL Single Profile Update API]とは、[!DNL Target]を実装していないチャネルで発生しているトランザクションに関連して更新を行う必要がある場合に一般的に使用されます。 例えば、オフラインアクションを実行する1人の訪問者のプロファイルを更新するとします。 アクションには、コールセンターへの連絡、ローンの資金調達、店舗でのロイヤルティカードの利用、キオスクへのアクセスなどが含まれます。

[!UICONTROL Single Profile Update API]と[[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)を対比します。

## リソース

詳しくは、次を参照してください。

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)
