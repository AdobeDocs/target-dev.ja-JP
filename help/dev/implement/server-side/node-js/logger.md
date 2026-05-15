---
title: ' [!DNL Adobe Target] Node.js SDKを初期化してリクエストをログに記録'
description: ' [!DNL Adobe Target] Node.js SDKでリクエストをログに記録する方法について説明します。'
feature: APIs/SDKs
exl-id: 5db3e301-47b3-4330-b185-c0c03f72e790
TQID: https://experienceleague.adobe.com/tC6xT-eAHOO17h1BK-PwWTBmwg3Dy0Wj8KYrV3W-VR4
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 83
ht-degree: 2%

---

# ロガー（Node.js）

## 説明

SDK[&#128279;](initialize-sdk.md)を初期化する場合、`options.logger` オブジェクトはオプションのオブジェクトです。 ただし、問題が発生したときに効果的にデバッグするには、SDKの初期化時に`logger` オブジェクトを指定する必要があります。

`logger` オブジェクトには`debug()`と`error()` メソッドが必要です。 `console`などの適切なロガーを指定すると、[!DNL Target]要求と応答がログに記録されます。

## 例

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg",
  logger: console
};

const targetClient = TargetClient.create(CONFIG);

const request = {
    execute: {
        mboxes: [{
            name: "a1-serverside-ab",
            index: 1
        }]
    }
};

const response = await targetClient.getOffers({ request, targetCookie });
```

リクエストと応答がコンソールに出力されます。
