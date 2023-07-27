---
keywords: モバイルアプリ，よくある質問， faq, target モバイルアプリ
description: に関するよくある質問とその回答を表示します。 [!DNL Adobe Target] （モバイルアプリ用）
title: よくある質問 [!DNL About Target] モバイルアプリ用？
feature: Implement Mobile
exl-id: 06cae3de-83a4-4018-a832-66fb292a1d0f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '300'
ht-degree: 5%

---

# [!DNL Target]モバイルアプリに関する FAQ

に関するよくある質問のリスト [!DNL Target] （モバイルアプリ用）

## でタグを使用する必要がある [!DNL Adobe Experience Platform] をクリックして SDK をデプロイしますか？それとも、Launch を使用せずに SDK をデプロイできますか？

SDK は、 [Adobe Marketing Cloud git](https://github.com/Adobe-Marketing-Cloud/acp-sdks/){target=_blank}. If you don't use [tags in Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/tags/home.html?lang=ja){target=_blank}を使用する場合は、独自の設定ファイルを管理し、アプリで管理する必要があります。

## 現在利用可能な SDK を教えてください。

Adobe Experience Platform Mobile SDK は、現在、iOS、Android および React をサポートしています。 詳しくは、 [Adobe Experience Cloud Platform Mobile SDK ガイド](https://experienceleague.adobe.com/docs/mobile.html?lang=ja){target=_blank}.

## 緯度と経度に関する検証の観点から、位置ベースの機能の頻度はどれくらいですか？

詳しくは、 [AdobePlaces ドキュメント](https://experienceleague.adobe.com/docs/places/using/home.html){target=_blank} を参照してください。

## Adobe Experience Platform Mobile SDK を動作させるには at.js が必要ですか？

いいえ。モバイル SDK を使用するのに at.js は必要ありません。 at.js は [!DNL Target] Web サイト用の JavaScript ライブラリ。 Adobe Experience Platform Mobile SDK は、モバイルアプリ用です。

## 次に該当 [!DNL Target] の機能のモバイル化 [!DNL Adobe Target] Premium 製品 SKU のみ？

いいえ。の場合 [!DNL Adobe Target Standard] のお客様は、Mobile SDK を [!UICONTROL A/B テスト] および [!UICONTROL エクスペリエンスのターゲット設定] (XT) アクティビティを [!DNL Target Standard] モバイルアプリのアドオン。 を使用する場合は、 [!UICONTROL Recommendations] または、AI を利用したモバイルアプリの機能には、 [Adobe Target Premium](https://experienceleague.adobe.com/docs/target/using/introduction/intro.html#premium) ライセンス。

## 次の間にモバイルアプリ統合があるか [!DNL Adobe Experience Manager] (AEM) および [!DNL Target] モバイルアクティビティ？

現在、JSON を共有できます [エクスペリエンスフラグメント](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html){target=_blank} AEMから [!DNL Target] その後、モバイルアプリアクティビティで使用する可能性があります。
