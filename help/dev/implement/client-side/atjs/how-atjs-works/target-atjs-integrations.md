---
keywords: at.js 統合, サポートされる統合, サポートされない統合, サードパーティ統合
description: '[!UICONTROL Analytics for Target] （A4T）、[!UICONTROL Experience Cloud ID Service] など、at [!DNL Adobe Target] js でサポートされている（およびサポートされていない）統合を参照してください。'
title: at.js はどのような統合をサポートしますか？
feature: at.js
exl-id: d2c61e77-5fc7-4c35-905b-76b8c4f9df4b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 52%

---

# at.js の統合

[!DNL Adobe Target] の一般的な統合と at.js のサポート状況について説明します。

サポートされていない統合やここに記載されていない統合を切実に必要としている場合は、担当のアカウント担当者またはコンサルタントにお問い合わせください。

## サポートされる統合

| 統合 | 詳細 |
|--- |--- |
| [!UICONTROL Analytics for Target] （A4T） | 「[Adobe Target のレポートソースとしての Adobe Analytics（A4T）](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html)」を参照してください。 |
| [!UICONTROL Profiles & Audiences] （P&amp;A） | [ コアサービスユーザーガイド ](https://experienceleague.adobe.com/docs/core-services/interface/audiences/audience-library.html?lang=ja) の *オーディエンス* を参照してください。 |
| [!UICONTROL Experience Cloud ID Service] | [Adobe Experience Cloud ID サービスのドキュメント](https://experienceleague.adobe.com/docs/id-service/using/home.html)を参照してください。 |
| [!UICONTROL Tags in Adobe Experience Platform] | [!DNL Adobe] の次世代タグ管理機能は [!UICONTROL Tags in Adobe Experience Platform] のとおりです。 関連 [!UICONTROL Tags] る顧客体験を強化するために必要な分析、マーケティングおよび広告タグをデプロイおよび管理するためのシンプルな方法を顧客に提供します。 [Adobe Experience Platformを使用した実装  [!DNL Target]  を参照してください ](../how-to-deployatjs/implement-target-using-adobe-launch.md)。 |
| [!UICONTROL Adobe Experience Manager] （AEM）Cloud Service | [!UICONTROL AEM Cloud Service] を使用すると、AEM ワークフロー内に [!UICONTROL A/B Test] アクティビティと [!UICONTROL Experience Targeting] アクティビティを作成できます。 FP-11577 （またはそれ以降）を使用する [!UICONTROL Adobe Experience Manager] 6.2 での at.js をサポート。 詳しくは、[ との統合  [!DNL Adobe Target]](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja) を参照し、AEMのバージョンを選択します。 |
| [!UICONTROL AEM Experience Fragments] | AEMで作成したエクスペリエンスフラグメントを [!DNL Target] アクティビティで使用すると、AEMの使いやすさと機能を、強力な自動インテリジェンス（AI）機能および機械学習（ML）機能と組み合わせて、エクスペリエンスを大規模にテストおよびパーソナライズで [!DNL Target] ます。  AEM では、パーソナライゼーション戦略に生かせるよう、すべてのコンテンツとアセットが一元化されます。AEM では、コードを記述しなくても、デスクトップ、タブレット、モバイルデバイス向けのコンテンツを 1 か所で簡単に作成できます。デバイスごとにページを作成する必要はありません。AEMはコンテンツを使用して、各デバイスのエクスペリエンスを自動調整します。  「[AEM エクスペリエンスフラグメント](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html)」を参照してください。 |

## サポートされない統合

| 統合 | 詳細 |
|--- |--- |
| 従来の [!DNL Target] から [!DNL SiteCatalyst] への統合 | これは、[!DNL SiteCatalyst] UI でレポートを実行できるように、ページ呼び出しを介して [!DNL SiteCatalyst] にキャンペーン ID とレシピ ID を送信した統合でした。 この機能は、A4T によって置き換えられます。 |
| 従来の [!DNL Target] から [!DNL SiteCatalyst] への統合 | この統合では、eVar および prop に基づいて成功指標およびユーザープロファイルを構築できるように、`"SiteCatalyst: Event"` および `"SiteCatalyst: Purchase"` という mbox 呼び出しを作成しました。この機能は、A4T および P&amp;A によって置き換えられます。 |
| 従来の [!DNL Audience Manager] （AAM）から [!DNL Target] への統合 | この統合では、フロントエンド API 呼び出しを作成して AAM セグメントを取得し、それらをページの各 mbox 呼び出しの mbox パラメーターとして送信しました。 |

## サードパーティ統合

| 統合 | 詳細 |
|--- |--- |
| 他のタグマネージャー | at.js は、アドビ以外のタグ管理プラットフォームで使用する必要がありますが、他のベンダーが開発したカスタム統合機能の使用には注意してください。そうした統合は、もう at.js には存在しない内部 mbox.js 関数に依存している可能性があります。 |
| サードパーティデータプロバイダー（Demandbase、BlueKai、weather API など） | Target のユーザープロファイルを補完するために使用される多くのサードパーティデータプロバイダーは、at.js[ データプロバイダー ](../atjs-functions/targetglobalsettings.md#data-providers) 機能を使用して統合できます。 |
