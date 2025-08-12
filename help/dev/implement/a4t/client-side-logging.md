---
title: Experience Platform Web SDKでの A4T データのクライアントサイドログ
description: Experience Platform web SDKを使用して、Adobe Analytics for Target （A4T）のクライアントサイドログを有効にする方法を説明します。
seo-title: Client-side logging for A4T data in the Experience Platform Web SDK
seo-description: Learn how to enable client-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: target;a4t；ログ；web sdk；エクスペリエンス；platform;
feature: Implementation
source-git-commit: 4d4ca7dcffdbaee5770084bd85c3109df0d6a8d4
workflow-type: tm+mt
source-wordcount: '996'
ht-degree: 0%

---

# [!DNL Experience Platform Web SDK] での A4T データのクライアントサイドログ

[!DNL Adobe Experience Platform Web SDK] を使用すると、web アプリケーションのクライアント側で [Adobe Analytics for Target （A4T） ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja) データを収集できます。

クライアントサイドログとは、関連する [!DNL Target] データがクライアントサイドで返され、データを収集して [!DNL Analytics] と共有できることを意味します。 [Data Insertion API](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html?lang=ja) を使用して手動で Analytics にデータを送信する場合は、このオプションを有効にしてください。

>[!NOTE]
>
>[AppMeasurement.js](https://experienceleague.adobe.com/docs/analytics/implementation/js/overview.html?lang=ja) を使用してこれを実行する方法は、現在開発中で、近い将来に利用可能になる予定です。

このドキュメントでは、[!DNL Platform Web SDK] のクライアントサイド A4T ログの設定手順と、一般的なユースケースの実装例を説明します。

## 前提条件 {#prerequisites}

このチュートリアルは、パーソナライゼーションのために [!DNL Platform Web SDK] を使用することに関連する基本的な概念とプロセスについて、十分に理解していることを前提としています。 概要については、次のドキュメントを確認してください。

* [Web SDKの設定 ](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/commands/configure/overview)
* [ イベントの送信 ](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/commands/sendevent/overview)
* [ パーソナライゼーションコンテンツのレンダリング ](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## クライアントサイドログ [!DNL Analytics] 設定 {#set-up-client-side-logging}

次のサブセクションでは、[!DNL Analytics] 実装でクライアント側ログ [!DNL Platform Web SDK] 有効にする方法の概要を説明します。

### [!DNL Analytics] クライアントサイドログを有効にする {#enable-analytics-client-side-logging}

実装でクライアントサイド [!DNL Analytics] ログが有効になっていることを考慮するには、[!DNL Adobe Analytics] データストリーム [ で ](https://experienceleague.adobe.com/ja/docs/experience-platform/datastreams/overview) 設定を無効にする必要があります。

![Analytics データストリーム設定が無効 ](/help/dev/implement/a4t/assets/disable-analytics-datastream.png)

### SDKから [!DNL A4T] データを取得して、[!DNL Analytics] に送信します {#a4t-to-analytics}

このレポート方法が正しく機能するには、[!DNL A4T][`sendEvent` コマンドから取得した ](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/commands/sendevent/overview) 関連データを [!DNL Analytics] ヒットで送信する必要があります。

Edge[!DNL Target]propositions 応答を計算する際に、クライアントサイドのログが有効 [!DNL Analytics] なっているかどうか（例えば、データストリームで [!DNL Analytics] が無効になっているかどうか）を確認します。 クライアントサイドログが有効になっている場合、システムは応答の各提案に [!DNL Analytics] トークンを追加します。

フローは次のようになります。

![ クライアントサイドログフロー ](/help/dev/implement/a4t/assets/analytics-client-side-logging.png)

次に、クライアントサイドログが有効になってい `interact` 場合の [!DNL Analytics] 応答の例を示します。 レポートを持つアクティビティに関する提案の場合 [!DNL Analytics]、`scopeDetails.characteristics.analyticsToken` のプロパティが表示されます。

```json
{
  "requestId": "1234",
  "handle": [
    {
      "payload": [
        {
          "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
          "scope": "a4t-test",
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
              "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
              "analyticsToken": "434689:0:0|2,434689:0:0|1"
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
          "scope": "a4t-test",
          "scopeDetails": {
            "decisionProvider": "TGT",
            "activity": {
              "id": "434689"
            },
            "characteristics": {
              "eventToken": "E0gb6q1+WyFW3FMbbQJmrg==",
              "analyticsToken": "434689:0:0|32767"
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
      ],
      "type": "personalization:decisions",
      "eventIndex": 0
    }
  ]
}
```

[!UICONTROL Form-based Experience Composer] のアクティビティの提案には、同じ提案のコンテンツとクリック指標項目の両方を含めることができます。 したがって、`scopeDetails.characteristics.analyticsToken` のプロパティでコンテンツ表示用の分析トークンを 1 つだけ用意するのではなく、`scopeDetails.characteristics.analyticsDisplayToken` プロパティと `scopeDetails.characteristics.analyticsClickToken` プロパティで、表示用とクリック用分析トークンの両方を指定することができます。

```json
{
  "requestId": "1234",
  "handle": [
    {
      "payload": [
        {
          "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
          "scope": "a4t-test",
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
               "displayToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
               "clickToken": "E0gb6q1+WyFW3FMbbQJmrg==",
               "analyticsDisplayToken": "434689:0:0|2,434689:0:0|1", 
               "analyticsClickToken": "434689:0:0|32767"
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
            },
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
      ],
      "type": "personalization:decisions",
      "eventIndex": 0
    }
  ]
}
```

`scopeDetails.characteristics.analyticsToken` のすべての値と `scopeDetails.characteristics.analyticsDisplayToken` （表示されたコンテンツの場合）および `scopeDetails.characteristics.analyticsClickToken` （クリック指標の場合）は、A4T ペイロードで、`tnta`Data Insertion API[ 呼び出しでタグとして収集し ](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) 含める必要があります。

>[!IMPORTANT]
>
>`analyticsToken`、`analyticsDisplayToken`、`analyticsClickToken` の各プロパティには、複数のトークンを含め、1 つのコンマで区切られた文字列として連結できます。
>
>次の節に示す実装例では、複数の [!DNL Analytics] トークンが繰り返し収集されています。 [!DNL Analytics] トークンの配列を連結するには、次のような関数を使用します。
>
>```javascript
>var concatenateAnalyticsPayloads = function concatenateAnalyticsPayloads(analyticsPayloads) {
>   if (analyticsPayloads.size > 1) {
>       return [].concat(analyticsPayloads).join(',');
>   }
>   return [].concat(analyticsPayloads).join();
>};
>```

## 実装例 {#implementation-examples}

次のサブセクションでは、一般的なユースケースでのクライアント側ログ [!DNL Analytics] 実装方法について説明します。

### [!UICONTROL Form-Based Experience Composer] アクティビティ {#form-based-composer}

[!DNL Platform Web SDK] を使用して、[Adobe Target フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=ja) アクティビティから提案の実行を制御できます。

特定の決定範囲の提案をリクエストすると、返される提案には、適切な [!DNL Analytics] トークンが含まれます。 ベストプラクティスは、[!DNL Experience Platform Web SDK] `sendEvent` コマンドを連結し、返された提案を繰り返し実行すると同時に、[!DNL Analytics] トークンを収集することです。

`sendEvent` のアクティビティ範囲に対して、[!UICONTROL Form-Based Experience Composer] コマンドを次のようにトリガーできます。

```javascript
alloy("sendEvent", {
    "decisionScopes": ["a4t-test"],
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function(results) {
  for (var i = 0; i < results.propositions.length; i++) {
    //Execute the propositions and collect the Analytics payload
  }
});
```

ここから、提案を実行し、最終的に [!DNL Analytics] に送信されるペイロードを構築するコードを実装する必要があります。 次に、`results.propositions` に含まれる可能性のある例を示します。

```json
[
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "a4t-test",
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
        "eventToken": "2lTS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqbOw==",
        "analyticsToken": "434689:0:0|2,434689:0:0|1"
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
    "scope": "a4t-test",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434689"
      },
      "characteristics": {
        "eventToken": "E0gb6q1+WyFW3FMbbQJmrg==",
        "analyticsToken": "434689:0:0|32767"
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
  },
  {
    "id": "AT:eyJhY3Rpdml0eUlkIjoiNDM0Njg5IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
    "scope": "a4t-test",
    "scopeDetails": {
      "decisionProvider": "TGT",
      "activity": {
        "id": "434688"
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
          "displayToken": "91TS5KA6gj4JuSjOdhqUhGqipfsIHvVzTQxHolz2IpTMromRrB5ztP5VMxjHbs7c6qPG9UF4rvQTJZniWgqgEt==",
          "clickToken": "Tagb6q1+WyFW3FMbbQJrtg==",
          "analyticsDisplayTokens": "434688:0:0|2,434688:0:0|1",
          "analyticsClickTokens": "434688:0:0|32767"
        }
      }
    },
    "items": [
      {
        "id": "1184845",
        "schema": "https://ns.adobe.com/personalization/html-content-item",
        "meta": {
          "geo.state": "bucuresti",
          "activity.id": "434688",
          "experience.id": "0",
          "activity.name": "a4t test form based activity 1",
          "offer.id": "1184845"
        },
        "data": {
          "id": "1184845",
          "format": "text/html",
          "content": "<div> analytics impressions 1</div>"
        }
      },
      {
        "id": "434688",
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

コンテンツ項目を含む提案から [!DNL Analytics] トークンを抽出するには、次のような関数を実装します。

```javascript
function getDisplayAnalyticsPayload(proposition) {
  if (!proposition || !proposition.scopeDetails || !proposition.scopeDetails.characteristics) {
    return;
  }
  var characteristics = proposition.scopeDetails.characteristics;
  if (characteristics.analyticsDisplayToken) {
    return characteristics.analyticsDisplayToken;
  }
  return characteristics.analyticsToken;
}
```

提案には、問題の項目の `schema` プロパティで示されるように、様々なタイプの項目を含めることができます。 [!UICONTROL Form-Based Experience Composer] のアクティビティでサポートされる提案項目スキーマは 4 つあります。

```javascript
var HTML_SCHEMA = "https://ns.adobe.com/personalization/html-content-item";
var MEASUREMENT_SCHEMA = "https://ns.adobe.com/personalization/measurement";
var JSON_SCHEMA = "https://ns.adobe.com/personalization/json-content-item";
var REDIRECT_SCHEMA = "https://ns.adobe.com/personalization/redirect-item";
```

`HTML_SCHEMA` と `JSON_SCHEMA` はオファーのタイプを反映するスキーマで、`MEASUREMENT_SCHEMA` は DOM 要素に添付する必要がある指標を反映します。

クリック指標の [!DNL Analytics] ペイロードは、訪問者が以前に表示されたコンテンツを実際にクリックした瞬間に、コンテンツ項目とは別に収集して [!DNL Analytics] に送信する必要があります。

この場合、クリック指標の A4T ペイロードを取得するための次のヘルパー関数が便利です。

```javascript
function getClickAnalyticsPayload(proposition) {
  if (!proposition || !proposition.scopeDetails || !proposition.scopeDetails.characteristics) {
    return;
  }
  var characteristics = proposition.scopeDetails.characteristics;
  if (characteristics.analyticsClickToken) {
    return characteristics.analyticsClickToken;
  }
  return characteristics.analyticsToken;
}
```

#### 実装の概要 {#implementation-summary}

要約すると、アクティビティを [!UICONTROL Form-Based Experience Composer] で適用する際には、次 [!DNL Experience Platform Web SDK] 手順を実行する必要があります。

1. アクティビティオファーを取得するイベント [!UICONTROL Form-Based Experience Composer] 送信する。
1. ページにコンテンツの変更を適用する。
1. `decisioning.propositionDisplay` 通知イベントの送信
1. SDK応答から [!DNL Analytics] の表示トークンを収集し、[!DNL Analytics] ヒットのペイロードを作成します。
1. [!DNL Analytics]Data Insertion API[ を使用して、ペイロードを ](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) に送信します。
1. 配信された提案にクリック指標がある場合は、クリックが実行されたときに `decisioning.propositionInteract` の通知イベントが送信されるようにクリックリスナーを設定する必要があります。 `onBeforeEventSend` ハンドラーは、イベントをインターセプトすると次のアクションが発生するように設定 `decisioning.propositionInteract` る必要があります。
   1. [!DNL Analytics] からのクリック `xdm._experience.decisioning.propositions` トークンの収集
   1. [!DNL Analytics]Data Insertion API[!DNL Analytics] を介して、収集された [ ペイロードでクリック ](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md) ールヒットを送信する

```javascript
alloy("sendEvent", {
    "decisionScopes": ["a4t-test"],
    "xdm": {
      "web": {
        "webPageDetails": {
          "name": "Home Page"
        }
      }
    }
  }
).then(function(results) {
  var analyticsPayload = new Set();
  results.propositions.forEach(function (proposition) {
    proposition.items.forEach(function (item) {
      if (item.schema === HTML_SCHEMA) {
        // 1. Apply offer
        // 2. Collect executed propositions and send the decisioning.propositionDisplay notification event
        // 3. Collect the display Analytics tokens
      }
      if (item.schema === MEASUREMENT_SCHEMA) {
        // Setup click listener, so that when clicked:
        // 1. Collect clicked propositions and send the decisioning.propositionInteract notification event
        // Note: onBeforeEventSend handler should be configured, so that when intercepting decisioning.propositionInteract events:
        //   1. Collect the click Analytics tokens from xdm._experience.decisioning.propositions
        //   2. Send the click Analytics hit with the collected Analytics payload via Data Insertion API
      }
    });
  });
  // Send the page view Analytics hit with the collected display Analytics payload via Data Insertion API
});
```

### [!UICONTROL Visual Experience Composer] （VEC）アクティビティ {#visual-experience-composer-acitivties}

[!DNL Platform Web SDK] を使用すると、[Visual Experience Composer （VEC） ](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=ja) を使用して作成されたオファーを処理できます。

>[!NOTE]
>
>このユースケースの実装手順は、[ フォームベースの Experience Composer アクティビティ ](#form-based-composer) の手順と非常によく似ています。 詳しくは、前の節を参照してください。

自動レンダリングが有効になっている場合は、ページ上で実行された提案から [!DNL Analytics] のトークンを収集できます。 ベストプラクティスは、[!DNL Experience Platform Web SDK] `sendEvent` コマンドを連結し、返された提案を繰り返し処理して、web SDKでレンダリングしようとしたものをフィルタリングすることです。

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
  
    var proposition = propositions[i];
    var renderAttempted = proposition.renderAttempted;

    if (renderAttempted === true) {
      var analyticsPayload = getDisplayAnalyticsPayload(proposition);
      
      if (analyticsPayload !== undefined) {
        analyticsPayloads.add(analyticsPayload);
      }
    }
  }
  var analyticsPayloadsToken = concatenateAnalyticsPayloads(analyticsPayloads);
  // Send the page view Analytics hit with collected Analytics payload via Data Insertion API
});
```

### `onBeforeEventSend` を使用したページ指標の処理 {#using-onbeforeeventsend}

[!DNL Adobe Target] アクティビティを使用すると、DOM に手動で添付するか、DOM （VEC が作成したアクティビティ）に自動的に添付するかして、ページ上で異なる指標を設定できます。 どちらのタイプも、web ページ上でのエンドユーザーのインタラクションが遅延します。

これに対処するために、ベストプラクティスは、[!DNL Analytics] `onBeforeEventSend` フックを使用して [!DNL Adobe Experience Platform Web SDK] ペイロードを収集することです。 `onBeforeEventSend` フックは、`configure` コマンドを使用して設定する必要があり、データストリームを介して送信されるすべてのイベントに反映されます。

次に、`onBeforeEventSent` ーザーのヒットをトリガー[!DNL Analytics] るように設定する方法の例を示します。

```javascript
alloy("configure", {
  datastreamId: "datastream configuration ID",
  orgId: "adobe ORG ID",
  onBeforeEventSend: function(options) {
    const xdm = options.xdm;
    const eventType = xdm.eventType;
    if (eventType === "decisioning.propositionInteract") {
      const analyticsPayloads = new Set();
      const propositions = xdm._experience.decisioning.propositions;

      for (var i = 0; i < propositions.length; i++) {
        var proposition = propositions[i];
        analyticsPayloads.add(getClickAnalyticsPayload(proposition));
      }
      // Trigger the Analytics hit
    }
  }
});
```

## 次の手順 {#next-steps}

このガイドでは、[!DNL Platform Web SDK] での A4T データのクライアントサイドログについて説明しました。 Edge Networkで A4T データを処理する方法について詳しくは、[ サーバーサイドログ ](/help/dev/implement/a4t/server-side-a4t.md) に関するガイドを参照してください。