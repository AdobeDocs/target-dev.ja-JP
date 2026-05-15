---
title: オンデバイス判定でサポートされている機能は何ですか？
description: ライブサーバーコールを使用して、マシンラーニングを通じて最も関連性が高く、魅力的なパーソナライズされたコンテンツを配信する方法を説明します。
feature: APIs/SDKs
exl-id: 15d9870f-6c58-4da0-bfe5-ef23daf7d273
TQID: https://experienceleague.adobe.com/Fgwu8Nh90i-tS1aCUVmQReGz6DYBP2GHdcNM7z17BSk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: eb30f47f-d87a-400f-8f78-63ce7979ff56
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 710
ht-degree: 9%

---

# サポートされる機能の概要

[!DNL Adobe Target]のサーバーサイド SDKにより、開発者はパフォーマンスと意思決定のためのデータの鮮度のどちらかを柔軟に選択できます。 言い換えれば、マシンラーニングを通じて最も関連性が高く、魅力的なパーソナライズされたコンテンツを配信することが最も重要な場合は、ライブサーバーコールを送信する必要があります。 しかし、パフォーマンスがより重要な場合は、デバイス上で決定する必要があります。 [!UICONTROL on-device decisioning]が機能するには、サポートされている機能の次の一覧を参照してください。

* アクティビティのタイプ
* Audience Targeting
* 配分方法

## アクティビティのタイプ

次の表は、[&#x200B; フォームベースのExperience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?)を使用して作成された[&#x200B; アクティビティタイプ &#x200B;](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html)が、[!UICONTROL on-device decisioning]でサポートされているか、サポートされていないかを示しています。

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


## オーディエンスターゲティング

次の表は、[!UICONTROL on-device decisioning]でサポートされているオーディエンスルールまたはサポートされていないオーディエンスルールを示しています。

| オーディエンスルール | オンデバイス判定 |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | ○ |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | × |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | × |
| [&#x200B; カスタムパラメーター](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | × |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | × |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | ○ |
| [Experience Cloud Audiences](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) （Adobe Audience Manager、Adobe Analytics、Adobe Experience Managerのオーディエンス） | × |

### [!UICONTROL on-device decisioning]の地域ターゲット設定

地域ベースのオーディエンスを使用する[!UICONTROL on-device decisioning] アクティビティの遅延をほぼゼロに近く維持するために、Adobeでは、`getOffers`への呼び出しで地理値を指定することをお勧めします。 これを行うには、リクエストの`Context`で`Geo` オブジェクトを設定します。 つまり、サーバーには、各エンドユーザーの場所を決定する方法が必要になります。 例えば、設定したサービスを使用して、サーバーがIPから地域へのルックアップを実行する場合があります。 Google Cloudなどの一部のホスティングプロバイダーは、各`HttpServletRequest`のカスタムヘッダーを使用してこの機能を提供します。

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

ただし、サーバーでIPから地域への検索を実行する機能がないものの、それでも地域ベースのオーディエンスを含む`getOffers`要求に対して[!UICONTROL on-device decisioning]を実行したい場合は、これもサポートされます。 このアプローチの欠点は、リモート IP-to-Geo ルックアップを使用することで、各`getOffers`呼び出しに遅延が追加されることです。 サーバーの近くにあるCDNにヒットするため、この待ち時間はリモート `getOffers`呼び出しよりも低くしてください。 SDKがユーザーのIP アドレスの位置情報を取得するには、リクエストの`Context`の`Geo` オブジェクトの`ipAddress` フィールドのみを指定する必要があります。 `ipAddress`以外のフィールドが指定されている場合、[!DNL Target] SDKは解決のために位置情報メタデータを取得しません。


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

次の表は、[!UICONTROL on-device decisioning]でサポートされている割り当て方法またはサポートされていない割り当て方法を示しています。

| 配分方法 | 対応 |
| --- | --- |
| 手動 | ○ |
| [最適なエクスペリエンスに自動割り当て](https://experienceleague.adobe.com/docs/target/using/activities/auto-allocate/automated-traffic-allocation.html) | × |
| [&#x200B; パーソナライズされたエクスペリエンスの自動ターゲティング &#x200B;](https://experienceleague.adobe.com/docs/target/using/activities/auto-target-to-optimize.html) | × |
