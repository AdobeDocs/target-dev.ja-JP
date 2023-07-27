---
title: でのプロキシ設定の実装 [!DNL Adobe Target] Node.js SDK
description: 設定方法を学ぶ [!UICONTROL TargetClient] プロキシの設定 [!DNL Adobe Target] Node.js SDK.
feature: APIs/SDKs
exl-id: c9f04e81-3fa3-4e64-a974-379420b0518a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '100'
ht-degree: 0%

---

# プロキシ設定 (Node.js)

ノード SDK の HTTP リクエストのプロキシを設定するには、初期化中に SDK で使用される Fetch API を上書きします。

次に、上書きの方法を示す基本的な例を示します。 `fetchApi` 期間中 `TargetClient` プロキシを追加する初期化：

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

これは、18.2 以降のノードバージョンでのみ機能します。 `undici.fetch` はデフォルトです `fetch` （ノード用）。
次を参照してください： [ノード SDK のサンプルリポジトリ](https://github.com/adobe/target-nodejs-sdk-samples/tree/master/proxy-configuration)
プロキシの設定例を参照してください。
