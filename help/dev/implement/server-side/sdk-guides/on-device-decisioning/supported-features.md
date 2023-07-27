---
title: オンデバイス判定でサポートされる機能は何ですか？
description: ライブサーバーコールを使用して、機械学習を通じて最も関連性の高い魅力的なパーソナライズされたコンテンツを配信する方法を説明します。
feature: APIs/SDKs
exl-id: 15d9870f-6c58-4da0-bfe5-ef23daf7d273
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '657'
ht-degree: 10%

---

# サポートされる機能の概要

[!DNL Adobe Target]のサーバー側 SDK は、デベロッパーが、パフォーマンスと決定のためのデータの鮮度を柔軟に選択できるようにします。 つまり、機械学習を通じて最も関連性が高く、パーソナライズされたコンテンツを配信することが最も重要な場合は、ライブサーバー呼び出しをおこなう必要があります。 ただし、パフォーマンスがより重要な場合は、デバイス上での判断が必要です。 の場合 [!UICONTROL オンデバイス判定] 機能するには、次のサポートされる機能のリストを参照してください。

* アクティビティのタイプ
* オーディエンスのターゲティング
* 配分方法

## アクティビティのタイプ

次の表に、どの [アクティビティタイプ](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) を使用して作成 [フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?) は、サポートされているか、サポートされていません [!UICONTROL オンデバイス判定].

| アクティビティタイプ | 対応 |
| --- | --- |
| [A/B テスト](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html) | ○ |
| [自動配分](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | × |
| [自動ターゲット](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html) | × |
| [Analytics for Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html)（A4T） | ○ |
| [多変量分析テスト](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html) （MVT） | × |
| [エクスペリエンスのターゲット設定](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html)（XT） | ○ |
| [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)（AP） | × |
| [レコメンデーション](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html) | × |


## オーディエンスのターゲティング

次の表は、どのオーディエンスルールがサポートされているか、またはサポートされていないかを示しています [!UICONTROL オンデバイス判定].

| オーディエンスルール | オンデバイス判定 |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | ○ |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | × |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | × |
| [カスタムパラメーター](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | × |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | × |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | ○ |
| [Experience Cloudオーディエンス](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) (Adobe Audience Manager、Adobe Analytics、Adobe Experience Managerのオーディエンス | × |

### の地域ターゲット設定 [!UICONTROL オンデバイス判定]

のほぼゼロの遅延を維持するために [!UICONTROL オンデバイス判定] アクティビティと地域ベースのオーディエンスの間では、Adobeは、 `getOffers`. これをおこなうには、 `Geo` オブジェクトを `Context` リクエストの。 つまり、サーバーは各エンドユーザーの場所を判断する方法が必要になります。 例えば、サーバーが、設定したサービスを使用して IP-to-Geo ルックアップを実行する場合があります。 Google Cloud など、一部のホスティングプロバイダーは、各 `HttpServletRequest`.

>[!BEGINTABS]

>[!TAB Node.js]

```csharp {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
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

>[!TAB Java]

```javascript {line-numbers="true"}
public class TargetRequestUtils {

    public static Context getContext(HttpServletRequest request) {
        Context context = new Context()
            .geo(ipToGeoLookup(request.getRemoteAddr()))
            .channel(ChannelType.WEB)
            .timeOffsetInMinutes(330.0)
            .address(getAddress(request));
        return context;
    }

    public static Geo ipToGeoLookup(String ip) {
        GeoResult geoResult = geoLookupService.lookup(ip);
        return new Geo()
            .city(geoResult.getCity())
            .stateCode(geoResult.getStateCode())
            .countryCode(geoResult.getCountryCode());
    }

}
```

>[!ENDTABS]

ただし、お使いのサーバーで IP-to-Geo 検索を実行する機能を持っていないのに、 [!UICONTROL オンデバイス判定] 対象： `getOffers` 地域ベースのオーディエンスを含むリクエストもサポートされます。 このアプローチの欠点は、リモートの IP-to-Geo ルックアップを使用し、各 IP-to-Geo ルックアップに遅延が追加されることです `getOffers` を呼び出します。 この待ち時間はリモートの待ち時間よりも短くする必要があります `getOffers` を呼び出します。サーバーの近くにある CDN をヒットするからです。 指定できるのは `ipAddress` フィールド `Geo` オブジェクトを `Context` を取得する必要があります。 他のフィールド ( `ipAddress` が指定されている場合、 [!DNL Target] SDK は解決のために位置情報メタデータを取得しません。


>[!BEGINTABS]

>[!TAB Node.js]

```csharp {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    decisioningMethod: "on-device"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
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

>[!TAB Java]

```javascript {line-numbers="true"}
public class TargetRequestUtils {

    public static Context getContext(HttpServletRequest request) {
        Context context = new Context()
            .geo(new Geo().ipAddress(request.getRemoteAddr()))
            .channel(ChannelType.WEB)
            .timeOffsetInMinutes(330.0)
            .address(getAddress(request));
        return context;
    }

}
```

>[!ENDTABS]

## 配分方法

次の表に、でサポートされている割り当て方法とサポートされていない割り当て方法を示します。 [!UICONTROL オンデバイス判定].

| 配分方法 | 対応 |
| --- | --- |
| 手動 | ○ |
| [最良のエクスペリエンスに自動配分](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | × |
| [パーソナライズされたエクスペリエンスの自動ターゲット](https://experienceleague.adobe.com/docs/target/using/activities/auto-target-to-optimize.html) | × |
