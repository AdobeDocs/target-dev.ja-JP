---
keywords: 実装， javascript ライブラリ， js, atjs，オンデバイス判定，デバイス判定，サポートされる機能， $8
description: どの機能がサポートされているかを説明します。 [!UICONTROL オンデバイス判定].
title: On-Device Decisioning でサポートされる機能
feature: at.js
exl-id: bdd65658-6c4a-41ae-a222-59c00a11bdac
source-git-commit: 79ffa3f58d780f587fe1202b82d3860395504dfe
workflow-type: tm+mt
source-wordcount: '666'
ht-degree: 10%

---

# 次の機能でサポートされます。 [!UICONTROL オンデバイス判定]

The [!DNL Adobe Target] JS SDK は、お客様がパフォーマンスと決定のためのデータの鮮度を柔軟に選択できるようにします。 つまり、機械学習を通じて最も関連性が高く、パーソナライズされたコンテンツを配信することが最も重要な場合は、ライブサーバー呼び出しをおこなう必要があります。 しかし、パフォーマンスがより重要な場合は、デバイス上およびメモリ内での判断が必要です。 の場合 [!UICONTROL オンデバイス判定] 機能するには、次の節で、サポートされる機能の一覧を参照してください。

## サポートされているアクティビティのタイプ

次の表に、どの [アクティビティタイプ](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) 作成者 [フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html) または [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) (VEC) は、 [!UICONTROL オンデバイス判定].

| アクティビティタイプ | 対応? |
| --- | --- |
| [A/B テスト](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html) | ○ |
| [自動配分](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | × |
| [自動ターゲット](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) ![Premium](../../../assets/premium.png) | × |
| [多変量分析テスト](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html) （MVT） | × |
| [エクスペリエンスのターゲット設定](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html)（XT） | ○ |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html) ![Premium](../../../assets/premium.png) | × |
| [レコメンデーション](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html) ![Premium](../../../assets/premium.png) | × |
| [Analytics for Target を使用するアクティビティ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?) (A4T) | ○ |

## オーディエンスのターゲティング

次の表は、どのオーディエンスルールがサポートされているか、またはサポートされていないかを示しています [!UICONTROL オンデバイス判定].

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
| Adobe Experience Cloud Audiences<P>([!DNL Audiences from Adobe Analytics], [!DNL Adobe Audience Manager], および [!DNL Adobe Experience Manager]) | × |

### の地域ターゲット設定 [!UICONTROL オンデバイス判定]

の待ち時間を最小限に抑える [!UICONTROL オンデバイス判定] アクティビティと地域ベースのオーディエンスの間では、Adobeは、 [getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md). リクエストのコンテキストで地域オブジェクトを設定します。 つまり、ブラウザーを使用して、各訪問者の場所を特定する方法です。 例えば、設定したサービスを使用して、IP-to-Geo ルックアップを実行できます。 Google Cloud など、一部のホスティングプロバイダーは、各 `HttpServletRequest`.

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

ただし、サーバーで IP-to-Geo 検索を実行できず、引き続きを実行したい場合は、 [!UICONTROL オンデバイス判定] 対象： [getOffers](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md) 地域ベースのオーディエンスを含むリクエストもサポートされます。 このアプローチの欠点は、リモートの IP-to-Geo ルックアップを使用し、各 IP-to-Geo ルックアップに遅延が追加される点です `getOffers` を呼び出します。 この遅延は、 `getOffers` サーバーの近くにある CDN をヒットするので、サーバー側判定を使用してを呼び出します。 SDK が訪問者の IP アドレスの地域情報を取得するために、リクエストのコンテキストの地域オブジェクトに「ipAddress」フィールドのみを指定します。 「ipAddress」以外のフィールドを指定した場合、 [!DNL Target] SDK は解決のために位置情報メタデータを取得しません。

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

次の表に、でサポートされている割り当て方法とサポートされていない割り当て方法を示します。 [!UICONTROL オンデバイス判定].

| 配分方法 | 対応? |
| --- | --- |
| 手動 | ○ |
| [最良のエクスペリエンスに自動配分](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | × |
| [パーソナライズされたエクスペリエンスの自動ターゲット](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) | × |
