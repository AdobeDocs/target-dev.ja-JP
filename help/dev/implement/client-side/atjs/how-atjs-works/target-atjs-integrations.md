---
keywords: at.js 統合, サポートされる統合, サポートされない統合, サードパーティ統合
description: '[!UICONTROL Analytics for Target] （A4T）、[!UICONTROL Experience Cloud ID Service]など、 [!DNL Adobe Target] at.jsでサポートされている（サポートされていない）統合をご覧ください。'
title: at.jsはどのような統合をサポートしていますか？
feature: at.js
exl-id: d2c61e77-5fc7-4c35-905b-76b8c4f9df4b
TQID: https://experienceleague.adobe.com/RdcxcIGufo2O5aKPqIAJVINkCzZ1Brcv8EXiX1n4buc
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2:
  - id: df62f171-ac37-440f-8f0f-f41a72ebdd34
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: e6ff21d3-dec6-4298-8590-7c749fffaf78
  - id: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 530
ht-degree: 53%

---

# at.js の統合

[!DNL Adobe Target] の一般的な統合と at.js のサポート状況について説明します。

サポートされていない統合やここに記載されていない統合を切実に必要としている場合は、担当のアカウント担当者またはコンサルタントにお問い合わせください。

## サポートされる統合

| 統合 | 詳細 |
|--- |--- |
| [!UICONTROL Analytics for Target]（A4T） | 「[Adobe Target のレポートソースとしての Adobe Analytics（A4T）](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja)」を参照してください。 |
| [!UICONTROL &#x200B; プロファイルとオーディエンス &#x200B;] （P&amp;A） | *コアサービスユーザーガイド*&#x200B;の[&#x200B; オーディエンス &#x200B;](https://experienceleague.adobe.com/docs/core-services/interface/audiences/audience-library.html?lang=ja)を参照してください。 |
| [!UICONTROL Experience Cloud ID サービス] | [Adobe Experience Cloud ID サービスのドキュメント](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja)を参照してください。 |
| Adobe Experience Platform&rbrack;の[!UICONTROL &#x200B; タグ | Adobe Experience Platform]の[[!UICONTROL &#x200B; タグは、[!DNL Adobe]の次世代型タグ管理機能です。 [!UICONTROL &#x200B; タグ &#x200B;]を使用すると、関連する顧客体験を強化するために必要な分析、マーケティング、広告タグを簡単にデプロイおよび管理できます。 Adobe Experience Platform]](../how-to-deployatjs/implement-target-using-adobe-launch.md)を使用して&lbrack;実装 [!DNL Target] を参照してください。 |
| [!UICONTROL Adobe Experience Manager] （AEM） Cloud Service | [!UICONTROL AEM Cloud Service]を使用すると、AEM ワークフロー内で[!UICONTROL A/B テスト &#x200B;]および[!UICONTROL &#x200B; エクスペリエンスのターゲット設定] アクティビティを作成できます。 FP-11577 （以降）を使用して[!UICONTROL Adobe Experience Manager] 6.2のat.jsをサポートします。 詳しくは、[Integrating with [!DNL Adobe Target]](https://experienceleague.adobe.com/docs/experience-manager-release-information/aem-release-updates/previous-updates/aem-previous-versions.html?lang=ja)を参照し、お使いのAEM バージョンを選択してください。 |
| [!UICONTROL AEM エクスペリエンスフラグメント] | [!DNL Target]のアクティビティでAEMで作成されたエクスペリエンスフラグメントを使用すると、AEMの使いやすさと機能を、[!DNL Target]の強力な自動インテリジェンス（AI）および機械学習（ML）機能と組み合わせて、エクスペリエンスを大規模にテストおよびパーソナライズできます。  AEM では、パーソナライゼーション戦略に生かせるよう、すべてのコンテンツとアセットが一元化されます。 AEM では、コードを記述しなくても、デスクトップ、タブレット、モバイルデバイス向けのコンテンツを 1 か所で簡単に作成できます。 デバイスごとにページを作成する必要はありません。AEM ではコンテンツを基に、各デバイスのエクスペリエンスが自動調整されます。  「[AEM エクスペリエンスフラグメント](https://experienceleague.adobe.com/docs/target/using/experiences/offers/aem-experience-fragments.html?lang=ja)」を参照してください。 |

## サポートされない統合

| 統合 | 詳細 |
|--- |--- |
| レガシー[!DNL Target]から[!DNL SiteCatalyst]への統合 | これは、ページ呼び出しを介してキャンペーン IDとレシピ IDを[!DNL SiteCatalyst]に送信し、[!DNL SiteCatalyst] UIでレポートを実行できるようにするための統合です。 この機能は、A4T によって置き換えられます。 |
| レガシー[!DNL Target]から[!DNL SiteCatalyst]への統合 | この統合では、eVar および prop に基づいて成功指標およびユーザープロファイルを構築できるように、`"SiteCatalyst: Event"` および `"SiteCatalyst: Purchase"` という mbox 呼び出しを作成しました。 この機能は、A4T および P&amp;A によって置き換えられます。 |
| 従来の[!DNL Audience Manager] （AAM）から[!DNL Target]への統合 | この統合では、フロントエンド API 呼び出しを作成して AAM セグメントを取得し、それらをページの各 mbox 呼び出しの mbox パラメーターとして送信しました。 |

## サードパーティ統合

| 統合 | 詳細 |
|--- |--- |
| 他のタグマネージャー | at.js は、アドビ以外のタグ管理プラットフォームで使用する必要がありますが、他のベンダーが開発したカスタム統合機能の使用には注意してください。 そうした統合は、もう at.js には存在しない内部 mbox.js 関数に依存している可能性があります。 |
| サードパーティデータプロバイダー（Demandbase、BlueKai、weather API など） | Targetのユーザープロファイリングを補完するために使用される多くのサードパーティデータプロバイダーは、at.js [&#x200B; データプロバイダー](../atjs-functions/targetglobalsettings.md#data-providers)機能を使用して統合できます。 |
