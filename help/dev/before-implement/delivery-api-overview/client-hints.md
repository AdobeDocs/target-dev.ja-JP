---
title: Adobe Target配信 API クライアントヒント
description: Client Hints を in [!DNL Adobe Target] Delivery API で使用する方法
exl-id: 317b9d7d-5b98-464e-9113-08b899ee1455
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '268'
ht-degree: 0%

---

# Client Hints と [!UICONTROL Adobe Target Delivery API]

Client Hints は、オファーリクエストで [!DNL Adobe Target] に送信する必要があります。

通常は、使用可能なすべての Client Hints を [!DNL Target] に送信することをお勧めします。 詳しくは、[ クライアントサイド実装 ](/help/dev/implement/client-side/atjs/user-agent-and-client-hints.md) の節の [User-agent and Client Hints](../../implement/client-side/overview.md) を参照してください。

## 配信 API の直接呼び出し

### ブラウザーから

この場合、ブラウザーは低エントロピーの Client Hints を送信し、リクエストヘッダーを介して自動的に [!DNL Target] します。 ただし、この実装にはブラウザーレベルのいくつかの制限があります。 最初 – リクエストが https で行われない限り、Client Hints ヘッダーはブラウザーから送信されません。 2 番目 – ページに [!DNL Target] まれる最初のリクエストでは Client Hints は送信されません。 Client Hints ヘッダーは、2 番目のリクエスト以降のすべてのリクエストでのみ送信されます。 つまり、オーディエンスのセグメント化とパーソナライゼーションは、[!DNL Target] ーザーが最初のページ訪問では実行できません。 これらの両方の制限を回避するには、ブラウザーで User Agent Client Hints API を使用して、Client Hints を直接収集し、リクエストペイロードで送信することを強くお勧めします。

### サーバーから

この場合、Client Hints は、配信 API リクエストに応じて、ブラウザーから [!DNL Target] に手動で転送する必要があります。

```
curl -X POST 'http://mboxedge28.tt.omtrdc.net/rest/v1/delivery?client=myClientCode&sessionId=abcdefghijkl00014' -d '{
  "context": {
    "userAgent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Safari/537.36",
    "clientHints": {
      "Sec-CH-UA-Model": "iPhone",
      "Sec-CH-UA-Mobile": true,
      "Sec-CH-UA-Platform": "iOS",
      "Sec-CH-UA": "[ { \"brand\": \"Chromium\", \"version\": \"91\" }, { \"brand\": \" Not;A Brand\", \"version\": \"99\" } ]",
      "Sec-CH-UA-Full-Version-List": "[ { \"brand\": \"Chromium\", \"version\": \"91.1.1.1\" }, { \"brand\": \" Not;A Brand\", \"version\": \"99.1.1.1\" } ]",
      "Sec-CH-UA-Platform-Version": "10.0.0",
      "Sec-CH-UA-Arch": "x86",
      "Sec-CH-UA-Bitness": "64"
    }
  },
  "execute": {
    "mboxes": [{
      "name": "home",
      "index": 1
    }]
  }
}'
```

## 形式

クライアントヒントヘッダー Sec-CH-UA および Sec-CH-UA-Full-Version-List の形式は、クライアントヒントブラウザー API （navigator.userAgentData.brands/navigator.userAgentData.getHighEntropyValues）の結果とは異なります。 これらの形式は両方とも、配信 API で受け入れられます。 配信 API は、値をリクエストヘッダーで使用される形式に正規化します。これは、プロファイルスクリプトで Client Hints にアクセスする場合に注意する必要があります。
