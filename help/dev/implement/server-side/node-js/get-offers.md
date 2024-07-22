---
title: Node.js SDK を使用する場合は、[!UICONTROL getOffers()] in [!DNL Adobe Target]  を使用します
description: '[!UICONTROL getOffers()] を使用して決定を実行し、 [!DNL Adobe Target] からエクスペリエンスを取得する方法を説明します。'
feature: APIs/SDKs
exl-id: 3c4125ea-68d4-405e-9b9a-5fa832743153
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 22%

---

# [!UICONTROL Get Offers] （Node.js）

## 説明

`[!UICONTROL getOffers()]` を使用して、決定を実行し、[!DNL Adobe Target] からエクスペリエンスを取得します。


## メソッド

### getOffers

```js {line-numbers="true"}
TargetClient.getOffers(options: Object): Promise
```

## パラメーター

`options` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- |--- | --- | --- | --- |
| リクエスト | オブジェクト | ○ | None | [[!DNL Target]  配信 API](/help/dev/implement/delivery-api/overview.md) リクエストに準拠 |
| visitorCookie | 文字列 | × | None | ECID （VisitorId） cookie |
| targetCookie | 文字列 | × | None | [!DNL Target] cookie |
| targetLocationHint | 文字列 | × | None | [!DNL Target] location hint |
| consumerId | 文字列 | × | None | [!UICONTROL Analytics for Target] （A4T）ステッチの consumerIds |
| CustomerIds | 配列 | × | None | VisitorId 互換フォーマットの顧客 ID |
| sessionId | 文字列 | × | None | 複数の [!DNL Target] リクエストのリンクに使用 |
| 訪問者 | オブジェクト | × | new VisitorId | 外部 VisitorId インスタンスの指定 |

## 約束

返される `Promise` は次の構造になっています。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| request | オブジェクト | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエスト |
| response | オブジェクト | [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) response |
| visitorState | オブジェクト | Visitor API `getInstance()` に渡す必要があるオブジェクト |
| targetCookie | オブジェクト | [!DNL Target] cookie |
| targetLocationHintCookie | オブジェクト | [!DNL Target] location hint cookie |
| analyticsDetails | 配列 | Analytics ペイロード（クライアントサイド Analytics を使用する場合） |
| responseTokens | 配列 | [ 応答トークン ](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?) のリスト。 |
| trace | 配列 | すべてのリクエスト mbox/ビューの集計トレースデータ |
| status | オブジェクト | 応答のステータスを含むオブジェクト。 |
| decisioningMethod | 文字列 | 使用する判定方法を決定します（[ オンデバイス ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、サーバーサイド、ハイブリッド） |

データをブラウザーに戻すために使用される `targetCookie` および `targetLocationHintCookie` オブジェクトは、次の構造を持っています。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| name | 文字列 | cookie 名 |
| value | 任意 | Cookie の値。は文字列に変換されます |
| maxAge | 数値 | `maxAge` オプションは、現在の時刻（秒）を基準に有効期限を設定する場合に便利です |

ターゲット応答のステータスを示すために使用される `status` オブジェクトは、次の構造を持ちます。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| status | 数値 | HTTP ステータスコード |
| message | 文字列 | 応答に関するメッセージ。 例えば、応答が決定されたかどうか [ オンデバイス ](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md) またはサーバーサイドか）を示します |
| remoteMbox | 配列 | Decisioning メソッドが `on-device` の場合、オンデバイスで完全に決定できなかった mbox 名の配列が提供されます。 つまり、[[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) リクエストが必要です。 |

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
