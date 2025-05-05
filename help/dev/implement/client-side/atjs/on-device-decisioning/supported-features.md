---
keywords: 実装，javascript ライブラリ，js, atjs, オンデバイス判定，オンデバイス判定，サポートされる機能，$8
description: '[!UICONTROL on-device decisioning] でサポートされている機能について説明します。'
title: オンデバイス判定でサポートされる機能
feature: at.js
exl-id: bdd65658-6c4a-41ae-a222-59c00a11bdac
source-git-commit: 79ffa3f58d780f587fe1202b82d3860395504dfe
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 12%

---

# [!UICONTROL on-device decisioning] でサポートされる機能

[!DNL Adobe Target] JS SDK を使用すると、意思決定のためのデータのパフォーマンスと鮮度を柔軟に選択できます。 つまり、最も関連性が高く、魅力的なパーソナライズされたコンテンツを機械学習を通じて配信することが最も重要な場合は、ライブサーバーを呼び出す必要があります。 ただし、パフォーマンスがより重要な場合は、オンデバイスおよびメモリ内の決定を行う必要があります。 [!UICONTROL on-device decisioning] の機能については、次の節でサポートされる機能の一覧を参照してください。

## サポートされているアクティビティのタイプ

次の表は、[&#128279;](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) フォームベースの Experience Composer[&#128279;](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) または [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html) （VEC）で作成された  アクティビティタイプ  が、[!UICONTROL on-device decisioning] でサポートされている（またはサポートされていない）ことを示しています。

| アクティビティタイプ | 対応? |
| --- | --- |
| [A/B テスト](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html) | ○ |
| [自動配分](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | × |
| [ 自動ターゲット ](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html)![Premium](../../../assets/premium.png) | × |
| [多変量分析テスト](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html) （MVT） | × |
| [エクスペリエンスのターゲット設定](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html)（XT） | ○ |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html) ![Premium](../../../assets/premium.png) | × |
| [Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html) ![Premium](../../../assets/premium.png) | × |
| [Analytics for Target を使用したアクティビティ ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?) （A4T） | ○ |

## オーディエンスのターゲティング

次の表に、[!UICONTROL on-device decisioning] でサポートされるオーディエンスルールとサポートされないオーディエンスルールを示します。

| オーディエンスルール | 対応? |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | ○<P>オンデバイス判定を使用する場合、次の地域属性がサポートされます。<ul><li>国 / 地域</li><li>市区町村</li><li>緯度</li><li>経度</li></ul> |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | × |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | × |
| [カスタムパラメーター](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | × |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | × |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | ○ |
| Adobe Experience Cloud オーディエンス<P>（[!DNL Audiences from Adobe Analytics]、[!DNL Adobe Audience Manager]、[!DNL Adobe Experience Manager]） | × |

### [!UICONTROL on-device decisioning] のジオターゲティング

ジオベースのオーディエンスを使用した [!UICONTROL on-device decisioning] アクティビティの待ち時間を最小限に抑えるために、Adobeでは [getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md) の呼び出しでジオバリューを自分で指定することをお勧めします。 リクエストのコンテキストで Geo オブジェクトを設定します。 つまり、ブラウザーから各訪問者の場所を特定します。 例えば、設定したサービスを使用して IP からジオへのルックアップを実行できます。 Google Cloud など、一部のホスティングプロバイダーは、各 `HttpServletRequest` ージのカスタムヘッダーを介してこの機能を提供します。

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

ただし、サーバーで IP からジオベースの検索を実行できなくても、ジオベースのオーディエンスを含む [getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md) リクエストに対して [!UICONTROL on-device decisioning] を実行する場合は、これもサポートされます。 このアプローチの欠点は、リモートの IP からジオルックアップを使用することで、`getOffers` 呼び出しごとに待ち時間が追加されることです。 この待ち時間は、サーバー側の決定を使用した `getOffers` 呼び出しよりも短い必要があります。これは、サーバーの近くにある CDN にヒットするためです。 SDK へのリクエストのコンテキストで、地域オブジェクトの「ipAddress」フィールドのみを指定して、訪問者の IP アドレスの地域 – 場所を取得します。 「ipAddress」に加えてその他のフィールドが指定された場合、[!DNL Target] SDK は解決のために位置情報メタデータを取得しません。

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

次の表に、[!UICONTROL on-device decisioning] でサポートされる配分方法とサポートされない配分方法を示します。

| 配分方法 | 対応? |
| --- | --- |
| 手動 | ○ |
| [ 最適なエクスペリエンスへの自動配分 ](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | × |
| [ パーソナライズされたエクスペリエンスの自動ターゲット ](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) | × |
