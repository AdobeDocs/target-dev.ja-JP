---
title: Adobe Target配信 API の通知
description: '[!UICONTROL Adobe Target Delivery API] を使用して通知を送信するにはどうすればよいですか？'
keywords: 配信 api
exl-id: 711388fd-2c1f-4ca4-939f-c56dc4bdc04a
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '414'
ht-degree: 0%

---

# 通知

プリフェッチされた mbox またはビューがエンドユーザーに訪問またはレンダリングされた場合、通知を実行する必要があります。

適切な mbox またはビューに対して通知を送信するには、各 mbox またはビューに対応する `eventToken` を必ず追跡してください。 レポートを正しく反映するには、対応する mbox またはビューに対して正しい `eventToken` を持つ通知を実行する必要があります。

## プリフェッチされた mbox の通知

1 回の配信呼び出しで 1 つまたは複数の通知を送信できます。 通知の `type` を正しく反映させるために、追跡する必要がある指標が mbox ごとに `click` または `display` のいずれであるかを判断します。 また、[!UICONTROL &#x200B; Adobe Target Delivery API] 経由で通知が正しく送信されたかどうかを判断できるように、通知ごとに `id` を渡します。 また、`timestamp` は、レポート目的で特定の mbox に対して `click` または `display` がいつ発生したかを示すために、[!DNL Target] に転送することが重要です。

```
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
-H 'Content-Type: application/json' \
-H 'cache-control: no-cache' \
-d '{
    "id": {
      "tntId": "abcdefghijkl00023.1_1"
    },
    "context": {
      "channel": "web",
      "browser" : {
        "host" : "demo"
      },
      "address" : {
        "url" : "http://demo.dev.tt-demo.com/demo/store/index.html"
      },
      "screen" : {
        "width" : 1200,
        "height": 1400
      }
    },
      "notifications": [
      {
      "id" : "SummerOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
      },
    {
      "id" : "SummerShoesOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerShoesOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
      },
    {
      "id" : "SummerDressOfferNotification",
        "timestamp" : 1555705311051,
        "type" : "display",
        "mbox" : {
          "name" :"SummerDressOffer"   
        },
        "tokens" : [
          "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q"
        ]
    } 
    ]
  }'
```

上記の呼び出し例では、`notifications` リクエストが正常に処理されたことを示す応答が返されます。

```
{
  "status": 200,
  "requestId": "36014eed-4772-4c48-a9e2-e532762b6a85",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.28_20"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "notifications": [
      {
          "id": "SummerOfferNotification"
      },
      {
          "id": "SummerDressOfferNotification"
      },
      {
          "id": "SummerShoesOfferNotification"
      }
  ]
}
```

[!DNL Target] に送信されたすべての `notifications` が正しく処理されると、応答の `notifications` 配列に表示されます。 ただし、`notifications` `id` が見つからない場合、その特定の `notification` は通過しませんでした。 このシナリオでは、成功した `notification` 応答が取得されるまで、再試行ロジックを導入できます。 API 呼び出しがブロックされず、パフォーマンスの遅延が発生しないように、再試行ロジックにタイムアウトが指定されていることを確認します。

## プリフェッチされたビューの通知

1 回の配信呼び出しで 1 つまたは複数の通知を送信できます。 通知のタイプを正しく反映するために、追跡する必要がある指標が mbox ごとに `click` または `display` のいずれであるかを判断します。 また、通知ごとに `id` を渡して、[!UICONTROL Adobe Target Delivery API] 経由で通知が正しく送信されたかどうかを判断できるようにします。 また、タイムスタンプは、レポート目的で特定のビューに対して `click` または `display` がいつ発生したかを示すために、[!DNL Target] に転送する必要があります。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
  },
  "context": {
    "channel": "web",
    "browser": {
      "host": "target.enablementadobe.com"
    },
    "address": {
      "url": "https://target.enablementadobe.com/react/demo/#/"
    }
  },
  "notifications": [{
      "id": "228",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "checkout-express",
      }
    },
    {
      "id": "5",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "home",
      }
    },
    {
      "id": "6",
      "type": "display",
      "timestamp": 1556226121884,
      "tokens": ["N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="],
      "view": {
        "name": "products",
      }
    }
  ]
}'
```

上記の呼び出し例では、`notifications` リクエストが正常に処理されたことを示す応答が返されます。

```
{
    "status": 200,
    "requestId": "85cc7394-c19a-4398-9b8b-bbee1e4c4579",
    "client": "demo",
    "id": {
        "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "notifications": [
        {
            "id": "5"
        },
        {
            "id": "6"
        },
        {
            "id": "228"
        }
    ]
}
```

[!DNL Target] に送信されたすべての `notifications` が正しく処理されると、応答の `notifications` 配列に表示されます。 ただし、`notifications` `id` が見つからない場合、その特定の通知は送信されませんでした。 このシナリオでは、成功した通知応答が取得されるまで、再試行ロジックを導入できます。 API 呼び出しがブロックされず、パフォーマンスの遅延が発生しないように、再試行ロジックにタイムアウトが指定されていることを確認します。
