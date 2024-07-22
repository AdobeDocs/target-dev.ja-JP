---
keywords: 実装，api, プロファイル，プロファイル api 設定，認証トークン
description: ' [!DNL Adobe Target] API を介してバッチ更新の認証を設定し、プロファイル認証トークンを生成する方法について説明します。'
title: プロファイル API 設定を使用してバッチ更新を有効または無効にする方法を教えてください。
feature: APIs/SDKs
exl-id: 968f33d0-296b-4248-8c9a-8e6f3077bdfa
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '316'
ht-degree: 28%

---

# プロファイル API 設定

[!DNL Adobe Target] API を使用したバッチ更新の認証を有効または無効にして、プロファイル認証トークンを生成します。

[!DNL Adobe Target] は、個々のユーザーごとにプロファイルを作成および管理します。 このプロファイルは [!DNL Target] エッジクラスターに保存され、訪問のたびにリアルタイムで更新されます。 また、プロファイルを個別にまたは一括で更新することもできます。

セキュリティを強化するために、Bulk Update API 呼び出しで、有効なアクセストークンをリクエストのヘッダーで渡す必要があります。

**認証を要求し、[!DNL Target] UI を使用してアクセストークンを生成するには、次のようにします。**

1. **[!UICONTROL Administration]**／**[!UICONTROL Implementation]**&#x200B;をクリックします。
1. スライド **[!UICONTROL Profile API]** 下で、**[!UICONTROL Require Authentication]** の切り替えを有効または無効の位置に切り替えます。

   ![alt 画像 ](assets/profile_api_settings.png)

1. （条件付き）認証要件を有効にした場合は、「**[!UICONTROL Generate New Profile Authentication Token]**」をクリックします。

   ![alt 画像 ](assets/profile_api_settings_2.png)

   トークンの有効期限は、「有効期限」ボックスに表示されている時間によって決まります。

   認証トークンを生成するには、次のいずれかのユーザー権限が必要です。

   * 管理者の役割または少なくとも承認者の権限がある

     Target Standardのお客様の詳細については、「*ユーザー* の [ 役割と権限の指定 ](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/users/user-management.html#roles-permissions) を参照してください。 [!DNL Target Premium]ユーザーの詳細については、[エンタープライズ権限の設定](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html)を参照してください。

   * ワークスペース／製品プロファイルレベルでの管理者の役割

     Workspaces は[!DNL Target Premium]のお客様のみが利用できます。詳しくは、 [エンタープライズの権限の設定](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html)を参照してください。

   * [!DNL Adobe Target]製品レベルの管理者権限（Sysadmin 権限）

API を使用してプロファイル認証トークンを生成もできます。詳しくは、[Adobe Target管理者およびプロファイル API ガイド ](../../administer/admin-api/admin-api-overview-new.md) の「プロファイル」を参照してください。

1. トークンをコピーし、「Authorization」 : 「Bearer」の形式でリクエストのヘッダーに含めます。

1. 必要に応じて、「**[!UICONTROL Generate New Profile Authentication Token]**」をクリックしてトークンを再生成します。

>[!WARNING]
>
>このトークンをリセットすると、現在のトークンを使用した API 呼び出しに失敗します。その場合は、このトークンを使用するすべてのスクリプトとアプリを更新する必要があります。
