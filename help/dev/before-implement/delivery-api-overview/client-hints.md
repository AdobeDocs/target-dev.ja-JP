---
title: Adobe Target Delivery API クライアントのヒント
description: クライアントヒントを [!DNL Adobe Target] 配信 API?
exl-id: 317b9d7d-5b98-464e-9113-08b899ee1455
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '272'
ht-degree: 0%

---

# クライアントヒントおよび [!UICONTROL Adobe Target Delivery API]

クライアントヒントは次の宛先に送信する必要があります： [!DNL Adobe Target] を設定します。

一般に、使用可能なすべてのクライアントヒントを [!DNL Target]. 詳しくは、 [ユーザーエージェントとクライアントヒント](/help/dev/implement/client-side/atjs/user-agent-and-client-hints.md) （内） [クライアント側実装](../../implement/client-side/overview.md) 」セクションに入力します。

## 配信 API の直接呼び出し

### ブラウザーから

この場合、ブラウザは低エントロピーのクライアントヒントをに送信します。 [!DNL Target] は、リクエストヘッダーを使用して自動的に送信されます。 ただし、この実装には、ブラウザーレベルの制限がいくつかあります。 1 つ目 — リクエストが https 経由でおこなわれない限り、クライアントヒントヘッダーはブラウザーから送信されません。 2 つ目 — クライアントヒントは、 [!DNL Target] 」という名前に変更されます。 クライアントヒントヘッダーは、2 番目のリクエストとそれ以降のすべてのリクエストでのみ送信されます。 つまり、によるオーディエンスのセグメント化とパーソナライゼーションは実行できません [!DNL Target] 最初のページ訪問時。 これらの制限を両方とも回避するには、ブラウザーで User Agent Client Hints API を使用してクライアントヒントを直接収集し、リクエストペイロードで送信することを強くお勧めします。

### サーバーから

この場合、クライアントヒントは、ブラウザーからに手動で転送する必要があります。 [!DNL Target] を Delivery API リクエストに設定します。

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

クライアントヒントヘッダー Sec-CH-UA および Sec-CH-UA-Full-Version-List は、クライアントヒントブラウザー API(navigator.userAgentData.brands/navigator.userAgentData.getHighEntropyValues) の結果とは異なる形式を持ちます。 これらの形式はどちらも、Delivery API で受け入れられます。 Delivery API は、値をリクエストヘッダーで使用される形式に正規化します。これは、プロファイルスクリプトでクライアントヒントにアクセスする場合に注意する必要があります。
