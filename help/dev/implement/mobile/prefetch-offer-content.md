---
keywords: offer, prefetch, iOS, android, sdk, mobile, mobile sdk, 8 ドル
description: IOSおよびAndroid Mobile SDKの[!DNL Adobe Target]先行取得機能を使用して、サーバー応答をキャッシュすることで、オファーコンテンツを可能な限り数回取得します。
title: モバイルアプリ用のオファーコンテンツを先行取得できますか？
feature: Implement Mobile
exl-id: 6f8e8298-f1e9-46f0-828f-717c7d632077
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
source-wordcount: '318'
ht-degree: 37%
---
# プリフェッチオファーコンテンツ

[!DNL Target] のプリフェッチ機能では、iOS および Android Mobile SDK を使用してサーバーからの応答をキャッシュすることにより、可能な限り少ない回数でオファーコンテンツを取得できます。

>[!IMPORTANT]
>
>[!DNL Adobe Mobile] バージョン 4.*x* SDKのサポートは2021年8月31日（PT）をもって終了し、[!DNL Adobe Target] モバイルユーザーには推奨されなくなりました。
>
>[Adobe Experience Platform SDK モバイル版アプリ ](https://developer.adobe.com/client-sdks/documentation/){target=_blank}は、モバイル アプリで[!DNL Adobe Experience Cloud]のソリューションとサービスを強化するために推奨されるソリューションです。

このプロセスにより、読み込み時間が短縮され、複数のネットワーク呼び出しを防ぎ [!DNL Target] 、モバイルアプリユーザーがmboxを訪問したことを通知できます。 プリフェッチ呼び出し中にすべてのコンテンツが取得され、キャッシュされます。このコンテンツは、指定された mbox 名のキャッシュされたコンテンツを含む今後のすべての呼び出しに対してキャッシュから取得されます。

IOSおよびAndroid Mobile SDKでプリフェッチ方式を使用する場合は、次の制限事項を考慮してください。

* プリフェッチコンテンツは、起動間で保持されません。 プリフェッチコンテンツは、アプリケーションが稼働しているか `clearPrefetchCache()` 、メソッドが呼び出されるまでキャッシュされます。
* プリフェッチ機能は、[!UICONTROL 自動割り当て]および[!UICONTROL 自動ターゲット ]のトラフィック割り当てメソッド、[!UICONTROL Automated Personalization]または[!UICONTROL Recommendations]のアクティビティタイプ、またはA/BまたはXT アクティビティ内の[Recommendations オファー](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations-as-an-offer.html)ではサポートされていません。

プリフェッチメソッド、パブリッククラス、コードサンプルなどについて詳しくは、以下のドキュメントを参照してください。

* **iOS:** [iOSでオファーコンテンツを先行取得](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-mob-target-prefetch-ios.html)、*Mobile Services iOS SDK ヘルプ*&#x200B;で。
* **Android:** [Androidでオファーコンテンツを先行取得](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/c-mob-target-prefetch-android.html)、*Mobile Services Android SDK ヘルプ*&#x200B;で。
