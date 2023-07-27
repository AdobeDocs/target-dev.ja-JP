---
title: オーディエンスのターゲティング
description: オーディエンスを使用して、実験やパーソナライゼーションアクティビティをターゲット設定できます。 [!DNL Adobe Target] は、無数の強力なオーディエンスターゲティング機能をすぐに使用できます。
exl-id: df1bd856-e848-452c-90a0-abf29e7a2313
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 20%

---

# オーディエンスのターゲティング

## 概要

オーディエンスを使用して、実験やパーソナライゼーションアクティビティをターゲット設定できます。 [!DNL Adobe Target] は、無数の強力なオーディエンスターゲティング機能をすぐに使用できます。 次の属性を使用できます。 [オーディエンスターゲティング](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/create-audience.html):

### [!DNL Target]ライブラリ

詳しくは、 [[!DNL Target] ライブラリ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-library.html).&#x200B;
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

詳しくは、 [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html).&#x200B;&#x200B;
* 国 / 地域
* 都道府県
* 市区町村
* 郵便番号
* 緯度
* 経度
* DMA
* 携帯電話会社

### ネットワーク

詳しくは、 [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html).

* ISP
* ドメイン名
* 接続速度

### モバイル

詳しくは、[モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html)を参照してください。

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

詳しくは、 [カスタムパラメーター](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html).

* 任意のキーと値のペア

### オペレーティングシステム

詳しくは、 [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html).

* Linux
* Macintosh
* Windows

### サイトのページ

詳しくは、 [サイトページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html).

* 現在のページ
* 前のページ
* ランディングページ
* HTTP ヘッダー

### ブラウザー

詳しくは、 [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html).

* タイプ
* 言語
* バージョン

### 訪問者プロファイル

詳しくは、 [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html).

* 永続化された任意のキーと値のペア

### トラフィックソース

詳しくは、 [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html).

* Baidu から
* Bing から
* Google から
* Yahoo から
* 参照ランディングページ：URL
* 参照ランディングページ：ドメイン
* 参照ランディングページ：クエリ

### 時間枠

詳しくは、[時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html)を参照してください。

* 開始日 / 終了日

## クライアントヒント

[!DNL Adobe Target] には、ブラウザー、オペレーティングシステム、モバイルオーディエンス属性、およびプロファイルスクリプトの特定のインスタンスを正しくセグメント化するためのクライアントヒントが必要です。 詳しくは、 [ユーザーエージェントとクライアントヒント](../../../client-side/atjs/user-agent-and-client-hints.md).

### クライアントヒントをに渡す方法 [!DNL Adobe Target]

Node.js SDK v2.4.0 および Java SDK v2.3.0 以降では、クライアントヒントをに送信できます。 [!DNL Target] 経由 `getOffers()` 呼び出し。 クライアントヒントは、 `request.context` オブジェクトにユーザーエージェントを追加します。

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

次の表は、オンデバイス判定でサポートされるオーディエンスルールとサポートされないオーディエンスルールを示しています。

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

### オンデバイス判定のジオターゲティング

地域ベースのオーディエンスを使用するオンデバイス判定アクティビティの待ち時間をほぼゼロに保つために、Adobeは、 `getOffers`. これをおこなうには、 `Geo` オブジェクトを `Context` リクエストの。 つまり、サーバーは各エンドユーザーの場所を判断する方法が必要になります。 例えば、サーバーが、設定したサービスを使用して IP-to-Geo ルックアップを実行する場合があります。 Google Cloud など、一部のホスティングプロバイダーは、各 `HttpServletRequest`.

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

ただし、サーバーで IP-to-Geo 検索を実行する機能を持っていないのに対し、オンデバイスでの判定を実行したい場合は、 `getOffers` 地域ベースのオーディエンスを含むリクエストもサポートされます。 このアプローチの欠点は、リモートの IP-to-Geo ルックアップを使用し、各 IP-to-Geo ルックアップに遅延が追加されることです `getOffers` を呼び出します。 この待ち時間はリモートの待ち時間よりも短くする必要があります `getOffers` を呼び出します。サーバーの近くにある CDN をヒットするからです。 次の条件を満たす必要があります。 **のみ** 提供する `ipAddress` フィールド `Geo` オブジェクトを `Context` を取得する必要があります。 他のフィールド ( `ipAddress` が指定されている場合、 [!DNL Target] SDK は解決のために位置情報メタデータを取得しません。

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

## サーバー側判定

次の表に、サーバー側判定でサポートされるオーディエンスルールとサポートされないオーディエンスルールを示します。

| オーディエンスルール | サーバー側判定 |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | ○ |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | ○ |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | ○ |
| [カスタムパラメーター](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | ○ |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | ○ |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | ○ |
| [Experience Cloudオーディエンス](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) (Adobe Audience Manager、Adobe Analytics、Adobe Experience Managerのオーディエンス | ○ |
