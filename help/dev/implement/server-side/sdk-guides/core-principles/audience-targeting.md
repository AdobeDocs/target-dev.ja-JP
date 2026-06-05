---
title: オーディエンスターゲティング
description: オーディエンスを使用すると、実験やパーソナライゼーション活動のターゲットを設定できます。 [!DNL Adobe Target] は、すぐに使用できる強力なオーディエンスターゲティング機能をサポートしています。
exl-id: df1bd856-e848-452c-90a0-abf29e7a2313
feature: Implement Server-side
TQID: https://experienceleague.adobe.com/BmKrCmWIkEkNHiipZ-DqDlhzOT7bVmKHl9de5uXhJQU
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: bcc5edb5-84c3-4940-9f84-ed88b6c16274
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1069
ht-degree: 17%

---

# オーディエンスターゲティング

## 概要

オーディエンスは、実験とパーソナライゼーションの活動をターゲットにするために使用できます。 [!DNL Adobe Target]は、標準で用意されている多数の強力なオーディエンスターゲティング機能をサポートしています。 [&#x200B; オーディエンスターゲティング &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/create-audience.html?lang=ja)では、次の属性を使用できます。

### [!DNL Target] ライブラリ

詳しくは、[[!DNL Target]  ライブラリ &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-library.html?lang=ja)を参照してください。
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

詳しくは、[地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=ja)を参照してください。
&#x200B;
* 国 / 地域
* 都道府県
* 市区町村
* 郵便番号
* 緯度
* 経度
* DMA
* 携帯電話会社

### ネットワーク

詳しくは、[&#x200B; ネットワーク &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=ja)を参照してください。

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

詳しくは、[&#x200B; カスタムパラメーター](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=ja)を参照してください。

* 任意のキー/値のペア

### オペレーティングシステム

詳しくは、[&#x200B; オペレーティングシステム &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=ja)を参照してください。

* Linux
* Macintosh
* Windows

### サイトのページ

詳しくは、[&#x200B; サイトページ &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=ja)を参照してください。

* 現在のページ
* 前のページ
* ランディングページ
* HTTP ヘッダー

### ブラウザー

詳しくは、[&#x200B; ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=ja)を参照してください。

* タイプ
* 言語
* バージョン

### 訪問者プロファイル

詳しくは、[訪問者プロファイル &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=ja)を参照してください。

* 永続化される任意のキーと値のペア

### トラフィックソース

詳しくは、[&#x200B; トラフィックソース &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=ja)を参照してください。

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

[!DNL Adobe Target]には、ブラウザー、オペレーティングシステム、およびモバイルオーディエンス属性の正しいセグメント化と、プロファイルスクリプトの特定のインスタンスに対するクライアントヒントが必要です。 背景の詳細については、[&#x200B; ユーザーエージェントとクライアントヒント &#x200B;](../../../client-side/atjs/user-agent-and-client-hints.md)を参照してください。

### クライアントヒントを[!DNL Adobe Target]に渡す方法

Node.js SDK v2.4.0およびJava SDK v2.3.0以降、クライアントヒントは`getOffers()`呼び出しを介して[!DNL Target]に送信できます。 クライアントヒントは、ユーザーエージェントと共に`request.context` オブジェクトに含める必要があります。

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

次の表は、オンデバイス決定でサポートされているオーディエンスルールまたはサポートされていないオーディエンスルールを示しています。

| オーディエンスルール | オンデバイス判定 |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=ja) | ○ |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=ja) | × |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=ja) | × |
| [&#x200B; カスタムパラメーター](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=ja) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=ja) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=ja) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=ja) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=ja) | × |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=ja) | × |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=ja) | ○ |
| [Experience Cloud Audiences](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=ja) （Adobe Audience Manager、Adobe Analytics、Adobe Experience Managerのオーディエンス） | × |

### オンデバイス判定のための地域ターゲティング

位置情報ベースのオーディエンスを使用したオンデバイスの意思決定アクティビティの遅延をほぼゼロに近く維持するために、Adobeでは、`getOffers`への呼び出しで位置情報の値を自分で指定することをお勧めします。 これを行うには、リクエストの`Context`で`Geo` オブジェクトを設定します。 つまり、サーバーには、各エンドユーザーの場所を決定する方法が必要になります。 例えば、設定したサービスを使用して、サーバーがIPから地域へのルックアップを実行する場合があります。 Google Cloudなどの一部のホスティングプロバイダーは、各`HttpServletRequest`のカスタムヘッダーを使用してこの機能を提供します。

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

ただし、サーバーでIPから地域への検索を実行する機能がないものの、地域ベースのオーディエンスを含む`getOffers`要求に対してデバイス上で決定を実行したい場合は、これもサポートされます。 このアプローチの欠点は、リモート IP-to-Geo ルックアップを使用することで、各`getOffers`呼び出しに遅延が追加されることです。 サーバーの近くにあるCDNにヒットするため、この待ち時間はリモート `getOffers`呼び出しよりも低くしてください。 SDKがユーザーのIP アドレスの位置情報を取得するには、リクエストの`Context`の`Geo` オブジェクトの`ipAddress` フィールドを&#x200B;**のみ**&#x200B;で指定する必要があります。 `ipAddress`以外のフィールドが指定されている場合、[!DNL Target] SDKは解決のために位置情報メタデータを取得しません。

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

## サーバーサイド決定

次の表に、サーバーサイド決定でサポートされているオーディエンスルールまたはサポートされていないオーディエンスルールを示します。

| オーディエンスルール | サーバーサイド決定 |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html?lang=ja) | ○ |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html?lang=ja) | ○ |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=ja) | ○ |
| [&#x200B; カスタムパラメーター](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html?lang=ja) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html?lang=ja) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html?lang=ja) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html?lang=ja) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=ja) | ○ |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html?lang=ja) | ○ |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html?lang=ja) | ○ |
| [Experience Cloud Audiences](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html?lang=ja) （Adobe Audience Manager、Adobe Analytics、Adobe Experience Managerのオーディエンス） | ○ |
