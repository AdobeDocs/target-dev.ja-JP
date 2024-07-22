---
title: リクエストをログに記録するように  [!DNL Adobe Target] Node.js SDK を初期化します
description: Node [!DNL Adobe Target] js SDK でリクエストをログに記録する方法を説明します。
feature: APIs/SDKs
exl-id: 5db3e301-47b3-4330-b185-c0c03f72e790
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '85'
ht-degree: 2%

---

# ロガー（Node.js）

## 説明

[SDK を初期化 ](initialize-sdk.md) する場合、`options.logger` オブジェクトはオプションのオブジェクトです。 ただし、問題が発生した場合に効果的にデバッグするには、SDK の初期化時に `logger` オブジェクトを指定する必要があります。

`logger` オブジェクトには `debug()` および `error()` メソッドが必要です。 `console` などの適切なロガーを指定すると、[!DNL Target] のリクエストと応答がログに記録されます。

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

リクエストと応答がコンソールに印刷されているのが確認できます。
