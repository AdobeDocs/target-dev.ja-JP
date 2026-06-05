---
title: at.jsとExperience Platform Web SDKの比較
description: at.js機能と [!DNL Experience Platform Web SDK]の比較について説明します。
keywords: target;adobe target;activity.id;experience.id;renderDecisions;decisionScopes；事前非表示のスニペット；vec；フォームベースのExperience Composer;xdm;audiences;decisions；スコープ；スキーマ；システム図；図
feature: AEP Web SDK
exl-id: 31c9722b-5d92-4653-aa20-4183d166c097
TQID: https://experienceleague.adobe.com/Ly2ytp87gfQ5mCES-43K5tU4-4fhTjdcdk-OxRRL-II
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
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: c2be0313-b3ae-45e0-b454-d20bf54b23f2
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 2308
ht-degree: 9%

---

# at.js ライブラリと[!DNL Adobe Experience Platform Web SDK]の比較

## 概要

この記事では、`at.js` ライブラリとExperience Platform Web SDKの違いについて説明します。

## ライブラリのインストール

### at.jsのインストール

[!DNL Adobe]では、[!DNL Adobe Experience Cloud]、[!UICONTROL 実装] タブから直接ライブラリをダウンロードできます。 at.js ライブラリは、顧客がclientCode、imsOrgIdなどの設定でカスタマイズされます。

### Web SDKのインストール

ビルド済みのバージョンは、CDNで利用できます。 CDN上のライブラリをページ上で直接参照するか、独自のインフラストラクチャでダウンロードしてホストできます。 最小化された形式と最小化されていない形式で使用できます。 最小化されていないバージョンは、デバッグの目的に役立ちます。

詳しくは、[JavaScript ライブラリを使用したWeb SDKのインストール &#x200B;](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/install/library)を参照してください。

## ライブラリの設定

### at.jsの設定

すべてのat.js ファイルの最後に、[!DNL Adobe]がインスタンス化して設定オブジェクトを渡すセクションが表示されます。 カスタマイズ可能で、ダウンロード時にAdobeはそのセクションに現在のお客様の設定を入力します。

```javascript
window.adobe.target.init(window, document, {
  "clientCode": "demo",
  "imsOrgId": "",
  "serverDomain": "localhost:5000",
  "timeout": 2000,
  "globalMboxName": "target-global-mbox",
  "version": "2.0.0",
  "defaultContentHiddenStyle": "visibility: hidden;",
  "defaultContentVisibleStyle": "visibility: visible;",
  "bodyHiddenStyle": "body {opacity: 0 !important}",
  "bodyHidingEnabled": true,
  "deviceIdLifetime": 63244800000,
  "sessionIdLifetime": 1860000,
  "selectorsPollingTimeout": 5000,
  "visitorApiTimeout": 2000,
  "overrideMboxEdgeServer": false,
  "overrideMboxEdgeServerTimeout": 1860000,
  "optoutEnabled": false,
  "optinEnabled": false,
  "secureOnly": false,
  "supplementalDataIdParamTimeout": 30,
  "authoringScriptUrl": "//cdn.tt.omtrdc.net/cdn/target-vec.js",
  "urlSizeLimit": 2048,
  "endpoint": "/rest/v1/delivery",
  "pageLoadEnabled": true,
  "viewsEnabled": true,
  "analyticsLogging": "server_side",
  "serverState": {},
  "decisioningMethod": "server-side",
  "legacyBrowserSupport":  false
});
```

[詳細情報](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)

### Platform Web SDKの設定

SDKの設定は、[`configure`](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/commands/configure/overview) コマンドを使用して行います。 `configure` コマンドは、最初に呼び出された&#x200B;*always*&#x200B;です。

## ページ読み込み[!DNL Target] オファーをリクエストして自動的にレンダリングする方法

### at.jsの使用

at.js 2.xを使用して、設定`pageLoadEnabled,`を有効にすると、ライブラリは[!DNL Target] Edgeへの呼び出しを`execute -> pageLoad`でトリガーします。 すべての設定がデフォルト値に設定されている場合、カスタムコーディングは必要ありません。 at.jsがページに追加され、ブラウザーによって読み込まれると、[!DNL Target] Edge呼び出しが実行されます。

### [!DNL PLatform Web SDK]の使用中

[!DNL Target] [Visual Experience Composer](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/visual-experience-composer)内で作成されたコンテンツは、SDKで自動的に取得およびレンダリングできます。

[!DNL Target]件のオファーをリクエストして自動的にレンダリングするには、`sendEvent` コマンドを使用し、`renderDecisions` オプションを`true.`に設定します。これにより、SDKは、自動レンダリングの対象となるパーソナライズされたコンテンツを自動的にレンダリングします。

例：

```javascript
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "commerce": {
      "order": {
        "purchaseID": "a8g784hjq1mnp3",
        "purchaseOrderNumber": "VAU3123",
        "currencyCode": "USD",
        "priceTotal": 999.98
      }
    }
  }
});
```

[!DNL Experience Platform Web SDK]は、[!DNL Platform WEB SDK]によって実行されたオファーを含む通知を自動的に送信します。 通知リクエストペイロードの例を次に示します。

```json
{
  "events": [{
      "xdm": {
        "_experience": {
          "decisioning": {
            "propositions": [
              {
                "id": "AT:eyJhY3Rpdml0eUlkIjoiMTI3MDE5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
                "scope": "cart",
                "scopeDetails": {
                  "decisionProvider": "TGT",
                  "activity": {
                    "id": "127019"
                  },
                  "experience": {
                    "id": "0"
                  },
                  "strategies": [
                    {
                      "step": "entry",
                      "algorithmID": "0",
                      "trafficType": "0"
                    },
                    {
                      "step": "display",
                      "algorithmID": "0",
                      "trafficType": "0"
                    }
                  ],
                  "characteristics": {
                    "eventToken": "bKMxJ8dCR1XlPfDCx+2vSGqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                  }
                }
              }
            ]
          }
        },
        "eventType": "display",
        "web": {
          "webPageDetails": {
            "viewName": "cart",
            "URL": "https://alloyio.com/personalizationSpa/cart"
          },
          "webReferrer": {
            "URL": ""
          }
        },
        "device": {
          "screenHeight": 800,
          "screenWidth": 1280,
          "screenOrientation": "landscape"
        },
        "environment": {
          "type": "browser",
          "browserDetails": {
            "viewportWidth": 1280,
            "viewportHeight": 284
          }
        },
        "placeContext": {
          "localTime": "2021-12-10T15:50:34.467+02:00",
          "localTimezoneOffset": -120
        },
        "timestamp": "2021-12-10T13:50:34.467Z",
        "implementationDetails": {
          "name": "https://ns.adobe.com/experience/alloy",
          "version": "2.6.2",
          "environment": "browser"
        }
      }
    }
  ]
}
```

[詳細情報](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## ページ読み込みターゲットのオファーを自動的にレンダリングする方法と&#x200B;*NOT*

### at.jsの使用

ページ読み込みのオファーを取得する[!DNL Target] Edgeへの呼び出しを実行するには、2つの方法があります。

例1:

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox", 
   success: console.log,
   error: console.error
});
```

例2:

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {}
    }
  }
})
.then(console.log)
.catch(console.error);
```

[詳細情報](https://experienceleague.adobe.com/en/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/atjs-functions)

### [!DNL Platform Web SDK]の使用中

`decisionScopes`の下に特別なスコープを持つ`sendEvent` コマンドを実行します：`__view__`。 [!DNL Adobe]は、このスコープをシグナルとして使用して、[!DNL Target]からすべてのページ読み込みアクティビティを取得し、すべてのビューを先行取得します。 [!DNL Platform Web SDK]は、すべてのVEC ビューベースのアクティビティを評価しようとしています。 ビューの先行取得を無効にすることは、現在[!DNL Platform Web SDK]ではサポートされていません。

パーソナライゼーションコンテンツにアクセスするには、コールバック関数を指定します。これは、SDKがサーバーから正常なレスポンスを受け取った後に呼び出されます。 コールバックには結果オブジェクトが指定されます。このオブジェクトには、返されたパーソナライゼーションコンテンツを含むpropositions プロパティが含まれている場合があります。

例：

```javascript
alloy("sendEvent", {
    xdm: {...},
    decisionScopes: ["__view__"]
  }).then(function(result) {
    if (result.propositions) {
      result.propositions.forEach(proposition => {
        proposition.items.forEach(item => {
          if (item.schema === HTML_SCHEMA) {
            // manually apply offer
            document.getElementById("form-based-offer-container").innerHTML =
              item.data.content;
            const executedPropositions = [
              {
                id: proposition.id,
                scope: proposition.scope,
                scopeDetails: proposition.scopeDetails
              }
            ];
          // manually send the display notification event, so that Target/Analytics impressions aare increased
            alloy("sendEvent",{
              "xdm": {
                "eventType": "decisioning.propositionDisplay",
                "_experience": {
                  "decisioning": {
                    "propositions": executedPropositions
                  }
                }
              }
            });
          }
        });
      });
    }
  });
```

[詳細情報](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## 特定のフォームベースのTarget mboxをリクエストする方法

### at.jsの使用

`getOffer`関数を使用してアクティビティを取得できます。

例1:

```javascript
adobe.target.getOffer({
   mbox: "hero-banner", 
   success: console.log,
   error: console.error
});
```

例2:

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        mboxes: [
        {
          index: 0,
          name: "hero-banner"
        }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

[詳細情報](https://experienceleague.adobe.com/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/atjs-functions.html?lang=ja)

### [!DNL Platform Web SDK]の使用中

`sendEvent` コマンドを使用し、`decisionScopes` オプションの下にmbox名を渡すことで、[!UICONTROL &#x200B; フォームベースのコンポーザー]のアクティビティを取得できます。 `sendEvent` コマンドは、要求されたアクティビティまたは提案を含むオブジェクトで解決されるプロミスを返します。

このコードスニペットは、`propositions`配列がどのように見えるかです。

```javascript
[
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "hero-banner",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "experience": {
        "id": "0"
      },
      "strategies": [
        {
          "algorithmID": "0",
          "trafficType": "0"
        }
      ],
      "characteristics": {
        "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw=="
      }
    },
    "items": [
      {
        "id": "1184844",
        "schema": "https://ns.adobe.com/personalization/html-content-item",
        "meta": {
          "geo.state": "bucuresti",
          "activity.id": "434689",
          "experience.id": "0",
          "activity.name": "a4t test form based activity",
          "offer.id": "1184844",
          "profile.tntId": "04608610399599289452943468926942466370-pybgfJ"
        },
        "data": {
          "id": "1184844",
          "format": "text/html",
          "content": "<div> analytics impressions </div>"
        }
      }
    ]
  },
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "hero-banner",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "characteristics": {
        "eventToken": "E0gb6q1+WyFW3FMbbQJmrg=="
      }
    },
    "items": [
      {
        "id": "434689",
        "schema": "https://ns.adobe.com/personalization/measurement",
        "data": {
          "type": "click",
          "format": "application/vnd.adobe.target.metric"
        }
      }
    ]
  }
]
```

例：

```javascript
alloy("sendEvent", {
  xdm: { ...},
  decisionScopes: ["hero-banner"]
}).then(function (result) {
  var propositions = result.propositions;

  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      for (var j = 0; j < proposition.items; j++) {
        var item = proposition.items[j];
        if (item.schema === HTML_SCHEMA) {
          // apply offer
          document.getElementById("form-based-offer-container").innerHTML =
            item.data.content;
          const executedPropositions = [
            {
              id: proposition.id,
              scope: proposition.scope,
              scopeDetails: proposition.scopeDetails
            }
          ];

          alloy("sendEvent", {
            "xdm": {
              "eventType": "decisioning.propositionDisplay",
              "_experience": {
                "decisioning": {
                  "propositions": executedPropositions
                }
              }
            }
          });
        }
      }
    }
  }
});
```

[詳細情報](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## [!DNL Target] アクティビティの適用方法

### at.jsの使用

`applyOffers`関数を使用して[!DNL Target] アクティビティを適用できます：`adobe.target.applyOffer(options).`

例：

```javascript
adobe.target.getOffers({...})
  .then(response => adobe.target.applyOffers({ response: response }))
  .then(() => console.log("Success"))
  .catch(error => console.log("Error", error));
```

`applyOffers` コマンドについて詳しくは、[専用ドキュメント &#x200B;](https://experienceleague.adobe.com/en/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/adobe-target-applyoffers-atjs-2)を参照してください。

### [!DNL Platform Web SDK]の使用中

`applyPropositions` コマンドを使用して、[!DNL Target] アクティビティを適用できます。

例：

```javascript
alloy("applyPropositions", {
    propositions: [...]
});
```

`applyPropositions` コマンドについて詳しくは、[専用ドキュメント &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)を参照してください。

## イベントの追跡方法

### at.jsの使用

イベントは、`trackEvent`関数を使用するか、`sendNotifications.`を使用して追跡できます

この関数は、クリックやコンバージョンなどのユーザーアクションを報告するリクエストを実行します。 この関数は、応答でアクティビティを配信しません。

**例 1**

```javascript
adobe.target.trackEvent({ 
    "type": "click",
    "mbox": "some-mbox"
});
```

**例 2**

```javascript
adobe.target.sendNotifications({ 
    request: {
       notifications: [{
          ...,
          mbox: {
            name: "some-mbox"
          },
          type: "click",
          ...
       }]
    }
});
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-trackevent.html)

### [!DNL Platform Web SDK]の使用中

`sendEvent` コマンドを呼び出し、`_experience.decisioning.propositions` XDM `fieldgroup`を入力し、`eventType`を2つの値のいずれかに設定することで、イベントとユーザーアクションを追跡できます。

* `decisioning.propositionDisplay`: [!DNL Target] アクティビティのレンダリングを通知します。
* `decisioning.propositionInteract`: マウスのクリックなど、アクティビティに対するユーザーの操作を示します。

`_experience.decisioning.propositions` XDM `fieldgroup`はオブジェクトの配列です。 各オブジェクトのプロパティは、`sendEvent` コマンドで返される`result.propositions`から派生します：`{ id, scope, scopeDetails }.`

**例1 - アクティビティのレンダリング後に`decisioning.propositionDisplay` イベントを追跡**

```javascript
alloy("sendEvent", {
  xdm: {},
  decisionScopes: ['discount']
}).then(function(result) {
  var propositions = result.propositions;

  var discountProposition;
  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      if (proposition.scope === "discount") {
        discountProposition = proposition;
        break;
      }
    }
  }

  if (discountProposition) {
    // Find the item from proposition that should be rendered.
    // Rather than assuming there a single item that has HTML
    // content, find the first item whose schema indicates
    // it contains HTML content.
    for (var j = 0; j < discountProposition.items.length; j++) {
      var discountPropositionItem = discountProposition.items[i];
      if (discountPropositionItem.schema === "https://ns.adobe.com/personalization/html-content-item") {
        var discountHtml = discountPropositionItem.data.content;
        // Render the content
        var dailySpecialElement = document.getElementById("daily-special");
        dailySpecialElement.innerHTML = discountHtml;
        
        // For this example, we assume there is only a single place to update in the HTML.
        break;  
      }
    }
    // Send a "decisioning.propositionDisplay" event signaling that the proposition has been rendered.
    alloy("sendEvent", {
      "xdm": {
        "eventType": "decisioning.propositionDisplay",
        "_experience": {
          "decisioning": {
            "propositions": [{
              "id": id,
              "scope": scope,
              "scopeDetails": scopeDetails
            }],
            "propositionEventType": {
              "display": 1
            }
          }
        }
      }
    });
  }
});
```

**例2 - クリック指標が発生した後の`decisioning.propositionInteract` イベントの追跡**

```javascript
alloy("sendEvent", {
  xdm: { ...},
  decisionScopes: ["hero-banner"]
}).then(function (result) {
  var propositions = result.propositions;

  if (propositions) {
    // Find the discount proposition, if it exists.
    for (var i = 0; i < propositions.length; i++) {
      var proposition = propositions[i];
      for (var j = 0; j < proposition.items.length; j++) {
        var item = proposition.items[j];

        if (item.schema === "https://ns.adobe.com/personalization/measurement") {
          // add metric to the DOM element
          const button = document.getElementById("form-based-click-metric");

          button.addEventListener("click", event => {
            const executedPropositions = [
              {
                id: proposition.id,
                scope: proposition.scope,
                scopeDetails: proposition.scopeDetails
              }
            ];
            // send the click track event
            alloy("sendEvent", {
              "xdm": {
                "eventType": "decisioning.propositionInteract",
                "_experience": {
                  "decisioning": {
                    "propositions": executedPropositions
                  }
                }
              }
            });
          });
        }
      }
    }
  }
});
```

[詳細情報](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/rendering-personalization-content#manual)

**例3 - アクションの実行後に発生したイベントの追跡**

次の使用例は、ボタンのクリックなど、特定のアクションを実行した後に発生したイベントを追跡します。
`__adobe.target` データオブジェクトを使用して、追加のカスタムパラメーターを追加できます。

`commerce` XDM オブジェクトを追加することもできます。

```js
alloy("sendEvent", {
    "xdm": {
        "_experience": {
            "decisioning": {
                "propositions": [
                    {
                        "scope": "orderConfirm" //example scope name
                    }
                ],
                "propositionEventType": {
                    "display": 1
                }
            }
        },
        "eventType": "decisioning.propositionDisplay"
    },
    "commerce": {
        "order": {
            "purchaseID": "a8g784hjq1mnp3",
            "purchaseOrderNumber": "VAU3123",
            "currencyCode": "USD",
            "priceTotal": 999.98
        }
    },
    "data": {
        "__adobe": {
            "target": {
                "pageType": "Order Confirmation",
                "user.categoryId": "Insurance"
            }
        }
    }
})
```

## シングルページアプリケーションでビューの変更をトリガーする方法

### at.jsの使用

`adobe.target.triggerView`関数を使用します。 この関数は、新しいページが読み込まれるときや、ページ上のコンポーネントが再レンダリングされるときに呼び出すことができます。 `adobe.target.triggerView()`関数は、[!UICONTROL Visual Experience Composer] （VEC）を使用して[!UICONTROL A/B テスト &#x200B;]および[!UICONTROL &#x200B; エクスペリエンスのターゲット設定] （XT）アクティビティを作成するシングルページアプリケーション （SPA）に対して実装する必要があります。 `adobe.target.triggerView()`がサイトに実装されていない場合、VECをSPAに使用することはできません。

**例**

```javascript
adobe.target.triggerView("homeView")
```

[詳細情報](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)

### [!DNL Platform Web SDK]の使用中

シングルページアプリケーション [!UICONTROL View Change]をトリガーまたはシグナルするには、`sendEvent` コマンドの`xdm` オプションの`web.webPageDetails.viewName` プロパティを設定します。 [!DNL Platform Web SDK]は、`sendEvent`で指定された`viewName`に対するオファーがある場合、ビューキャッシュをチェックし、それらを実行して表示通知イベントを送信します。

**例**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  xdm:{
    web:{
      webPageDetails:{
        viewName: "homeView"
      }
    }
  }
});
```

[詳細情報](/help/dev/implement/client-side/aep-web-sdk/spa-implementation.md)

## [!UICONTROL 応答トークン &#x200B;]の活用方法

[!DNL Target]から返されたPersonalization コンテンツには、[応答トークン &#x200B;](https://experienceleague.adobe.com/en/docs/target/using/administer/response-tokens)が含まれています。 応答トークンには、アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、位置情報などの詳細が含まれます。 これらの詳細は、サードパーティのツールと共有することも、デバッグに使用することもできます。 応答トークンは、[!DNL Target] ユーザーインターフェイスで設定できます。

### at.jsの使用

at.js カスタムイベントを使用して、[!DNL Target]応答をリッスンし、応答トークンを読み取ります。

**例**

```javascript
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(e) { 
  console.log("Request succeeded", e.detail); 
}); 
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)

### [!DNL Platform Web SDK]の使用中

>[!IMPORTANT]
>
>[!DNL Experience Platform Web SDK] バージョン 2.6.0以降を使用していることを確認してください。

応答トークンは、`sendEvent` コマンドの結果で公開される`propositions`の一部として返されます。 各提案には`items,`の配列が含まれ、各項目には`meta` オブジェクトが含まれており、対応トークンが[!DNL Target]管理UIで有効になっている場合は、応答トークンが入力されます。 [詳細情報](https://experienceleague.adobe.com/en/docs/target/using/administer/response-tokens)

**例**

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    if (result.propositions) {
      // Format of result.propositions:
      /*
        [
            {
                "id": "",
                "scope": "",
                "items": [
                    {
                        "id": "",
                        "schema": "",
                        "data": {},
                        "meta": { // RESPONSE TOKENS
                            "activity.name": ...,
                            "offer.id": ...,
                            "profile.activeActivities": ...
                        }
                    }
                ],
                "scopeDetails": {}
                "renderAttempted": false
            }
        ]
      */
    }
  });
```

[詳細情報](/help/dev/implement/client-side/aep-web-sdk/accessing-response-tokens.md)

## フリッカーの管理方法

### at.jsの使用

at.jsを使用すると、at.jsが処理できるように`bodyHidingEnabled: true`を設定することで、ちらつきを管理できます
DOMの変更を取得して適用する前に、パーソナライズされたコンテナを事前に非表示にします。

パーソナライズされたコンテンツを含むページセクションは、at.js `bodyHiddenStyle.`を上書きすることで事前非表示にできます

既定では、`bodyHiddenStyle`はHTML `body.`全体を非表示にします

両方の設定は、`window.targetGlobalSettings.` `window.targetGlobalSettings`を使用して上書きできます。これらは、at.jsを読み込む前に配置する必要があります。

### [!DNL Platform Web SDK]の使用中

[!DNL Platform Web SDK]を使用すると、次の例のように、configure コマンドで事前非表示スタイルを設定できます。

```javascript
alloy("configure", {
  datastreamId: "configurationId",
  orgId: "orgId@AdobeOrg",
  debugEnabled: true,
  prehidingStyle: "body { opacity: 0 !important }"
});
```

[!DNL Platform Web SDK]非同期を読み込む場合、[!DNL Adobe]は、[!DNL Platform Web SDK]を挿入する前に、次のスニペットをページに挿入することをお勧めします。

```html
<script>
  !function(e,a,n,t){
  if (a) return;
  var i=e.head;if(i){
  var o=e.createElement("style");
  o.id="alloy-prehiding",o.innerText=n,i.appendChild(o),
  setTimeout(function(){o.parentNode&&o.parentNode.removeChild(o)},t)}}
  (document, document.location.href.indexOf("adobe_authoring_enabled") !== -1, "body { opacity: 0 !important }", 3000);
</script>
```

## A4Tはどのように扱われているのか

### at.jsの使用

at.jsを使用してサポートされるA4T ログには、次の2種類があります。

* Analytics クライアントサイドロギング
* Analytics Server Side Logging

#### Analytics クライアントサイドロギング

**例1: [!DNL Target] グローバル設定の使用**

Analytics クライアント側ログは、at.js設定で`analyticsLogging: client_side`を設定するか、`window.targetglobalSettings` オブジェクトを上書きすることで有効にできます。

このオプションを設定すると、返されるペイロードの形式は次のようになります。

```json
{
  "analytics": {
    "payload": {
      "pe": "tnt",
      "tnta": "167169:0:0|0|100,167169:0:0|2|100,167169:0:0|1|100"
    }
  }
}
```

次に、ペイロードを[!DNL &#x200B; Data Insertion API]経由で[!DNL Analytics]に転送できます。

例2: `getOffers`関数ごとに設定する：

```javascript
adobe.target.getOffers({
      request: {
        experienceCloud: {
          analytics: {
            logging: "client_side"
          }
        },
        prefetch: {
          mboxes: [{
            index: 0,
            name: "a1-serverside-xt"
          }]
        }
      }
    })
    .then(console.log)
```

このコードスニペットは、応答ペイロードがどのように表示されるかです。

```json
{
  "prefetch": {
    "mboxes": [{
      "index": 0,
      "name": "a1-serverside-xt",
      "options": [{
        "content": "<img src=\"http://s7d2.scene7.com/is/image/TargetAdobeTargetMobile/L4242-xt-usa?tm=1490025518668&fit=constrain&hei=491&wid=980&fmt=png-alpha\"/>",
        "type": "html",
        "eventToken": "n/K05qdH0MxsiyH4gX05/2qipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
        "responseTokens": {
          "profile.memberlevel": "0",
          "geo.city": "bucharest",
          "activity.id": "167169",
          "experience.name": "USA Experience",
          "geo.country": "romania"
        }
      }],
      "analytics": {
        "payload": {
          "pe": "tnt",
          "tnta": "167169:0:0|0|100,167169:0:0|2|100,167169:0:0|1|100"
        }
      }
    }]
  }
}
```

[!DNL Analytics] ペイロード （`tnta` トークン）は、[&#x200B; データ挿入API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)を使用して[!DNL Analytics] ヒットに含める必要があります。

#### [!DNL Analytics] サーバーサイドのログ

[!DNL Analytics] サーバー側ログは、at.js設定で`analyticsLogging: server_side`を設定するか、`window.targetglobalSettings` オブジェクトを上書きすることで有効にできます。

その後、データは次のように流れます。

Analytics Server Side Logging ワークフローを示す![図](/help/dev/implement/client-side/aep-web-sdk/assets/a4t-server-side-atjs.png)

[詳細情報](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html)

### [!DNL Platform Web SDK]の使用中

Web SDKでは、次の機能もサポートしています。

* Analytics クライアントサイドログ
* Analytics Server Side ログ

#### [!DNL Analytics] クライアント側ログ

[!DNL Analytics] Client Side Loggingは、そのDataStream設定で[!DNL Adobe Analytics]が無効になっている場合に有効になります。

Analytics クライアント側のログ記録ワークフローを示す![図](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-disabled-datastream-config.png)

お客様は、`sendEvent` コマンドをチェーンして[!DNL Analytics] データ挿入API[&#128279;](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)を使用してと共有する必要がある[!DNL Analytics] トークン （`tnta`）にアクセスし、結果として得られる提案の配列を繰り返します。

**例**

```javascript
alloy("sendEvent", {
    "renderDecisions": true,
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function (results) {
  var analyticsPayloads = new Set();
  for (var i = 0; i < results.propositions.length; i++) {
    var proposition = results.propositions[i];
    var renderAttempted = proposition.renderAttempted;

    if (renderAttempted === true) {
      var analyticsPayload = getAnalyticsPayload(proposition);
      if (analyticsPayload !== undefined) {
        analyticsPayloads.add(analyticsPayload);
      }
    }
  }
  var analyticsPayloadsToken = concatenateAnalyticsPayloads(analyticsPayloads);
  // send the page view Analytics hit with collected Analytics payload using Data Insertion API
});
```

[!DNL Analytics] Client Sideが有効になっている場合のデータの流れを示す図を次に示します。

Analytics クライアントサイドの![&#x200B; データフロー図のログ &#x200B;](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-client-side-logging.png)

#### [!DNL Analytics] サーバーサイドのログ

[!DNL Analytics] サーバー側ログは、そのDataStream設定で[!DNL Analytics]が有効になっている場合に有効になります。

Analytics設定を示す![&#x200B; データストリーム UI。](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-enabled-datastream-config.png)

サーバーサイド [!DNL Analytics] ログが有効になっている場合、[!DNL Analytics] レポートに正しいインプレッションとコンバージョンが表示されるように、[!DNL Analytics]と共有する必要があるA4T ペイロードがEdge Network レベルで共有されるので、お客様は追加の処理を行う必要はありません。

サーバーサイド分析ログが有効になっている場合、システムにデータが流れ込む方法は次のとおりです。

![&#x200B; サーバーサイド分析ログのデータフローを示す図](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-server-side-logging.png)

## [!DNL Target] グローバル設定の設定方法

### at.jsの使用

at.js ライブラリの設定は、[!DNL Target] UIやREST APIを使用して設定を設定するのではなく、`window.targetGlobalSettings,`を使用して上書きできます。

オーバーライドは、at.jsが読み込まれる前に、または管理/実装/at.js設定を編集/コード設定/ライブラリヘッダーで定義する必要があります。

例：

```javascript
window.targetGlobalSettings = {  
   timeout: 200, // using custom timeout  
   visitorApiTimeout: 500, // using custom API timeout  
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com  
};
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetgobalsettings.html)

### [!DNL Platform Web SDK]の使用中

この機能は、Web SDKではサポートされていません。

## [!DNL Target] プロファイル属性の更新方法

### at.jsの使用

**例 1**

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox",
   params: {
     "profile.name": "test",
     "profile.gender": "female"
   },
   success: console.log,
   error: console.error
});
```

**例 2**

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {
          profileParameters: {
            name: "test",
            gender: "female"
          }
        }
    }
  }
})
.then(console.log)
.catch(console.error);
```

### [!DNL Platform Web SDK]の使用中

[!DNL Target] プロファイルを更新するには、`sendEvent` コマンドを使用して`data.__adobe.target` プロパティを設定し、`profile.`を使用してキー名の先頭に付けます

**例**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  data: {
    __adobe: {
      target: {
        "profile.gender": "female",
        "profile.age": 30
      }
    }
  }
});
```

## [!DNL Target Recommendations]の使用方法

### at.jsの使用

**例 1**

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox",
   params: {
     "entity.name": "T-shirt",
     "entity.id": "1234"
   },
   success: console.log,
   error: console.error
});
```

**例 2**

```javascript
adobe.target.getOffers({
    request: {
      execute: {
        pageLoad: {
          parameters: {
            "entity.name": "T-shirt",
            "entity.id": "1234"
          }
        }
    }
  }
})
.then(console.log)
.catch(console.error);
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/adobe-target-getoffers-atjs-2.html)

### [!DNL Platform Web SDK]の使用中

[!DNL Recommendations] データを送信するには、`sendEvent` コマンドを使用して`data.__adobe.target` プロパティを設定し、`entity.`を使用してキー名の先頭に付けます

**例**

```javascript
alloy("sendEvent", {
  renderDecisions: true,
  data: {
    __adobe: {
      target: {
        "entity.name": "T-shirt",
        "entity.id": "1234"
      }
    }
  }
});
```

## サードパーティ IDの使用方法

### at.jsの使用

at.jsを使用すると、`getOffer,`または`getOffers`を使用して`mbox3rdPartyId`を送信する方法が複数あります。

**例 1**

```javascript
adobe.target.getOffer({
  mbox:"test",
  params:{
    "mbox3rdPartyId": "1234"
  },
  success: console.log,
  error: console.error
});
```

**例 2**

```javascript
adobe.target.getOffers({
    request: {
      id:{
        thirdPartyId: "1234"
      },
      execute: {
        pageLoad: {}
    }
  }
})
.then(console.log)
.catch(console.error);
```

または、`targetPageParams`または`targetPageParamsAll.`のいずれかで`mbox3rdPartyId`を設定する方法があります

`targetPageParams`を設定すると、`pag-lLoad`とも呼ばれる`target-global-mbox`のリクエストが送信されます。

レコメンデーションは、[!DNL Target] リクエストごとに送信されるため、`targetPageParamsAll`を使用して設定する必要があります。 `targetPageParamsAll`を使用する利点は、ページ上の`mbox3rdPartyId`を1回定義して、すべての[!DNL Target] リクエストに適切な`mbox3rdPartyId.`を割り当てることができることです

```javascript
window.targetPageParamsAll = function() {
      return {
        "mbox3rdPartyId": "1234"
      };
    };
```

```javascript
window.targetPageParams = function() {
  return {
    "mbox3rdPartyId": "1234"
  };
};
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetpageparams.html)

### [!DNL Platform Web SDK]の使用中

[!DNL Platform Web SDK]は[!DNL Target]のサードパーティ IDをサポートしています。 しかし、それにはいくつかの手順が必要です。

ID マップを使用すると、顧客は複数のIDを送信できます。 すべてのIDには名前空間が設定されています。 各名前空間には、1つ以上のIDを含めることができます。 特定のIDをプライマリとしてマークできます。 この知識を念頭に置いて、[!DNL Platform Web SDK]で[!DNL Target]のサードパーティ IDを使用するために必要な手順を確認できます。

1. データストリーム設定ページで[!DNL Target] サードパーティ IDを含む名前空間を設定します。

![&#x200B; ターゲット サードパーティ ID名前空間フィールドを表示するデータストリーム UI](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

1. 次のように、`sendEvent` コマンドごとにID名前空間を送信します。

```javascript
alloy("sendEvent", {
  "renderDecisions": true,
  "xdm": {
    "identityMap": {
      "TGT3PID": [
        {
          "id": "1234",
          "primary": true
        }
      ]
    }
  }
});
```

## プロパティトークンの設定方法

### at.jsの使用

at.jsを使用してプロパティトークンを設定する方法は2つあります。`targetPageParams`または`targetPageParamsAll.`を使用して`targetPageParams`を使用すると、`target-global-mbox`呼び出しにプロパティトークンが追加されますが、`targetPageParamsAll`を使用すると、[!DNL Target]呼び出しすべてにトークンが追加されます。

**例 1**

```javascript
   window.targetPageParamsAll = function() {
      return {
        "at_property": "1234"
      };
    };
```

**例 2**

```javascript
window.targetPageParams = function() {
      return {
        "at_property": "1234"
      };
    };
```

### [!DNL Platform Web SDK]の使用中

[!DNL Platform Web SDK]を使用している顧客は、データストリーム設定を設定する際に、[!DNL Adobe Target]名前空間の下でプロパティをより高いレベルで設定できます。

Adobe Targetの設定を示す![&#x200B; データストリーム UI。](/help/dev/implement/client-side/aep-web-sdk/assets/at-property-setup.png)

つまり、特定のデータストリーム設定に対する[!DNL Target]呼び出しごとに、そのプロパティトークンが含まれています。

## mboxのプリフェッチ方法

### at.jsの使用

この機能は、at.js 2.xでのみ使用できます。 at.js 2.xには、`getOffers`という名前の新しい関数があります。 `getOffers`関数を使用すると、1つ以上のmboxのコンテンツを先行取得できます。 次に例を示します。

```javascript
adobe.target.getOffers({
    request: {
      prefetch: {
        mboxes: [{
          index: 0,
          name: "test-mbox",
          parameters: {
            ...
          },
          profileParameters: {
            ...
          }
        }]
    }
  }
})
.then(console.log)
.catch(console.error);
```

>[!NOTE]
>
>Adobeでは、`mboxes`配列内のすべての`mbox`に独自のインデックスがあることを確認することをお勧めします。 通常、最初のmboxには`index=0`、次のmboxには`index=1,`などが含まれます。

### [!DNL Platform Web SDK]の使用中

この機能は現在[!DNL Platform Web SDK]ではサポートされていません。

## [!DNL Target]実装のデバッグ方法

### at.jsの使用

at.js ライブラリは、次のデバッグ機能を公開します。

* Mbox無効 – [!DNL Target]の取得とレンダリングを無効にして、[!DNL Target]の操作なしでページが壊れているかどうかを確認します
* Mbox デバッグ - at.jsはアクションごとにログを記録します
* ターゲット トレース – 決定プロセスに参加した詳細を含むトレース オブジェクトで生成されたmbox トレース トークンは、`window.___target_trace` オブジェクトで利用できます。

>[!NOTE]
>
>これらのデバッグ機能はすべて、[Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob)の強化機能で使用できます。

### [!DNL Platform Web SDK]の使用中

[!DNL Platform Web SDK]を使用する場合、複数のデバッグ機能があります：

* [Assurance](https://experienceleague.adobe.com/ja/docs/experience-platform/assurance/home)を使用しています
* [Web SDK デバッグ有効](https://experienceleague.adobe.com/ja/docs/experience-platform/assurance/home)
* [Web SDK モニタリングフックを使用](https://github.com/adobe/alloy/wiki/Monitoring-Hooks)
* [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/en/docs/experience-platform/debugger/home)を使用
* ターゲットトレース
