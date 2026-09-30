---
title: Adobe Experience Platform Web SDKを使用した応答トークンへのアクセス
description: '[!DNL Adobe Experience Platform Web SDK]で応答トークンにアクセスする方法を説明します。'
keywords: パーソナライゼーション；ターゲット；adobe target;renderDecisions;sendEvent;decisionScopes;result.decisions，応答トークン；
feature: AEP Web SDK
exl-id: b125017c-c257-4f2f-a479-dd0f20e76a9a
TQID: 'https://experienceleague.adobe.com/kqa-HY5-dOvNq-yGqthunYDdyTKkiiFdsHquyN34ERg'
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
    internal-label: Target
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
    internal-label: Implementation
  - id: b050e0cd-2ddd-42cd-a71b-5d9e1fdf75e0
    internal-label: APIs and SDKs
subfeature_v2:
  - id: a1f3c920-a3a8-4506-8067-53189547b5e6
    internal-label: AEP Web SDK
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
    internal-label: Implementation
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
source-git-commit: 5d119ccf18b09b3ba864a69642458597f65c754f
workflow-type: tm+mt
source-wordcount: '273'
ht-degree: 4%
---
# 応答トークンへのアクセス

[!DNL Adobe Target]から返されるPersonalization コンテンツには、[応答トークン &#x200B;](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=ja)が含まれます。このトークンには、アクティビティ、オファー、エクスペリエンス、ユーザープロファイル、位置情報などの詳細が含まれます。 これらの詳細は、サードパーティのツールと共有することも、デバッグに使用することもできます。 応答トークンは、[!DNL Target] ユーザーインターフェイスで設定できます。

パーソナライゼーションコンテンツにアクセスするには、イベントの送信時にコールバック機能を提供します。 このコールバックは、SDKがサーバーから正常な応答を受け取った後に呼び出されます。 コールバックに`result` オブジェクトが指定されています。このオブジェクトには、返されたパーソナライゼーションコンテンツを含む`propositions` プロパティが含まれている場合があります。 以下は、コールバック関数を提供する例です。

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    if (result.propositions) {
      // Manually render propositions
    }
  });
```

この例では、`result.propositions`は、存在する場合、イベントに関連するパーソナライゼーション提案を含む配列です。 `result.propositions.`のコンテンツについて詳しくは、[&#x200B; パーソナライゼーションコンテンツのレンダリング &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)を参照してください

Web SDKによって自動的にレンダリングされたすべての提案からすべてのアクティビティ名を収集し、それらを1つの配列にプッシュするとします。 その後、単一の配列をサードパーティに送信できます。 この場合：

1. `result` オブジェクトから提案を抽出します。
1. 各提案を繰り返します。
1. SDKが提案をレンダリングしたかどうかを確認します。
1. その場合は、提案書の各項目を繰り返します。
1. 応答トークンを含むオブジェクトである`meta` プロパティからアクティビティ名を取得します。
1. アクティビティ名を配列にプッシュします。
1. アクティビティ名をサードパーティに送信します。

コードは次のようになります。

```javascript
alloy("sendEvent", {
    renderDecisions: true,
    xdm: {}
  }).then(function(result) {
    var activityNames = [];
    propositions.forEach(function(proposition) {
      if (proposition.renderAttempted) {
        proposition.items.forEach(function(item) {
          if (item.meta) {
            // item.meta contains the response tokens.
            var activityName = item.meta["activity.name"];
            // Ignore duplicates
            if (activityNames.indexOf(activityName) === -1) {
              activityNames.push(activityName);
            }
          }
        });
      }
    });
    // Now that activity names are in an array,
    // you can send them to a third party or use
    // them in some other way.
  });
```
