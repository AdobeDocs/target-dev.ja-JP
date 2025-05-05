---
keywords: オファー，プリフェッチ，iOS, android, sdk, モバイル，モバイル sdk, 8 ドル
description: iOSおよびAndroid Mobile SDK の  [!DNL Adobe Target] prefetch 機能を使用して、サーバー応答をキャッシュすることで、オファーコンテンツを可能な限り数回だけ取得します。
title: モバイルアプリのオファーコンテンツをプリフェッチできますか？
feature: Implement Mobile
exl-id: 6f8e8298-f1e9-46f0-828f-717c7d632077
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 43%

---

# プリフェッチオファーコンテンツ

[!DNL Target] のプリフェッチ機能では、iOS および Android Mobile SDK を使用してサーバーからの応答をキャッシュすることにより、可能な限り少ない回数でオファーコンテンツを取得できます。

>[!IMPORTANT]
>
>[!DNL Adobe Mobile] バージョン 4 のサポート。*x* SDK は 2021 年 8 月 31 日（PT）をもって終了し、[!DNL Adobe Target] モバイルユーザーには推奨されなくなりました。
>
>[ モバイルアプリ用Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/){target=_blank} は、モバイルアプリで [!DNL Adobe Experience Cloud] のソリューションおよびサービスを強化するための推奨ソリューションです。

このプロセスにより、読み込み時間が短縮され、複数のネットワーク呼び出しを防ぎ [!DNL Target] 、モバイルアプリユーザーがmboxを訪問したことを通知できます。プリフェッチ呼び出し中にすべてのコンテンツが取得され、キャッシュされます。このコンテンツは、指定された mbox 名のキャッシュされたコンテンツを含む今後のすべての呼び出しに対してキャッシュから取得されます。

iOSおよびAndroid Mobile SDK でプリフェッチメソッドを使用する場合は、次の制限事項を考慮してください。

* プリフェッチコンテンツは、起動間で保持されません。プリフェッチコンテンツは、アプリケーションが稼働しているか `clearPrefetchCache()` 、メソッドが呼び出されるまでキャッシュされます。
* プリフェッチ機能は、[!UICONTROL Auto-Allocate] および [!UICONTROL Auto-Target] のトラフィック割り当て方法、[!UICONTROL Automated Personalization] または [!UICONTROL Recommendations] のアクティビティタイプ、または [A/B または XT アクティビティ内の Recommendations オファー ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-as-an-offer.html?lang=ja) ではサポートされていません。

プリフェッチメソッド、パブリッククラス、コードサンプルなどについて詳しくは、以下のドキュメントを参照してください。

* **iOS:**&#x200B;[Mobile Services iOS SDK ヘルプ ](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-mob-target-prefetch-ios.html) の *iOSでのオファーコンテンツのプリフェッチ*。
* **Android:**&#x200B;[Mobile Services Android SDK ヘルプ ](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html) の *Androidでのオファーコンテンツのプリフェッチ*。
