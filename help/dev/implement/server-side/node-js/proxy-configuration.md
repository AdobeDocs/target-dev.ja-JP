---
title: ' [!DNL Adobe Target] Node.js SDK へのプロキシ設定の実装'
description: ' [!DNL Adobe Target] Node.js SDK で [!UICONTROL TargetClient] プロキシ設定を指定する方法を説明します。'
feature: APIs/SDKs
exl-id: c9f04e81-3fa3-4e64-a974-379420b0518a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '94'
ht-degree: 0%

---

# プロキシ設定（Node.js）

Node SDK の HTTP リクエストのプロキシを設定するには、初期化中に SDK が使用するフェッチ API を上書きします。

次に、`TargetClient` の初期化中に `fetchApi` を上書きしてプロキシを追加する方法を示す基本的な例を示します。

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

これは、ノードのデフォルト `fetch` であるノードバージョン 18.2 以降 `undici.fetch` のみ機能することに注意してください。
[Node SDK サンプルリポジトリ ](https://github.com/adobe/target-nodejs-sdk-samples/tree/master/proxy-configuration) を参照してください。
（古いバージョンのノードのプロキシ設定の例やその他詳細情報）。
