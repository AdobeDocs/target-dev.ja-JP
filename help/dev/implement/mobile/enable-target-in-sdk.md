---
keywords: モバイルアプリ, モバイルアプリ sdk, ターゲットモバイルアプリ, モバイルターゲット sdk, モバイルアプリ sdk, sdk での target の有効化
description: モバイルアプリにAdobeMobile Services SDK を追加する方法について説明します。
title: 有効にする方法 [!DNL Target] （内） [!DNL Adobe Mobile SDK]?
feature: Implement Mobile
exl-id: 4263b96a-23c8-4513-8302-00080122181d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '295'
ht-degree: 49%

---

# 有効にする [!DNL Target] （SDK 内）

次を追加： [!UICONTROL AdobeMobile Services SDK] をアプリに追加します。

>[!IMPORTANT]
>
>のサポート [!DNL Adobe Mobile] バージョン 4.*x* SDK は 2021 年 8 月 31 日をもって終了しており、お勧めしません。 [!DNL Adobe Target] モバイルユーザー。
>
>The [モバイルアプリ用Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/){target=_blank} は、を強化するための推奨されるソリューションです。 [!DNL Adobe Experience Cloud] ソリューションおよびサービスをモバイルアプリに追加できます。

1. Adobe Mobile Services SDK をアプリにインストールしていない場合は、Analytics または Experience Cloud の資格情報を使用して、 [Adobe Mobile Services](https://mobilemarketing.adobe.com/) の Web サイトから SDK をダウンロードします。

1. 次を追加： [!DNL Adobe Mobile Services SDK] をアプリに追加します。

   [コア実装とライフサイクル](https://experienceleague.adobe.com/docs/mobile-services/ios/getting-started-ios/dev-qs.html)の説明を参照してください。

1. クライアントコードとタイムアウトを追加し、SSL を有効にします。

   Experience Cloud で、Mobile Services を開き、**[!UICONTROL アプリ設定]**／**[!UICONTROL Target SDK の設定]**&#x200B;に移動します。

   を追加します。 [!DNL Target] clientcode と timeout。 クライアントコードは、それぞれのアカウントまたは会社で一意になります。タイムアウトは、が終了するまでの時間（秒数）です。 [!DNL Target] は、デフォルトコンテンツを表示する前に応答を待ちます。 Adobe Mobile Services のアプリ設定ページで「**[!UICONTROL HTTPS を使用]**」オプションがチェックされていることを確認します。HTTPS が有効になっていない場合、 iOS9+のすべての呼び出しは、 [!DNL Target] サーバー。

   ![代替画像](assets/mobile-clientcode.png)

1. アプリを作成、設置した後、アプリ設定を探し、目的の SDK をダウンロードします。

   ![代替画像](assets/download-sdk.png)

>[!WARNING]
>
> モバイルマーケティングインターフェイスにアクセスできない場合、アプリコードの設定ファイルで直接変更できます。ただし、ユーザーインターフェイスの設定ページと同期されません。
