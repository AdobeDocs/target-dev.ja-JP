---
title: Adobe Target Delivery API クライアントヒント
description: ' [!DNL Adobe Target] 配信APIでクライアントヒントを使用するにはどうすればよいですか？'
exl-id: 317b9d7d-5b98-464e-9113-08b899ee1455
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/ijbOsWitZdNHpjNduh8xtPyEYdw2tsWz2rB6jZ5JbQA
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: ff2b9b37-92e0-45fc-b853-379d44c08c89
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 282
ht-degree: 0%

---

# クライアントヒントと[!UICONTROL Adobe Target配信API]

クライアントヒントは、オファーリクエストで[!DNL Adobe Target]に送信する必要があります。

通常、使用可能なすべてのクライアントヒントを[!DNL Target]に送信することをお勧めします。 詳しくは、「[&#x200B; クライアントサイド実装](../../implement/client-side/overview.md)」セクションの「[User-agent and Client Hints](/help/dev/implement/client-side/atjs/user-agent-and-client-hints.md)」を参照してください。

## 配信API ダイレクトコール

### ブラウザーから

この場合、ブラウザーはリクエストヘッダーを介して、低エントロピーのクライアントヒントを自動的に[!DNL Target]に送信します。 しかし、この実装にはブラウザーレベルの制限がいくつかあります。 最初 – リクエストがhttps経由で行われていない限り、クライアントヒントヘッダーはブラウザーから送信されません。 2番目 – ページ上の[!DNL Target]への最初のリクエストでは、クライアントヒントは送信されません。 クライアントヒントヘッダーは、2回目のリクエストとその後のすべてのリクエストでのみ送信されます。 つまり、[!DNL Target]様が最初のページ訪問時にオーディエンスのセグメント化とパーソナライゼーションを実行することはできません。 これらの制限を回避するには、ブラウザーでUser Agent Client Hints APIを使用してクライアントヒントを直接収集し、リクエストペイロードに送信することを強くお勧めします。

### サーバーから

この場合、クライアントヒントは、Delivery API リクエストでブラウザーから[!DNL Target]に手動で転送する必要があります。

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

クライアントヒントヘッダーSec-CH-UAおよびSec-CH-UA-Full-Version-Listの形式は、クライアントヒントブラウザーAPI （navigator.userAgentData.brands/navigator.userAgentData.getHighEntropyValues）の結果とは異なります。 これらの形式は両方ともDelivery APIで受け入れられます。 Delivery APIは、値をリクエストヘッダーで使用される形式に正規化します。これは、プロファイルスクリプトでクライアントヒントにアクセスする場合に注意が必要です。
