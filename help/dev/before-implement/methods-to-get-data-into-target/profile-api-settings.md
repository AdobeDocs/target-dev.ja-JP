---
keywords: 実装，api, プロファイル，プロファイル api設定，認証トークン
description: ' [!DNL Adobe Target] APIを介したバッチ更新の認証を設定し、プロファイル認証トークンを生成する方法を説明します。'
title: プロファイル API設定を使用してバッチ更新を有効または無効にするにはどうすればよいですか？
feature: APIs/SDKs
exl-id: 968f33d0-296b-4248-8c9a-8e6f3077bdfa
TQID: https://experienceleague.adobe.com/-KYSphaCrm0ICK7g92v9x-uK--nwirs4-DWBR3G5rTM
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 347
ht-degree: 33%

---

# プロファイル API 設定

[!DNL Adobe Target] APIを介したバッチ更新の認証を有効または無効にし、プロファイル認証トークンを生成します。

[!DNL Adobe Target]は、個々のユーザーごとにプロファイルを作成および管理します。 このプロファイルは[!DNL Target] エッジクラスターに保存され、訪問ごとにリアルタイムで更新されます。 プロファイルは個別に更新することも、API経由で一括で更新することもできます。

セキュリティ強化のために、バルク更新 API を呼び出す際にリクエストのヘッダーで有効なアクセストークンを渡すことを要求するよう設定できます。

**認証を必要とし、[!DNL Target] UIを使用してアクセストークンを生成するには：**

1. **[!UICONTROL Administration]**／**[!UICONTROL Implementation]**&#x200B;をクリックします。
1. **[!UICONTROL Profile API]**&#x200B;の下で、**[!UICONTROL Require Authentication]**&#x200B;の切り替えを有効または無効にする位置にスライドします。

   ![alt画像](assets/profile_api_settings.png)

1. （条件付き）認証要件を有効にした場合は、**[!UICONTROL Generate New Profile Authentication Token]**&#x200B;をクリックします。

   ![alt画像](assets/profile_api_settings_2.png)

   トークンの有効期限は、「期限切れ」ボックスにリストされている時間に従って切れます。

   認証トークンを生成するには、次のいずれかのユーザー権限が必要です。

   * 管理者の役割または少なくとも承認者権限を持つ

     Target Standard ユーザーの詳細については、*ユーザー*&#x200B;の[役割と権限の指定](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/users/user-management.html?lang=ja#roles-permissions)を参照してください。 [!DNL Target Premium]ユーザーの詳細については、[エンタープライズ権限の設定](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html?lang=ja)を参照してください。

   * ワークスペース／製品プロファイルレベルでの管理者の役割

     Workspaces は[!DNL Target Premium]のお客様のみが利用できます。 詳しくは、 [エンタープライズの権限の設定](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html?lang=ja)を参照してください。

   * [!DNL Adobe Target]製品レベルの管理者権限（Sysadmin 権限）

API を使用してプロファイル認証トークンを生成もできます。 詳しくは、[Adobe Target管理者およびプロファイル API ガイド &#x200B;](../../administer/admin-api/admin-api-overview-new.md)の「プロファイル」を参照してください。

1. トークンをコピーし、「Authorization」 : 「Bearer」の形式でリクエストのヘッダーに含めます。

1. **[!UICONTROL Generate New Profile Authentication Token]**&#x200B;をクリックして、必要に応じてトークンを再生成します。

>[!WARNING]
>
>このトークンをリセットすると、現在のトークンを使用した API 呼び出しに失敗します。 その場合は、このトークンを使用するすべてのスクリプトとアプリを更新する必要があります。
