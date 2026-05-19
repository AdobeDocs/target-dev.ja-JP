---
keywords: 実装，javascript ライブラリ，js, atjs, オンデバイス判定，オンデバイス判定，サポートされている機能，8 ドル
description: '[!UICONTROL on-device decisioning]でサポートされている機能について説明します。'
title: On-Device Decisioningでサポートされている機能
feature: at.js
exl-id: bdd65658-6c4a-41ae-a222-59c00a11bdac
TQID: https://experienceleague.adobe.com/ummFURb6WnrNCbiQNDtzWmtZq05am9CMn9UXL0SPaXo
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 720
ht-degree: 8%

---

# [!UICONTROL on-device decisioning]でサポートされている機能

[!DNL Adobe Target] JS SDKでは、パフォーマンスとデータの鮮度のどちらかを柔軟に選択して意思決定を行うことができます。 言い換えれば、マシンラーニングを通じて最も関連性が高く、魅力的なパーソナライズされたコンテンツを配信することが最も重要な場合は、ライブサーバーコールを送信する必要があります。 しかし、パフォーマンスがより重要な場合は、オンデバイスとメモリ内の決定を行う必要があります。 [!UICONTROL on-device decisioning]が機能するには、サポートされている機能を一覧表示する次の節を参照してください。

## サポートされているアクティビティのタイプ

次の表は、[&#x200B; フォームベースのExperience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=ja)または[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=ja) （VEC）によって作成された[&#x200B; アクティビティタイプ &#x200B;](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html?lang=ja)が、[!UICONTROL on-device decisioning]でサポートされているか、サポートされていないかを示しています。

| アクティビティタイプ | 対応? |
| --- | --- |
| [A/B テスト](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=ja) | ○ |
| [自動配分](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=ja) | × |
| [自動ターゲット &#x200B;](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=ja) ![&#x200B; プレミアム &#x200B;](../../../assets/premium.png) | × |
| [多変量分析テスト](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html?lang=ja) （MVT） | × |
| [エクスペリエンスのターゲット設定](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=ja)（XT） | ○ |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=ja) ![&#x200B; プレミアム &#x200B;](../../../assets/premium.png) | × |
| [おすすめ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=ja) ![&#x200B; プレミアム &#x200B;](../../../assets/premium.png) | × |
| [Analytics for Targetを使用したアクティビティ &#x200B;](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja&) （A4T） | ○ |

## オーディエンスターゲティング

次の表は、[!UICONTROL on-device decisioning]でサポートされているオーディエンスルールまたはサポートされていないオーディエンスルールを示しています。

| オーディエンスルール | 対応? |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=ja) | ○<P>オンデバイス決定を使用する場合、次の位置情報の属性がサポートされます。<ul><li>国 / 地域</li><li>市区町村</li><li>緯度</li><li>経度</li></ul> |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=ja) | × |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=ja) | × |
| [カスタムパラメーター](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=ja) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=ja) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=ja) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=ja) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=ja) | × |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=ja) | × |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=ja) | ○ |
| Adobe Experience Cloud Audiences<P>（[!DNL Audiences from Adobe Analytics]、[!DNL Adobe Audience Manager]、[!DNL Adobe Experience Manager]） | × |

### [!UICONTROL on-device decisioning]の地域ターゲット設定

地域ベースのオーディエンスを使用して[!UICONTROL on-device decisioning]のアクティビティの遅延を最小限に抑えるために、Adobeでは、[getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)への呼び出しで、自分で地域値を指定することをお勧めします。 リクエストのコンテキストでGeo オブジェクトを設定します。 これは、ブラウザから、各訪問者の場所を決定する方法を意味します。 例えば、設定したサービスを使用して、IP-to-Geo ルックアップを実行できます。 Google Cloudなどの一部のホスティングプロバイダーは、各`HttpServletRequest`のカスタムヘッダーを使用してこの機能を提供します。

```javascript {line-numbers="true"}
window.adobe.target.getOffers({ 
    decisioningMethod: "on-device", 
    request: { 
        context: { 
            geo: { 
                city: "SAN FRANCISCO", 
                countryCode: "US", 
                stateCode: "CA", 
                latitude: 37.75, 
                longitude: -122.4 
            } 
        }, 
        execute: { 
            pageLoad: {} 
        } 
    } 
})
```

ただし、サーバーでIPから地域へのルックアップを実行できませんが、それでも地域ベースのオーディエンスを含む[getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)要求に対して[!UICONTROL on-device decisioning]を実行したい場合は、これもサポートされています。 このアプローチの欠点は、リモート IP-to-Geo参照を使用することで、各`getOffers`呼び出しに遅延が追加されることです。 サーバーの近くにあるCDNにヒットするため、サーバーサイドの決定を行う`getOffers`呼び出しよりも待ち時間を短くする必要があります。 SDKに対するリクエストのコンテキストで、Geo オブジェクトの「ipAddress」フィールドのみを指定して、訪問者のIP アドレスの位置情報を取得します。 「ipAddress」以外のフィールドが指定されている場合、[!DNL Target] SDKは解決のために位置情報メタデータを取得しません。

```javascript {line-numbers="true"}
window.adobe.target.getOffers({ 
    decisioningMethod: "on-device", 
    request: { 
        context: { 
            geo: { 
                ipAddress: "127.0.0.1" 
            } 
        }, 
        execute: { 
            pageLoad: {} 
        } 
    } 
})
```

### 配分方法

次の表は、[!UICONTROL on-device decisioning]でサポートされている割り当て方法またはサポートされていない割り当て方法を示しています。

| 配分方法 | 対応? |
| --- | --- |
| 手動 | ○ |
| [最適なエクスペリエンスに自動割り当て](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html?lang=ja) | × |
| [&#x200B; パーソナライズされたエクスペリエンスの自動ターゲティング &#x200B;](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=ja) | × |
