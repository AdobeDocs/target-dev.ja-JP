---
title: at.js とExperience Platform web SDKの比較
description: at.js 機能と  [!DNL Experience Platform Web SDK] の比較を説明します。
keywords: target;adobe target;activity.id;experience.id;renderDecisions;decisionScopes；事前非表示スニペット；vec；フォームベースの Experience Composer;xdm；オーディエンス；決定；範囲；スキーマ；システム図；図
feature: AEP Web SDK
source-git-commit: d6b93537692a1efbc2650a015f5a44d4fd1fd422
workflow-type: tm+mt
source-wordcount: '2006'
ht-degree: 5%

---

# at.js ライブラリと [!DNL Adobe Experience Platform Web SDK] の比較

## 概要

ここでは、`at.js` ライブラリとExperience Platform Web SDKの違いの概要を説明します。

## ライブラリのインストール

### at.js のインストール

お客様 [!DNL Adobe]、[!DNL Adobe Experience Cloud] の「」タブから直接ライブラリをダウンロード [!UICONTROL Implementation] きます。 at.js ライブラリは、顧客が持つ設定（clientCode、imsOrgId など）によってカスタマイズされます。

### Web SDKのインストール

事前ビルドバージョンは CDN で使用できます。 CDN のライブラリをページで直接参照するか、ダウンロードして独自のインフラストラクチャにホストすることができます。 縮小形式と非縮小形式で利用できます。 デバッグの目的では、非縮小バージョンが役立ちます。

詳しくは [JavaScript ライブラリを使用した web SDKのインストール ](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/install/library) を参照してください。

## ライブラリの設定

### at.js の設定

すべての at.js ファイルの末尾に、[!DNL Adobe] がインスタンス化して設定オブジェクトを渡すセクションが表示されます。 このセクションはカスタマイズ可能で、ダウンロード時に、Adobeによって現在のお客様の設定がセクションに入力されます。

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

SDKの設定は、[`configure`](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/commands/configure/overview) コマンドで行います。 `configure` コマンドは *常に* 最初に呼び出されます。

## ページ読み込み [!DNL Target] オファーをリクエストし自動的にレンダリングする方法

### at.js の使用

at.js 2.x では、ライブラリトリガー`pageLoadEnabled,` 設定を有効にすると、[!DNL Target] を使用したEdge`execute -> pageLoad` の呼び出しが実行されます。 すべての設定がデフォルト値に設定されている場合、カスタムコーディングは必要ありません。 at.js がページに追加され、ブラウザーによって読み込まれると、[!DNL Target] Edge呼び出しが実行されます。

### [!DNL PLatform Web SDK] の使用

[!DNL Target] [Visual Experience Composer](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/visual-experience-composer) 内で作成されたコンテンツは、SDKによって自動的に取得およびレンダリングできます。

[!DNL Target] オファーをリクエストして自動的にレンダリングするには、`sendEvent` コマンドを使用し、「`renderDecisions`」オプションを「`true.`」に設定します。これにより、自動レンダリングの対象となるパーソナライズされたコンテンツがSDKで自動的にレンダリングされます。

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

[!DNL Experience Platform Web SDK] によって実行されたオファーを含む通知を自動的に送信し [!DNL Platform WEB SDK] す。 次に、通知リクエストペイロードの例を示します。

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

## ページ読み込み Target オファーをリクエストおよび *NOT* 自動的にレンダリングする方法

### at.js の使用

ページ読み込み用にオファーを取得するEdge[!DNL Target] 呼び出しを実行する方法は 2 つあります。

例 1:

```javascript
adobe.target.getOffer({
   mbox: "target-global-mbox", 
   success: console.log,
   error: console.error
});
```

例 2:

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

### [!DNL Platform Web SDK] の使用

`sendEvent`: `decisionScopes` の下で、特別なスコープを持つ `__view__` コマンドを実行します。 [!DNL Adobe] は、このスコープをシグナルとして使用し、[!DNL Target] からすべてのページ読み込みアクティビティを取得し、すべてのビューをプリフェッチします。 また、[!DNL Platform Web SDK] は、すべての VEC ビューベースのアクティビティの評価も試みます。 ビューのプリフェッチの無効化は、現在 [!DNL Platform Web SDK] ではサポートされていません。

パーソナライゼーションコンテンツにアクセスするには、コールバック関数を指定します。この関数は、SDKがサーバーから正常に応答を受け取った後に呼び出されます。 コールバックには、返されたパーソナライゼーションコンテンツを含む propositions プロパティを含む結果オブジェクトが提供されます。

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

## 特定のフォームベースのターゲット mbox のリクエスト方法

### at.js の使用

`getOffer` の関数を使用して、f アクティビティを取得できます。

例 1:

```javascript
adobe.target.getOffer({
   mbox: "hero-banner", 
   success: console.log,
   error: console.error
});
```

例 2:

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

### [!DNL Platform Web SDK] の使用

[!UICONTROL Form-Based Composer] コマンドを使用して、`sendEvent` オプションの下にある mbox 名を渡すことで、`decisionScopes` ベースのアクティビティを取得できます。 `sendEvent` コマンドは、リクエストされたアクティビティ/提案を含むオブジェクトで解決される promise を返します。

このコードスニペットは、`propositions` 配列がどのように表示されるかを示しています。

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

### at.js の使用

[!DNL Target] の関数を使用して、`applyOffers` のアクティビティを適用できます。`adobe.target.applyOffer(options).`

例：

```javascript
adobe.target.getOffers({...})
  .then(response => adobe.target.applyOffers({ response: response }))
  .then(() => console.log("Success"))
  .catch(error => console.log("Error", error));
```

`applyOffers` コマンドについて詳しくは、[ 専用ドキュメント ](https://experienceleague.adobe.com/en/docs/target-dev/developer/client-side/at-js-implementation/functions-overview/adobe-target-applyoffers-atjs-2) を参照してください。

### [!DNL Platform Web SDK] の使用

[!DNL Target] コマンドを使用して、`applyPropositions` アクティビティを適用できます。

例：

```javascript
alloy("applyPropositions", {
    propositions: [...]
});
```

`applyPropositions` コマンドについて詳しくは、[ 専用ドキュメント ](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/personalization/rendering-personalization-content) を参照してください。

## イベントの追跡方法

### at.js の使用

`trackEvent` 関数を使用するか `sendNotifications.` を使用すると、イベントを追跡できます

この関数は、クリックやコンバージョンなどのユーザーアクションを報告するリクエストを実行します。この関数は、応答内のアクティビティを配信しません。

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

### [!DNL Platform Web SDK] の使用

イベントおよびユーザーアクションを追跡するには、`sendEvent` コマンドを呼び出し、`_experience.decisioning.propositions` XDM `fieldgroup` にデータを入力し、`eventType` を 2 つの値のいずれかに設定します。

* `decisioning.propositionDisplay`: [!DNL Target] アクティビティのレンダリングをシグナルで通知します。
* `decisioning.propositionInteract`：マウスクリックなど、アクティビティに対するユーザーのインタラクションを示します。

`_experience.decisioning.propositions` XDM `fieldgroup` はオブジェクトの配列です。 各オブジェクトのプロパティは、`result.propositions` のコマンドで返される `sendEvent` から派生します。`{ id, scope, scopeDetails }.`

**例 1 - アクティビティのレンダリング後に `decisioning.propositionDisplay` イベントを追跡**

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

**例 2 - クリック指標が発生した後の `decisioning.propositionInteract` イベントの追跡**

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

**例 3 - アクションを実行した後に発生したイベントの追跡**

この例では、ボタンのクリックなど、特定のアクションの実行後に発生したイベントを追跡します。
`__adobe.target` データオブジェクトを介して、追加のカスタムパラメーターを追加できます。

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

### at.js の使用

`adobe.target.triggerView` 関数を使用します。 この関数は、新しいページが読み込まれるときや、ページ上のコンポーネントが再レンダリングされるときに呼び出すことができます。`adobe.target.triggerView()` 関数は、[!UICONTROL Visual Experience Composer] （VEC）を使用して [!UICONTROL A/B Test] and [!UICONTROL Experience Targeting] （XT）アクティビティを作成するために、単一ページアプリケーション（SPA）に実装する必要があります。 `adobe.target.triggerView()` がサイトに実装されていない場合、VEC は SPA には使用できません。

**例**

```javascript
adobe.target.triggerView("homeView")
```

[詳細情報](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)

### [!DNL Platform Web SDK] の使用

シングルページアプリケーション [!UICONTROL View Change] ードをトリガーまたはシグナルで送信するには、`web.webPageDetails.viewName` コマンドの `xdm` オプションで `sendEvent` プロパティを設定します。 [!DNL Platform Web SDK] は、で指定されたビューにオファーがある場合、ビューのキャッシ `viewName` を確認 `sendEvent`、そのオファーを実行して表示通知イベントを送信します。

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

## [!UICONTROL Response Tokens] の活用方法

[!DNL Target] から返されるPersonalization コンテンツには [ レスポンストークン ](https://experienceleague.adobe.com/en/docs/target/using/administer/response-tokens) が含まれます。 応答トークンとは、アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、地域情報などに関する詳細のことです。 これらの詳細は、サードパーティのツールと共有したり、デバッグに使用したりできます。 レスポンストークンは、[!DNL Target] ユーザーインターフェイスで設定できます。

### at.js の使用

at.js カスタムイベントを使用して [!DNL Target] 応答をリッスンし、応答トークンを読み取ります。

**例**

```javascript
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(e) { 
  console.log("Request succeeded", e.detail); 
}); 
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)

### [!DNL Platform Web SDK] の使用

>[!IMPORTANT]
>
>[!DNL Experience Platform Web SDK] バージョン 2.6.0 以降を使用していることを確認します。

応答トークンは、`propositions` コマンドの結果で公開される `sendEvent` の一部として返されます。 各提案には `items,` の配列が含まれ、`meta` 管理 UI で有効になっている場合、各項目には応答トークンが入力された [!DNL Target] オブジェクトがあります。 [詳細情報](https://experienceleague.adobe.com/en/docs/target/using/administer/response-tokens)

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

### at.js の使用

at.js を使用すると、at.js が処理するように `bodyHidingEnabled: true` を設定することで、ちらつきを管理できます
dom の変更を取得して適用する前に、パーソナライズされたコンテナを事前に非表示にします。

パーソナライズされたコンテンツを含むページセクションは、at.js `bodyHiddenStyle.` を上書きすることで事前に非表示にすることができます

デフォルトでは、`bodyHiddenStyle` はHTML `body.` ージ全体を非表示にします

どちらの設定も `window.targetGlobalSettings.`at.js を読み込む前に指定す `window.targetGlobalSettings` 必要があるため、上書きできます。

### [!DNL Platform Web SDK] の使用

[!DNL Platform Web SDK] を使用すると、次の例のように、configure コマンドで事前非表示スタイルを設定できます。

```javascript
alloy("configure", {
  datastreamId: "configurationId",
  orgId: "orgId@AdobeOrg",
  debugEnabled: true,
  prehidingStyle: "body { opacity: 0 !important }"
});
```

[!DNL Platform Web SDK] async を読み込む場合、[!DNL Adobe] では、次のスニペットを、挿入する前にページに挿入するこ [!DNL Platform Web SDK] をお勧めします。

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

## A4T の処理方法

### at.js の使用

at.js を使用してサポートされている A4T ログには、次の 2 種類があります。

* Analytics クライアントサイドログ
* Analytics サーバーサイドログ

#### Analytics クライアントサイドログ

**例 1：グローバル設定 [!DNL Target] 使用**

Analytics のクライアントサイドログは、at.js 設定で `analyticsLogging: client_side` を設定するか、`window.targetglobalSettings` オブジェクトを上書きすることで有効にできます。

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

その後、ペイロードは [!DNL Analytics] を介して [!DNL &#x200B; Data Insertion API] に転送できます。

例 2：すべての `getOffers` 関数で設定する

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

このコードスニペットは、応答ペイロードを次のように表示します。

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

[!DNL Analytics] ペイロード（`tnta` トークン）は、[!DNL Analytics]Data Insertion API[ を使用して、](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) ヒットに含める必要があります。

#### [!DNL Analytics] サーバーサイドログ

[!DNL Analytics] サーバーサイドログは、at.js 設定で `analyticsLogging: server_side` を設定するか、`window.targetglobalSettings` オブジェクトを上書きすることで有効にできます。

その後、データは次のようにフローします。

![Analytics サーバーサイドログのワークフローを示す図 ](/help/dev/implement/client-side/aep-web-sdk/assets/a4t-server-side-atjs.png)

[ 詳細情報 ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4timplementation.html)

### [!DNL Platform Web SDK] の使用

Web SDKは、次の項目もサポートしています。

* Analytics クライアントサイドログ
* Analytics サーバーサイドログ

#### [!DNL Analytics] クライアントサイドログ

[!DNL Analytics] クライアントサイドログは、DataStream 設定で [!DNL Adobe Analytics] が無効になっている場合に有効になります。

![Analytics クライアントサイドログのワークフローを示す図 ](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-disabled-datastream-config.png)

顧客は [!DNL Analytics] トークン（`tnta`）にアクセスできます。このトークンは、[!DNL Analytics]Data Insertion API[ を使用して ](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) と共有する必要があります。`sendEvent` コマンドを連鎖し、結果として得られる提案配列を繰り返し処理します。

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

次の図は、クライアントサイドが有効な場合のデータフロー [!DNL Analytics] 示しています。

![Analytics クライアントサイドログのデータフロー図 ](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-client-side-logging.png)

#### [!DNL Analytics] サーバーサイドログ

[!DNL Analytics] サーバーサイドログは、その DataStream 設定に対して [!DNL Analytics] が有効になっている場合に有効になります。

![Analytics 設定を示すデータストリーム UI。](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-enabled-datastream-config.png)

サーバーサイド [!DNL Analytics] ログが有効な場合、[!DNL Analytics] レポートが正しいインプレッション数を示し、コンバージョンがEdge Network レベルで共有されるように、[!DNL Analytics] と共有する必要がある A4T ペイロードを指定します。これにより、お客様は追加の処理を行う必要がなくなります。

サーバーサイド分析ログが有効な場合のシステムへのデータのフローは次のとおりです。

![ サーバーサイド分析ログのデータフローを示す図 ](/help/dev/implement/client-side/aep-web-sdk/assets/analytics-server-side-logging.png)

## グローバル設定 [!DNL Target] 設定方法

### at.js の使用

`window.targetGlobalSettings,` UI や REST API を使用して設定を構成する代わりに、[!DNL Target] を使用して at.js ライブラリの設定を上書きできます。

上書きは、at.js が読み込まれる前か、管理/実装/at.js 設定を編集/コード設定/ライブラリヘッダーで定義する必要があります。

例：

```javascript
window.targetGlobalSettings = {  
   timeout: 200, // using custom timeout  
   visitorApiTimeout: 500, // using custom API timeout  
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com  
};
```

[詳細情報](https://experienceleague.adobe.com/docs/target/using/implement-target/client-side/at-js-implementation/functions-overview/targetgobalsettings.html)

### [!DNL Platform Web SDK] の使用

この機能は Web SDKではサポートされていません。

## プロファイル属性 [!DNL Target] 更新方法

### at.js の使用

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

### [!DNL Platform Web SDK] の使用

[!DNL Target] プロファイルを更新するには、`sendEvent` コマンドを使用して、`data.__adobe.target` を使用してキー名のプレフィックスとして `profile.` プロパティを設定します

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

## [!DNL Target Recommendations] の使用方法

### at.js の使用

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

### [!DNL Platform Web SDK] の使用

[!DNL Recommendations] データを送信するには、`sendEvent` コマンドを使用して、`data.__adobe.target` プロパティを設定し、`entity.` を使用してキー名のプレフィックスを指定します

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

## サードパーティ ID の使用方法

### at.js の使用

at.js の使用 `mbox3rdPartyId` または `getOffer,` を使用して `getOffers` を送信する方法は複数あります。

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

または、`mbox3rdPartyId` または `targetPageParams` のいずれかで `targetPageParamsAll.` を設定する方法があります

`targetPageParams` を設定すると、は、`target-global-mbox` とも呼ばれる `pag-lLoad` のリクエストを送信します。

レコメンデーションは、`targetPageParamsAll` リクエストのたびに送信されるので、[!DNL Target] を使用して設定する必要があります。 `targetPageParamsAll` を使用する利点は、ページ上で `mbox3rdPartyId` を 1 回定義して、すべての [!DNL Target] リクエストが適切な `mbox3rdPartyId.` を持つようにできることです

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

### [!DNL Platform Web SDK] の使用

[!DNL Platform Web SDK] はサードパーティ ID[!DNL Target] サポートしています。 ただし、もう 2、3 の手順が必要です。

顧客は ID マップを使用して複数の ID を送信できます。 すべての ID に名前空間が設定されています。 各名前空間には、1 つ以上の ID を設定できます。 特定の ID をプライマリとしてマークできます。 この知識を念頭に置くと、[!DNL Platform Web SDK] サードパーティ ID を使用するために [!DNL Target] を設定するために必要な手順を確認できます。

1. データストリーム設定ページで、[!DNL Target] のサードパーティ ID を含む名前空間を設定します。

![ 「ターゲットサードパーティ ID 名前空間」フィールドを示すデータストリーム UI](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

1. 次のように、すべての `sendEvent` コマンドで、その ID 名前空間を送信します。

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

### at.js の使用

at.js を使用してプロパティトークンを設定するには、`targetPageParams` を使用するか、`targetPageParamsAll.` を使用します。`targetPageParams` を使用すると、`target-global-mbox` 呼び出しにプロパティトークンが追加されますが、`targetPageParamsAll` を使用すると、[!DNL Target] のすべての呼び出しにトークンが追加されます。

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

### [!DNL Platform Web SDK] の使用

[!DNL Platform Web SDK] を使用すると、お客様は、データストリーム設定を設定する際に、[!DNL Adobe Target] の名前空間の下に、より高いレベルでプロパティを設定できます。

![Adobe Targetの設定を示すデータストリーム UI。](/help/dev/implement/client-side/aep-web-sdk/assets/at-property-setup.png)

つまり、その特定のデータストリーム設定に対するすべての [!DNL Target] 呼び出しには、そのプロパティトークンが含まれます。

## mbox をプリフェッチする方法

### at.js の使用

この機能は at.js 2.x でのみ使用できます。at.js 2.x には、`getOffers` という名前の新しい関数があります。 `getOffers` 関数を使用すると、1 つ以上の mbox のコンテンツをプリフェッチできます。 次に例を示します。

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
>Adobeでは、`mbox` 配列のすべての `mboxes` に独自のインデックスを設定することをお勧めします。 通常、最初の mbox は次の mbox を `index=0` し、それ以降も同様 `index=1,` 処理を行います。

### [!DNL Platform Web SDK] の使用

この機能は、現在 [!DNL Platform Web SDK] ではサポートされていません。

## [!DNL Target] 実装をデバッグする方法

### at.js の使用

at.js ライブラリは、次のデバッグ機能を公開します。

* mbox 無効化 – [!DNL Target] が取得とレンダリングを無効にして、インタラクションを行わずにページが破損しているかどうか [!DNL Target] 確認します
* mbox のデバッグ - at.js はすべてのアクションをログに記録します
* Target Trace – 決定プロセスに関与した詳細を含むトレースオブジェクトで生成された mbox トレーストークンを使用すると、オブジェクトの下で使用でき `window.___target_trace` す。

>[!NOTE]
>
>これらすべてのデバッグ機能は、[Adobe Experience Platform Debugger](https://chrome.google.com/webstore/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) の拡張機能で利用できます。

### [!DNL Platform Web SDK] の使用

[!DNL Platform Web SDK] を使用する場合、複数のデバッグ機能があります。

* [Assurance](https://experienceleague.adobe.com/en/docs/experience-platform/assurance/home) の使用
* [Web SDKのデバッグが有効になりました ](https://experienceleague.adobe.com/en/docs/experience-platform/assurance/home)
* [Web SDK モニタリングフック ](https://github.com/adobe/alloy/wiki/Monitoring-Hooks) の使用
* [Adobe Experience Platform Debugger](https://experienceleague.adobe.com/en/docs/experience-platform/debugger/home) の使用
* ターゲット トレース