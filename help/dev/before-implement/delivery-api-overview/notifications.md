---
title: Adobe Target Delivery API 通知
description: 通知を送信する際に、 [!UICONTROL Adobe Target Delivery API]?
keywords: 配信 api
exl-id: 711388fd-2c1f-4ca4-939f-c56dc4bdc04a
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '426'
ht-degree: 0%

---

#  通知 

事前に取得された mbox またはビューがエンドユーザーに訪問またはレンダリングされた場合に、通知を発行する必要があります。

適切な mbox またはビューに対して通知が送信されるようにするには、必ず、対応する `eventToken` mbox またはビューごとに 適切な `eventToken` を正しく反映するには、対応する mbox またはビューを実行する必要があります。

## プリフェッチされた mbox に関する通知

1 回の配信呼び出しで 1 つ以上の通知を送信できます。 追跡する必要がある指標が、 `click` または `display` mbox ごとに、 `type` 通知の内容が正しく反映されている可能性があります。 また、 `id` 通知が[!UICONTROL  Adobe Target Delivery API]. The `timestamp` は、次に転送することも重要です： [!DNL Target] いつ頃を示す `click` または `display` レポート目的で特定の mbox に対して発生していた問題を修正しました。

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

上記の呼び出しの例では、 `notifications` リクエストが正常に処理されました。

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

すべての `notifications` 送信先 [!DNL Target] が正しく処理されている場合は、 `notifications` 配列を返します。 ただし、 `notifications` `id` その特定の `notification` 通り抜けなかった このシナリオでは、成功するまで再試行ロジックを設定できます `notification` 応答を取得します。 再試行ロジックにタイムアウトが指定されていることを確認して、API 呼び出しがブロックされてパフォーマンスが遅延するのを防ぎます。

## プリフェッチされたビューの通知

1 回の配信呼び出しで 1 つ以上の通知を送信できます。 追跡する必要がある指標が、 `click` または `display` 通知のタイプが正しく反映されるように、mbox ごとに設定します。 また、 `id` 通知が [!UICONTROL Adobe Target Delivery API]. タイムスタンプは、に転送することも重要です。 [!DNL Target] いつ頃を示す `click` または `display` レポート目的で特定のビューに対して発生していました。

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

上記の呼び出しの例では、 `notifications` リクエストが正常に処理されました。

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

すべての `notifications` 送信先  [!DNL Target] が正しく処理されている場合は、 `notifications` 配列を返します。 ただし、 `notifications` `id` が見つからない場合、特定の通知は送信されませんでした。 このシナリオでは、成功した通知応答が取得されるまで、再試行ロジックを設定できます。 再試行ロジックにタイムアウトが指定されていることを確認して、API 呼び出しがブロックされてパフォーマンスが遅延するのを防ぎます。
