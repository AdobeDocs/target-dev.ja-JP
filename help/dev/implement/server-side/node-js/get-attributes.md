---
title: ' [!DNL Adobe Target] Node.js SDKでの非同期リクエストの使用方法'
description: ' [!DNL Target] Node.js SDKが非同期リクエストをサポートする方法を説明します。これにより、効果的な目標時間をゼロに短縮できます。'
feature: APIs/SDKs
exl-id: aa06f3ca-7d2a-4334-8092-730a8705dfb0
TQID: https://experienceleague.adobe.com/cIoEnAinSLl-TO2vunG164i97Y2h-9NdE487ZyXJSzs
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: bcc5edb5-84c3-4940-9f84-ed88b6c16274
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 116
ht-degree: 18%

---

# 属性の取得（Node.js）

## 説明

`[!UICONTROL getAttributes()]`は、[!DNL Target]から実験とパーソナライズされたエクスペリエンスを取得し、属性値を抽出するために使用されます。

## メソッド

### getAttributes

```js {line-numbers="true"}
TargetClient.getAttributes(mboxNames: Array, options: Object): Promise
```

## パラメーター

| 名前 | タイプ | 必須 | デフォルト |
| --- | --- | --- |--- |
| mboxNames | 配列 | ○ | None |
| options | オブジェクト | × | None |

## 約束

`TargetClient.getAttributes()`が返した`Promise`は、次のメソッドを使用してオブジェクトを解決します。

| メソッド | 戻り値の型 | 説明 |
| --- | --- | --- |
| getValue （mboxName, key） | 任意 | 指定したmbox名と属性キーの値を返します |
| asObject （mboxName） | オブジェクト | キーと値のペアを持つシンプルなjson オブジェクトを返します |
| getResponse （） | [getOffers Response](https://github.com/jasonwaters/target-nodejs-sdk#targetclientgetoffers) | 通常`getOffers`が返す応答オブジェクトを返します |

## 例

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);
const offerAttributes = await targetClient.getAttributes(["demo-engineering-flags"]);


//returns just the value of searchProviderId from the mbox offer
const searchProviderId = offerAttributes.getValue("demo-engineering-flags", "searchProviderId");

//returns a simple JSON object representing the mbox offer
const engineeringFlags = offerAttributes.asObject("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

const assetUrl = `http://${engineeringFlags.cdnHostname}/path/to/asset`;
```
