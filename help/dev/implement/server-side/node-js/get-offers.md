---
title: Node.js SDKの使用時に [!DNL Adobe Target] で[!UICONTROL getOffers()]を使用
description: '[!UICONTROL getOffers()]を使用して決定を実行し、 [!DNL Adobe Target]からエクスペリエンスを取得する方法を説明します。'
feature: APIs/SDKs
exl-id: 3c4125ea-68d4-405e-9b9a-5fa832743153
TQID: https://experienceleague.adobe.com/WRGy74F1kUobRl1Pakse0VnXt3cT3-ntCljm4bHtiZ4
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 326
ht-degree: 21%

---

# [!UICONTROL Get Offers] （Node.js）

## 説明

`[!UICONTROL getOffers()]`は、決定を実行し、[!DNL Adobe Target]からエクスペリエンスを取得するために使用されます。


## メソッド

### getOffers

```js {line-numbers="true"}
TargetClient.getOffers(options: Object): Promise
```

## パラメーター

`options` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- |--- | --- | --- | --- |
| リクエスト | オブジェクト | ○ | None | [[!DNL Target] 配信API](/help/dev/implement/delivery-api/overview.md)要求に準拠しています |
| visitorCookie | 文字列 | × | None | ECID （VisitorId） cookie |
| targetCookie | 文字列 | × | None | [!DNL Target] Cookie |
| targetLocationHint | 文字列 | × | None | [!DNL Target]場所のヒント |
| consumerId | 文字列 | × | None | [!UICONTROL Analytics for Target] （A4T）ステッチ用のconsumerId |
| CustomerId | 配列 | × | None | VisitorIdと互換性のある形式の顧客ID |
| sessionId | 文字列 | × | None | 複数の[!DNL Target]要求のリンクに使用 |
| 訪問者 | オブジェクト | × | 新しい訪問者ID | 外部VisitorId インスタンスの指定 |

## 約束

返された`Promise`の構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| request | オブジェクト | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエスト |
| 応答 | オブジェクト | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md)件の応答 |
| visitorState | オブジェクト | 訪問者API `getInstance()`に渡すオブジェクト |
| targetCookie | オブジェクト | [!DNL Target] Cookie |
| targetLocationHintCookie | オブジェクト | [!DNL Target]の場所ヒント Cookie |
| analyticsDetails | 配列 | Analytics ペイロード（クライアントサイドのAnalytics使用の場合） |
| responseTokens | 配列 | [応答トークン &#x200B;](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=ja&)のリスト。 |
| trace | 配列 | すべてのリクエスト mbox/ビューのトレース データを集約しました |
| status | オブジェクト | 応答のステータスを含むオブジェクト。 |
| decisioningMethod | 文字列 | 使用する決定方法を決定します（[&#x200B; オンデバイス &#x200B;](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、サーバーサイド、ハイブリッド） |

ブラウザーにデータを渡すために使用される`targetCookie`および`targetLocationHintCookie` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| name | 文字列 | cookie 名 |
| value | 任意 | Cookie値の場合、は文字列に変換されます |
| maxAge | 数値 | `maxAge` オプションは、現在の時間に対して秒単位で有効期限を設定するのに便利です |

ターゲット応答のステータスを示すために使用される`status` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| status | 数値 | HTTP ステータスコード |
| message | 文字列 | 応答に関するメッセージ。 例えば、応答が[&#x200B; オンデバイス &#x200B;](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)またはサーバーサイドのどちらに決定されたかを示す場合があります |
| remoteMboxes | 配列 | 決定方法が`on-device`の場合、オンデバイスで完全に決定できなかったmbox名の配列が与えられます。 つまり、[[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエストが必要です。 |

## 例

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

const request = {
    context: {channel: "web"},
    execute: {
        mboxes: [{
            name: "a1-serverside-ab",
            index: 1
        }]
}};

const response = await targetClient.getOffers({ request });
```
