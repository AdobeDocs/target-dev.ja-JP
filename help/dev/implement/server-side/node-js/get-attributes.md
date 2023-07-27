---
title: で非同期リクエストを使用する方法 [!DNL Adobe Target] Node.js SDK
description: 方法を学ぶ [!DNL Target] Node.js SDK は非同期リクエストをサポートしているので、有効なターゲット時間をゼロに抑えることができます。
feature: APIs/SDKs
exl-id: aa06f3ca-7d2a-4334-8092-730a8705dfb0
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '116'
ht-degree: 18%

---

# 属性の取得 (Node.js)

## 説明

`[!UICONTROL getAttributes()]` は、次の場所から実験とパーソナライズされたエクスペリエンスを取得するために使用されます。 [!DNL Target] 属性値を抽出します。

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

## プロミス

The `Promise` 返送者： `TargetClient.getAttributes()` は次のメソッドを使用してオブジェクトを解決します。

| メソッド | 戻り値の型 | 説明 |
| --- | --- | --- |
| getValue(mboxName, key) | 任意 | 指定された mbox 名と属性キーの値を返します |
| asObject(mboxName) | オブジェクト | キーと値のペアを持つシンプルな json オブジェクトを返します |
| getResponse() | [getOffers 応答](https://github.com/jasonwaters/target-nodejs-sdk#targetclientgetoffers) | が返す応答オブジェクトを返します。 `getOffers` |

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
