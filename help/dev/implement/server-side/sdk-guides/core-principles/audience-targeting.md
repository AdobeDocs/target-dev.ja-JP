---
title: オーディエンスのターゲティング
description: オーディエンスを使用すると、実験およびパーソナライゼーションアクティビティのターゲットを設定できます。 [!DNL Adobe Target]  は、すぐに使用できる無数の強力なオーディエンスターゲット設定機能をサポートしています。
exl-id: df1bd856-e848-452c-90a0-abf29e7a2313
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '702'
ht-degree: 26%

---

# オーディエンスのターゲティング

## 概要

オーディエンスを使用すると、実験およびパーソナライゼーションアクティビティのターゲットを設定できます。 [!DNL Adobe Target] は、すぐに使用できる無数の強力なオーディエンスターゲット設定機能をサポートしています。 [ オーディエンスターゲティング ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/create-audience.html?lang=ja) には、次の属性を使用できます。

### [!DNL Target] Library

詳しくは、[[!DNL Target]  ライブラリ ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-library.html?lang=ja) を参照してください。
&#x200B;
* Bing から
* Chrome ブラウザー
* Firefox ブラウザー
* Google から
* Internet Explorer
* Linux オペレーティングシステム
* Mac OS オペレーティングシステム
* 新しい訪問者数
* 再訪問者
* Safari ブラウザー
* タブレットデバイス
* Windows オペレーティングシステム
* Yahoo から

### 地域

詳しくは、[ 地域 ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=ja) を参照してください。
&#x200B;&#x200B;
* 国 / 地域
* 都道府県
* 市区町村
* 郵便番号
* 緯度
* 経度
* DMA
* 携帯電話会社

### ネットワーク

詳しくは、「[ ネットワーク ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=ja)」を参照してください。

* ISP
* ドメイン名
* 接続速度

### モバイル

詳しくは、[モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=ja)を参照してください。

* デバイスのマーケティング名
* デバイスモデル
* デバイスのベンダー
* モバイルデバイス
* 携帯電話
* タブレット
* OS
* 画面の高さ（px）
* 画面の幅 (px)

### カスタム

詳しくは、[ カスタムパラメーター ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=ja) を参照してください。

* 任意のキーと値のペア

### オペレーティングシステム

詳しくは、[ オペレーティングシステム ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=ja) を参照してください。

* Linux
* Macintosh
* Windows

### サイトのページ

詳しくは、「[ サイトのページ ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=ja)」を参照してください。

* 現在のページ
* 前のページ
* ランディングページ
* HTTP ヘッダー

### ブラウザー

詳しくは、[ ブラウザー ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=ja) を参照してください。

* タイプ
* 言語
* バージョン

### 訪問者プロファイル

詳しくは、[ 訪問者プロファイル ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=ja) を参照してください。

* 永続する任意のキーと値のペア

### トラフィックソース

詳しくは、[ トラフィックソース ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=ja) を参照してください。

* Baidu から
* Bing から
* Google から
* Yahoo から
* 参照ランディングページ：URL
* 参照ランディングページ：ドメイン
* 参照ランディングページ：クエリ

### 時間枠

詳しくは、[時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=ja)を参照してください。

* 開始日 / 終了日

## クライアントヒント

ブラウザー、オペレーティングシステム、モバイルオーディエンス属性およびプロファイルスクリプトの特定のインスタンスを正しくセグメント化するには、[!DNL Adobe Target] に Client Hints が必要です。 背景情報について詳しくは、[User Agent and Client Hints](../../../client-side/atjs/user-agent-and-client-hints.md) を参照してください。

### Client Hints を [!DNL Adobe Target] に渡す方法

Node.js SDK v2.4.0 および Java SDK v2.3.0 以降では、`getOffers()` 呼び出しを使用してクライアントヒントを [!DNL Target] に送信できます。 Client Hints は、User Agent と共に `request.context` オブジェクトに含める必要があります。

>[!BEGINTABS]

>[!TAB Node.js SDK]

```js {line-numbers="true"}
targetClient.getOffers({ 
    request: { 
        context: { 
            channel: "mobile" 
            userAgent: "Mozilla/5.0 (Linux; Android 12; Pixel 4a) AppleWebKit/537.36 (KHTML, like Gecko) Mobile Safari/537.36", 
            clientHints: { 
                mobile: "true", 
                platform: "Linux", 
                platformVersion: "12.1", 
                model: "Pixel 4a", 
                browserUAWithMajorVersion: "\"Not A;Brand\";v=\"98\", \"Chromium\";v=\"98\", \"Google Chrome\";v=\"98\"", 
                browserUAWithFullVersion: "\" Not A;Brand\";v=\"98.0.0.0\", \"Chromium\";v=\"98.0.4844.83\", \"Google Chrome\";v=\"98.0.4758.101\"", 
                bitness: "64", 
                architecture: "x86" 
            } 
        }, 
        execute: { 
            mboxes: [{ 
                name: "home", 
                index: 1 
            }] 
        } 
    } 
});
```

>[!TAB Java SDK]

```javascript {line-numbers="true"}
import com.adobe.target.delivery.v1.model.ClientHints; 
import com.adobe.target.delivery.v1.model.Context; 
import com.adobe.target.delivery.v1.model.ExecuteRequest; 
import com.adobe.target.edge.client.model.TargetDeliveryRequest; 

 
ClientHints clientHints = new ClientHints(); 
clientHints.setMobile(true); 
clientHints.setPlatform("macOS"); 
clientHints.setArchitecture("x86"); 
clientHints.setPlatformVersion("11.3.1"); 
clientHints.setBrowserUAWithMajorVersion( 
  "\" Not A;Brand\";v=\"99\", \"Chromium\";v=\"99\", \"Google Chrome\";v=\"99\""); 
String userAgent = 
  "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Safari/537.36"; 

 
TargetDeliveryRequest request = TargetDeliveryRequest.builder() 
        .execute(new ExecuteRequest().pageLoad(pageLoad)) 
        .context(new Context().clientHints(clientHints).userAgent(userAgent)) 
        .build(); 
```

>[!ENDTABS]

## オンデバイス判定

次の表に、オンデバイス判定でサポートされるオーディエンスルールとサポートされないオーディエンスルールを示します。

| オーディエンスルール | オンデバイス判定 |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=ja) | ○ |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=ja) | × |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=ja) | × |
| [ カスタムパラメーター ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=ja) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=ja) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=ja) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=ja) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=ja) | × |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=ja) | × |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=ja) | ○ |
| [Experience Cloudオーディエンス ](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=ja) （Adobe Audience Manager、Adobe AnalyticsおよびAdobe Experience Managerのオーディエンス） | × |

### オンデバイス判定のためのジオターゲティング

ジオベースのオーディエンスを使用したオンデバイス判定アクティビティの待ち時間をほぼゼロに保つために、Adobeは `getOffers` の呼び出しでジオバリューを自分で指定することをお勧めします。 これを行うには、リクエストの `Context` に `Geo` オブジェクトを設定します。 つまり、サーバーには、各エンドユーザーの場所を判断する方法が必要になります。 例えば、サーバーは、設定したサービスを使用して IP からジオルックアップを実行する場合があります。 Google Cloud など、一部のホスティングプロバイダーは、各 `HttpServletRequest` ージのカスタムヘッダーを介してこの機能を提供します。

>[!BEGINTABS]

>[!TAB Node.js SDK]

```js {line-numbers="true"}
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

>[!TAB Java SDK]

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

ただし、サーバーで IP からジオベースの検索を実行できない場合でも、ジオベースのオーディエンスを含んだ `getOffers` リクエストに対してオンデバイス判定を実行すると、これもサポートされます。 このアプローチの欠点は、リモートの IP からジオルックアップを使用することで、各 `getOffers` 呼び出しに待ち時間が追加されることです。 この待ち時間は、サーバーの近くにある CDN にヒットするため、リモート `getOffers` 呼び出しよりも短くなる必要があります。 SDK がユーザーの IP アドレスの位置情報を取得するには、リクエストの `Context` に `Geo` オブジェクトの `ipAddress` フィールドを **のみ** 指定する必要があります。 `ipAddress` 以外のフィールドが指定された場合、[!DNL Target] SDK は解決のために位置情報メタデータを取得しません。

>[!BEGINTABS]

>[!TAB Node.js SDK]

```js {line-numbers="true"}
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

>[!TAB Java SDK]

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

## サーバーサイドの決定

次の表に、サーバー側判定でサポートされるオーディエンスルールとサポートされないオーディエンスルールを示します。

| オーディエンスルール | サーバー側決定 |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=ja) | ○ |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=ja) | ○ |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=ja) | ○ |
| [ カスタムパラメーター ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=ja) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=ja) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=ja) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=ja) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=ja) | ○ |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=ja) | ○ |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=ja) | ○ |
| [Experience Cloudオーディエンス ](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=ja) （Adobe Audience Manager、Adobe AnalyticsおよびAdobe Experience Managerのオーディエンス） | ○ |
