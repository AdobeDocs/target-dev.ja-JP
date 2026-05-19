---
title: Experience Platform Web SDKでのA4T データのクライアントサイドログ
description: Experience Platform Web SDKを使用して、Adobe Analytics for Target （A4T）のクライアントサイドログを有効にする方法について説明します。
seo-title: Client-side logging for A4T data in the Experience Platform Web SDK
seo-description: Learn how to enable client-side logging for Adobe Analytics for Target (A4T) using the Experience Platform Web SDK.
keywords: target;a4t；ログ；web sdk;experience;platform;
feature: Implementation
exl-id: fef34eec-128f-4433-a557-42f1347cf2c3
TQID: https://experienceleague.adobe.com/A-6Z757zzqoIW12ICTs9WBwXjHbapgLArhGSoIgMulo
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c2be0313-b3ae-45e0-b454-d20bf54b23f2
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1139
ht-degree: 4%

---

# [!DNL Experience Platform Web SDK]のA4T データのクライアント側ログ

[!DNL Adobe Experience Platform Web SDK]を使用すると、Web アプリケーションのクライアント側で[Adobe Analytics for Target （A4T） &#x200B;](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja) データを収集できます。

クライアントサイドのログとは、クライアントサイドで関連する[!DNL Target] データが返されることを意味します。これにより、データを収集して[!DNL Analytics]と共有できます。 このオプションは、[Data Insertion API](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html?lang=ja)を使用してAnalyticsにデータを手動で送信する場合に有効にする必要があります。

>[!NOTE]
>
>[AppMeasurement.js](https://experienceleague.adobe.com/docs/analytics/implementation/js/overview.html?lang=ja)を使用してこれを実行する方法は現在開発中で、近日中に公開される予定です。

このドキュメントでは、[!DNL Platform Web SDK]のクライアントサイド A4T ログを設定する手順について説明し、一般的なユースケースの実装例を示します。

## 前提条件 {#prerequisites}

このチュートリアルでは、パーソナライゼーションの目的で[!DNL Platform Web SDK]を使用することに関連する基本的な概念とプロセスについて理解していることを前提としています。 概要が必要な場合は、次のドキュメントを参照してください。

* [Web SDKの設定](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/commands/configure/overview)
* [イベントの送信](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/commands/sendevent/overview)
* [パーソナライゼーションコンテンツのレンダリング](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)

## [!DNL Analytics] クライアントサイドのログを設定 {#set-up-client-side-logging}

次のサブセクションでは、[!DNL Platform Web SDK]実装に対して[!DNL Analytics] クライアントサイドのログ記録を有効にする方法について説明します。

### [!DNL Analytics] クライアント側ログを有効にする {#enable-analytics-client-side-logging}

実装で[!DNL Analytics]のクライアントサイドのログ記録が有効になっていることを考慮するには、[&#x200B; データストリーム &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-platform/datastreams/overview)で[!DNL Adobe Analytics]設定を無効にする必要があります。

![Analytics データストリーム設定が無効です](/help/dev/implement/a4t/assets/disable-analytics-datastream.png)

### SDKから[!DNL A4T] データを取得し、[!DNL Analytics]に送信します {#a4t-to-analytics}

このレポート方法を正しく機能させるには、[!DNL Analytics] ヒットで[`sendEvent`](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/commands/sendevent/overview) コマンドから取得した[!DNL A4T]関連データを送信する必要があります。

[!DNL Target] Edgeが提案レスポンスを計算すると、[!DNL Analytics] クライアントサイドのログが有効になっているかどうかを確認します（例えば、[!DNL Analytics]がデータストリームで無効になっている場合）。 クライアント側のログ記録が有効になっている場合、システムは応答の各提案に[!DNL Analytics] トークンを追加します。

フローは次のようになります。

![&#x200B; クライアントサイドのログフロー](/help/dev/implement/a4t/assets/analytics-client-side-logging.png)

次に、[!DNL Analytics] クライアントサイドのログが有効になっている場合の`interact`応答の例を示します。 提案が[!DNL Analytics]件のレポートを持つアクティビティ用の場合、`scopeDetails.characteristics.analyticsToken`個のプロパティがあります。

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

[!UICONTROL Form-based Experience Composer] アクティビティの提案には、同じ提案の下にコンテンツとクリック指標の両方の項目を含めることができます。 したがって、`scopeDetails.characteristics.analyticsToken` プロパティにコンテンツ表示用の分析トークンを1つ持つ代わりに、対応して、`scopeDetails.characteristics.analyticsDisplayToken`および`scopeDetails.characteristics.analyticsClickToken` プロパティで表示トークンとクリック分析トークンの両方を指定できます。

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

`scopeDetails.characteristics.analyticsToken`からのすべての値、および`scopeDetails.characteristics.analyticsDisplayToken` （表示されたコンテンツの場合）と`scopeDetails.characteristics.analyticsClickToken` （クリック指標の場合）は、収集し、[Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)呼び出しに`tnta` タグとして含める必要があるA4T ペイロードです。

>[!IMPORTANT]
>
>`analyticsToken`、`analyticsDisplayToken`、`analyticsClickToken` プロパティには、複数のトークンを含めることができ、1つのコンマ区切り文字列として連結できます。
>
>次の節で提供する実装例では、複数の[!DNL Analytics] トークンが反復的に収集されています。 [!DNL Analytics] トークンの配列を連結するには、次のような関数を使用します。
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

次のサブセクションでは、一般的なユースケースに対して[!DNL Analytics] クライアントサイドのログを実装する方法を示します。

### [!UICONTROL Form-Based Experience Composer]件のアクティビティ {#form-based-composer}

[!DNL Platform Web SDK]を使用して、[Adobe Target フォームベースのExperience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=ja) アクティビティからの提案の実行を制御できます。

特定の決定範囲に対する提案をリクエストする場合、返される提案には適切な[!DNL Analytics] トークンが含まれます。 ベストプラクティスは、[!DNL Experience Platform Web SDK] `sendEvent` コマンドをチェーンし、返された提案を繰り返し実行して、[!DNL Analytics] トークンを同時に収集しながら実行することです。

次のように、[!UICONTROL Form-Based Experience Composer] アクティビティスコープの`sendEvent` コマンドをトリガーできます。

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

ここから、提案を実行し、最終的に[!DNL Analytics]に送信されるペイロードを構築するためのコードを実装する必要があります。 次に、`results.propositions`の例を示します。

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

コンテンツ項目を含む提案から[!DNL Analytics] トークンを抽出するには、次のような関数を実装できます。

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

提案には、対象となるアイテムの`schema` プロパティで示すように、様々なタイプのアイテムを含めることができます。 [!UICONTROL Form-Based Experience Composer] アクティビティでサポートされている提案項目スキーマは4つあります。

```javascript
var HTML_SCHEMA = "https://ns.adobe.com/personalization/html-content-item";
var MEASUREMENT_SCHEMA = "https://ns.adobe.com/personalization/measurement";
var JSON_SCHEMA = "https://ns.adobe.com/personalization/json-content-item";
var REDIRECT_SCHEMA = "https://ns.adobe.com/personalization/redirect-item";
```

`HTML_SCHEMA`と`JSON_SCHEMA`はオファーのタイプを反映するスキーマですが、`MEASUREMENT_SCHEMA`はDOM要素に添付する必要がある指標を反映しています。

訪問者が以前に表示されたコンテンツを実際にクリックした時点で、クリック指標の[!DNL Analytics] ペイロードを収集し、コンテンツ項目とは別に[!DNL Analytics]に送信する必要があります。

この場合、クリック指標A4T ペイロードを取得するための次のヘルパー関数が便利です。

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

要約すると、[!DNL Experience Platform Web SDK]で[!UICONTROL Form-Based Experience Composer] アクティビティを適用する場合は、次の手順を実行する必要があります。

1. [!UICONTROL Form-Based Experience Composer] アクティビティオファーを取得するイベントを送信します。
1. コンテンツの変更をページに適用します。
1. `decisioning.propositionDisplay`通知イベントを送信します。
1. SDK レスポンスから[!DNL Analytics]表示トークンを収集し、[!DNL Analytics] ヒットのペイロードを構築します。
1. [Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)を使用してペイロードを[!DNL Analytics]に送信します。
1. 配信された提案にクリック指標がある場合は、クリックが実行されたときに`decisioning.propositionInteract`通知イベントが送信されるように、クリックリスナーを設定する必要があります。 `onBeforeEventSend` ハンドラーは、`decisioning.propositionInteract` イベントをインターセプトする際に、次のアクションが発生するように設定する必要があります。
   1. `xdm._experience.decisioning.propositions`からクリックトークン [!DNL Analytics]を収集しています
   1. 収集した[!DNL Analytics] ペイロードを含むクリック [!DNL Analytics] ヒットを[Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)経由で送信します。

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

### [!UICONTROL Visual Experience Composer] （VEC） アクティビティ {#visual-experience-composer-acitivties}

[!DNL Platform Web SDK]を使用すると、[Visual Experience Composer （VEC） &#x200B;](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html?lang=ja)を使用して作成されたオファーを処理できます。

>[!NOTE]
>
>このユースケースを実装する手順は、[&#x200B; フォームベースのExperience Composer アクティビティ &#x200B;](#form-based-composer)の手順とよく似ています。 詳しくは、前の節を参照してください。

自動レンダリングが有効になっている場合は、ページで実行された提案から[!DNL Analytics] トークンを収集できます。 ベストプラクティスは、[!DNL Experience Platform Web SDK] `sendEvent` コマンドをチェーンし、返された提案を繰り返し実行して、Web SDKがレンダリングしようとしているものをフィルタリングすることです。

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

### `onBeforeEventSend`を使用したページ指標の処理 {#using-onbeforeeventsend}

[!DNL Adobe Target] アクティビティを使用すると、ページ上に異なる指標を設定できます。これは、手動でDOMに添付するか、自動的にDOMに添付されます（VECが作成したアクティビティ）。 どちらのタイプも、web ページ上の遅延したエンドユーザーインタラクションです。

これを考慮するため、ベストプラクティスは、`onBeforeEventSend` [!DNL Adobe Experience Platform Web SDK] フックを使用して[!DNL Analytics] ペイロードを収集することです。 `onBeforeEventSend` フックは、`configure` コマンドを使用して設定する必要があり、データストリームを介して送信されるすべてのイベントに反映されます。

次に、`onBeforeEventSent`をトリガー[!DNL Analytics] ヒットに設定する方法の例を示します。

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

## 次のステップ {#next-steps}

このガイドでは、[!DNL Platform Web SDK]のA4T データのクライアント側のログ記録について説明しました。 Edge NetworkでのA4T データの処理方法について詳しくは、[&#x200B; サーバーサイドのログ記録](/help/dev/implement/a4t/server-side-a4t.md)に関するガイドを参照してください。
