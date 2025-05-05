---
keywords: モバイルアプリ，よくある質問，faq, target モバイルアプリ
description: モバイルアプリに関するよくある質問とその回答  [!DNL Adobe Target]  表示します。
title: モバイルアプリに関するよくある質問  [!DNL About Target]  何ですか？
feature: Implement Mobile
exl-id: 06cae3de-83a4-4018-a832-66fb292a1d0f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 1%

---

# モバイルアプリ用 [!DNL Target] に関する FAQ

モバイルアプリの [!DNL Target] についてのよくある質問のリストです。

## SDK をデプロイするには [!DNL Adobe Experience Platform] のタグを使用する必要がありますか。Launch を使用せずに SDK をデプロイできますか。

SDK は [Adobe Marketing Cloud Git](https://github.com/Adobe-Marketing-Cloud/acp-sdks/){target=_blank} で利用できます。 [Adobe Experience Platformのタグ ](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja){target=_blank} を使用しない場合は、独自の設定ファイルを管理し、アプリで管理する必要があります。

## 現在利用可能な SDK はどれですか？

Adobe Experience Platform Mobile SDK は現在、iOS、Androidおよび React をサポートしています。 詳しくは、[Adobe Experience Cloud Platform Mobile SDK ガイド ](https://experienceleague.adobe.com/docs/mobile.html?lang=ja){target=_blank} を参照してください。

## 緯度と経度の検証の観点から、位置ベースの機能の頻度はどれくらいですか？

詳しくは、[Adobe場所のドキュメント ](https://experienceleague.adobe.com/docs/places/using/home.html?lang=ja){target=_blank} を参照してください。

## Adobe Experience Platform Mobile SDK が機能するには at.js が必要ですか。

いいえ。at.js で Mobile SDK を使用する必要はありません。 at.js は、web サイト用の [!DNL Target] JavaScript ライブラリです。 Adobe Experience Platform Mobile SDK は、モバイルアプリ用です。

## [!DNL Target] Mobile は [!DNL Adobe Target] Premium 製品 SKU のみの機能ですか？

いいえ。[!DNL Adobe Target Standard] のお客様は、アドビの Mobile SDK を [!UICONTROL A/B Test] および [!UICONTROL Experience Targeting] （XT）アクティビティに使用できるのは、[!DNL Target Standard] Mobile アプリアドオンを使用する場合のみです。 モバイルアプリで [!UICONTROL Recommendations] または AI を利用した機能を使用する場合は、[Adobe Target Premium](https://experienceleague.adobe.com/docs/target/using/introduction/intro.html?lang=ja#premium) ライセンスが必要です。

## [!DNL Adobe Experience Manager] （AEM）と [!DNL Target] のモバイルアクティビティの間にモバイルアプリの統合はありますか。

現在、JSON[ エクスペリエンスフラグメント ](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html?lang=ja){target=_blank} をAEMから [!DNL Target] に共有でき、モバイルアプリアクティビティで使用する可能性があります。
