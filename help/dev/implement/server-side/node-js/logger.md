---
title: を初期化します。 [!DNL Adobe Target] リクエストをログに記録する Node.js SDK
description: リクエストを [!DNL Adobe Target] Node.js SDK.
feature: APIs/SDKs
exl-id: 5db3e301-47b3-4330-b185-c0c03f72e790
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 2%

---

# ロガー (Node.js)

## 説明

条件 [SDK の初期化](initialize-sdk.md)、 `options.logger` オブジェクトはオプションのオブジェクトです。 ただし、問題が発生した場合にデバッグを効果的におこなうために、 `logger` オブジェクトは、SDK の初期化時に指定する必要があります。

The `logger` オブジェクトには `debug()` および `error()` メソッド。 適切なロガーが指定された場合（例： ） `console`, [!DNL Target] リクエストと応答がログに記録されます。

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

コンソールに要求と応答が印刷されています。
