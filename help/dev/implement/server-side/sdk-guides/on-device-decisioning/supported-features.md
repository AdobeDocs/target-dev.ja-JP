---
title: オンデバイス判定でサポートされている機能は何ですか？
description: ライブサーバー呼び出しを使用して、機械学習を通じて最も関連性が高く魅力的なパーソナライズされたコンテンツを配信する方法を説明します。
feature: APIs/SDKs
exl-id: 15d9870f-6c58-4da0-bfe5-ef23daf7d273
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '474'
ht-degree: 13%

---

# サポートされる機能の概要

[!DNL Adobe Target] のサーバーサイド SDK を使用すると、開発者は意思決定のためにデータのパフォーマンスと鮮度を柔軟に選択できます。 つまり、最も関連性が高く、魅力的なパーソナライズされたコンテンツを機械学習を通じて配信することが最も重要な場合は、ライブサーバーを呼び出す必要があります。 ただし、パフォーマンスがより重要な場合は、オンデバイスで決定する必要があります。 [!UICONTROL on-device decisioning] の機能については、次のサポートされる機能のリストを参照してください。

* アクティビティのタイプ
* オーディエンスのターゲティング
* 配分方法

## アクティビティのタイプ

次の表に、[ フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) を使用して作成された [ アクティビティタイプ ](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?) が、[!UICONTROL on-device decisioning] でサポートされているかサポートされていないかを示します。

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

次の表に、[!UICONTROL on-device decisioning] でサポートされるオーディエンスルールとサポートされないオーディエンスルールを示します。

| オーディエンスルール | オンデバイス判定 |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | ○ |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | × |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | × |
| [ カスタムパラメーター ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | × |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | × |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | ○ |
| [Experience Cloudオーディエンス ](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) （Adobe Audience Manager、Adobe AnalyticsおよびAdobe Experience Managerのオーディエンス） | × |

### [!UICONTROL on-device decisioning] のジオターゲティング

ジオベースのオーディエンスを使用する [!UICONTROL on-device decisioning] アクティビティの待ち時間をほぼゼロに保つために、Adobeでは `getOffers` の呼び出しでジオバリューを自分で指定することをお勧めします。 これを行うには、リクエストの `Context` に `Geo` オブジェクトを設定します。 つまり、サーバーには、各エンドユーザーの場所を判断する方法が必要になります。 例えば、サーバーは、設定したサービスを使用して IP からジオルックアップを実行する場合があります。 Google Cloud など、一部のホスティングプロバイダーは、各 `HttpServletRequest` ージのカスタムヘッダーを介してこの機能を提供します。

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

ただし、サーバーで IP からジオベースの検索を実行できない場合でも、ジオベースのオーディエンスを含む `getOffers` リクエストに対して [!UICONTROL on-device decisioning] を実行すると便利です。 このアプローチの欠点は、リモートの IP からジオルックアップを使用することで、各 `getOffers` 呼び出しに待ち時間が追加されることです。 この待ち時間は、サーバーの近くにある CDN にヒットするため、リモート `getOffers` 呼び出しよりも短くなる必要があります。 SDK がユーザーの IP アドレスの位置情報を取得するには、リクエストの `Context` に `Geo` オブジェクトの `ipAddress` フィールドのみを指定する必要があります。 `ipAddress` 以外のフィールドが指定された場合、[!DNL Target] SDK は解決のために位置情報メタデータを取得しません。


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

次の表に、[!UICONTROL on-device decisioning] でサポートされる配分方法とサポートされない配分方法を示します。

| 配分方法 | 対応 |
| --- | --- |
| 手動 | ○ |
| [ 最適なエクスペリエンスへの自動配分 ](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | × |
| [ パーソナライズされたエクスペリエンスの自動ターゲット ](https://experienceleague.adobe.com/docs/target/using/activities/auto-target-to-optimize.html) | × |
