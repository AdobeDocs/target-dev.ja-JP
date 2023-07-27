---
keywords: at.js 統合, サポートされる統合, サポートされない統合, サードパーティ統合
description: 次の条件を満たす統合（およびサポート対象外）を参照： [!DNL Adobe Target] at.js、を含む [!UICONTROL Analytics for Target] (A4T)、 [!UICONTROL Experience CloudID サービス]など。
title: at.js はどのような統合をサポートしていますか。
feature: at.js
exl-id: d2c61e77-5fc7-4c35-905b-76b8c4f9df4b
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '516'
ht-degree: 64%

---

# at.js の統合

[!DNL Adobe Target] の一般的な統合と at.js のサポート状況について説明します。

サポートされていない統合やここに記載されていない統合を切実に必要としている場合は、担当のアカウント担当者またはコンサルタントにお問い合わせください。

## サポートされる統合

| 統合 | 詳細 |
|--- |--- |
| [!UICONTROL Analytics for Target]（A4T） | 「[Adobe Target のレポートソースとしての Adobe Analytics（A4T）](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html)」を参照してください。 |
| [!UICONTROL Profiles &amp; Audiences] (P&amp;A) | 詳しくは、 [オーディエンス](https://experienceleague.adobe.com/docs/core-services/interface/audiences/audience-library.html?lang=ja) （内） *コアサービスユーザーガイド*. |
| [!UICONTROL Experience Cloud ID サービス] | [Adobe Experience Cloud ID サービスのドキュメント](https://experienceleague.adobe.com/docs/id-service/using/home.html)を参照してください。 |
| [!UICONTROL Adobe Experience Platformのタグ] | [!UICONTROL Adobe Experience Platformのタグ] は、次世代のタグ管理機能です。 [!DNL Adobe]. [!UICONTROL タグは、適切な顧客体験の実現に必要な分析、マーケティングおよび広告タグをデプロイおよび管理するためのシンプルな手段を提供します。]詳しくは、 [実装方法 [!DNL Target] Adobe Experience Platformの使用](../how-to-deployatjs/implement-target-using-adobe-launch.md). |
| [!UICONTROL Adobe Experience Manager] (AEM)CLOUD SERVICE | The [!UICONTROL AEM Cloud Service] 作成を可能にする [!UICONTROL A/B テスト] および [!UICONTROL エクスペリエンスのターゲット設定] 「 」アクティビティをAEMワークフロー内で使用する場合にのみ有効です。 at.js と [!UICONTROL Adobe Experience Manager] 6.2 と FP-11577（またはそれ以降）。 詳しくは、[ との統合 [!DNL Adobe Target]を参照し、対象の AEM バージョンを選択してください。](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja) |
| [!UICONTROL AEM エクスペリエンスフラグメント] | エクスペリエンスフラグメントをAEMの [!DNL Target] アクティビティを使用すると、AEMの使いやすさと機能を、の強力な Automated Intelligence(AI) 機能と機械学習 (ML) 機能と組み合わせることができます。 [!DNL Target] 大規模にエクスペリエンスをテストし、パーソナライズする。  AEM では、パーソナライゼーション戦略に生かせるよう、すべてのコンテンツとアセットが一元化されます。AEM では、コードを記述しなくても、デスクトップ、タブレット、モバイルデバイス向けのコンテンツを 1 か所で簡単に作成できます。デバイスごとにページを作成する必要はありません。AEMは、コンテンツを使用して各エクスペリエンスを自動的に調整します。  「[AEM エクスペリエンスフラグメント](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html)」を参照してください。 |

## サポートされない統合

| 統合 | 詳細 |
|--- |--- |
| レガシー [!DNL Target] から [!DNL SiteCatalyst] 統合 | この統合では、[!DNL SiteCatalyst] UI でレポートを実行できるように、ページ呼び出しを使用してキャンペーンおよびレシピ ID を [!DNL SiteCatalyst] に送信しました。この機能は、A4T によって置き換えられます。 |
| レガシー [!DNL Target] から [!DNL SiteCatalyst] 統合 | この統合では、eVar および prop に基づいて成功指標およびユーザープロファイルを構築できるように、`"SiteCatalyst: Event"` および `"SiteCatalyst: Purchase"` という mbox 呼び出しを作成しました。この機能は、A4T および P&amp;A によって置き換えられます。 |
| レガシー [!DNL Audience Manager] (AAM先 ) [!DNL Target] 統合 | この統合では、フロントエンド API 呼び出しを作成して AAM セグメントを取得し、それらをページの各 mbox 呼び出しの mbox パラメーターとして送信しました。 |

## サードパーティ統合

| 統合 | 詳細 |
|--- |--- |
| 他のタグマネージャー | at.js は、アドビ以外のタグ管理プラットフォームで使用する必要がありますが、他のベンダーが開発したカスタム統合機能の使用には注意してください。そうした統合は、もう at.js には存在しない内部 mbox.js 関数に依存している可能性があります。 |
| サードパーティデータプロバイダー（Demandbase、BlueKai、weather API など） | Target のユーザープロファイリングを補完するために使用される多くのサードパーティデータプロバイダーは、at.js の[データプロバイダー](../atjs-functions/targetglobalsettings.md#data-providers)機能を使用して統合することができます。  |
