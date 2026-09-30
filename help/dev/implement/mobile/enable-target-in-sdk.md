---
keywords: モバイルアプリ, モバイルアプリ sdk, ターゲットモバイルアプリ, モバイルターゲット sdk, モバイルアプリ sdk, sdk での target の有効化
description: Adobe Mobile Services SDKをモバイルアプリに追加する方法について説明します。
title: '[!DNL Adobe Mobile SDK]で[!DNL Target]を有効にするにはどうすればよいですか？'
feature: Implement Mobile
exl-id: 4263b96a-23c8-4513-8302-00080122181d
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
    internal-label: Target
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
    internal-label: Implementation
subfeature_v2:
  - id: d051910f-2bda-47ea-a969-6ade9fcd71f1
    internal-label: Implement mobile
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
source-git-commit: 5d119ccf18b09b3ba864a69642458597f65c754f
workflow-type: tm+mt
source-wordcount: '303'
ht-degree: 38%
---
# SDKで[!DNL Target]を有効にする

[!UICONTROL Adobe Mobile Services SDK]をアプリに追加します。

>[!IMPORTANT]
>
>[!DNL Adobe Mobile] バージョン 4.*x* SDKのサポートは2021年8月31日（PT）をもって終了し、[!DNL Adobe Target] モバイルユーザーには推奨されなくなりました。
>
>[Adobe Experience Platform SDK モバイル版アプリ ](https://developer.adobe.com/client-sdks/documentation/){target=_blank}は、モバイル アプリで[!DNL Adobe Experience Cloud]のソリューションとサービスを強化するために推奨されるソリューションです。

1. アプリにAdobe Mobile Services SDKをインストールしていない場合は、AnalyticsまたはExperience Cloudの資格情報を使用し、[Adobe Mobile Services](https://mobilemarketing.adobe.com/) web サイトからSDKをダウンロードします。

1. アプリに[!DNL Adobe Mobile Services SDK]を追加します。

   [コア実装とライフサイクル](https://experienceleague.adobe.com/docs/mobile-services/ios/getting-started-ios/dev-qs.html)の説明を参照してください。

1. クライアントコードとタイムアウトを追加し、SSL を有効にします。

   Experience Cloud で、Mobile Services を開き、**[!UICONTROL アプリ設定]**／**[!UICONTROL Target SDK の設定]**&#x200B;に移動します。

   [!DNL Target] クライアントコードとタイムアウトを追加します。 クライアントコードは、それぞれのアカウントまたは会社で一意になります。 タイムアウトは、デフォルトのコンテンツを表示する前に[!DNL Target]が応答を待つまでの秒数です。 Adobe Mobile Services のアプリ設定ページで「**[!UICONTROL HTTPS を使用]**」オプションがチェックされていることを確認します。 HTTPSが有効になっていない場合は、[!DNL Target] サーバーをiOSしない限り、許可リストに加える9以降のすべての呼び出しはブロックされます。

   ![alt画像](assets/mobile-clientcode.png)

1. アプリを作成/配置したら、アプリの設定を見つけて、目的のSDKをダウンロードします。

   ![alt画像](assets/download-sdk.png)

>[!WARNING]
>
> モバイルマーケティングインターフェイスにアクセスできない場合、アプリコードの設定ファイルで直接変更できます。ただし、ユーザーインターフェイスの設定ページと同期されません。
