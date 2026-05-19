---
title: ' [!DNL Adobe Target] Node.js SDKでプロキシ設定を実装する'
description: ' [!DNL Adobe Target] Node.js SDKで[!UICONTROL TargetClient] プロキシ設定を設定する方法について説明します。'
feature: APIs/SDKs
exl-id: c9f04e81-3fa3-4e64-a974-379420b0518a
TQID: https://experienceleague.adobe.com/kaE-ZEOTteaVp5kWSHiVYCvEiHuQHSMqeWRq6r-mJaA
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 102
ht-degree: 0%

---

# プロキシ設定（Node.js）

Node SDKのHTTP リクエストのプロキシを設定するには、初期化時にSDKで使用されるフェッチ APIを上書きします。

次に、`TargetClient`の初期化中に`fetchApi`を上書きしてプロキシを追加する方法を示す基本的な例を示します。

```javascript {line-numbers="true"}
const { ProxyAgent } = require("undici");

const proxyAgent = new ProxyAgent("your proxy address here");

const fetchImpl = (url, options) => {
  const fetchOptions = options;
  fetchOptions.dispatcher = proxyAgent;
  return fetch(url, fetchOptions);
};

client = TargetClient.create({
    ...,
    fetchApi: fetchImpl
});
```

これはNode バージョン 18.2以降でのみ機能します。このバージョンでは、`undici.fetch`がノードのデフォルト `fetch`です。
[Node SDK サンプルリポジトリにアクセスしてください](https://github.com/adobe/target-nodejs-sdk-samples/tree/master/proxy-configuration)
古いバージョンのノードのプロキシ設定の例と詳細については、こちらを参照してください。
