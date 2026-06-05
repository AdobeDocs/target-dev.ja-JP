---
title: Adobe Target Delivery APIの通知
description: '[!UICONTROL Adobe Target Delivery API]を使用して通知を送信するにはどうすればよいですか？'
keywords: Delivery API
exl-id: 711388fd-2c1f-4ca4-939f-c56dc4bdc04a
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/rooWLG-bh7lu7eBELTQys3KoNtS-6ZicxfHoQcU6TU0
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 426
ht-degree: 0%

---

# 通知

プリフェッチされたmboxまたはビューが訪問されたり、エンドユーザーにレンダリングされたりした場合は、通知を実行する必要があります。

適切なmboxまたはビューに対して通知を送信するには、mboxまたはビューごとに対応する`eventToken`を必ず追跡してください。 レポートを正しく反映するには、対応するmboxまたはビューに対して正しい`eventToken`の通知を実行する必要があります。

## プリフェッチ済みMboxの通知

1回の配信呼び出しを使用して、1つまたは複数の通知を送信できます。 追跡する必要のある指標が、通知の`type`を正しく反映できるように、各mboxの`click`または`display`のどちらかであるかを判断します。 また、[!UICONTROL &#x200B; Adobe Target Delivery API]を通じて通知が正しく送信されたかどうかを判断できるように、各通知に`id`を渡します。 `timestamp`は、レポート用に特定のmboxで`click`または`display`がいつ発生したかを示すために[!DNL Target]に転送することも重要です。

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

上記の呼び出しの例では、`notifications` リクエストが正常に処理されたことを示す応答が返されます。

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

[!DNL Target]に送信されたすべての`notifications`が正しく処理されると、応答の`notifications`配列に表示されます。 ただし、`notifications` `id`が見つからない場合、その特定の`notification`は処理されませんでした。 このシナリオでは、正常な`notification`応答が取得されるまで、再試行ロジックを配置できます。 API呼び出しがブロックされず、パフォーマンス遅延が発生しないように、再試行ロジックにタイムアウトが指定されていることを確認します。

## プリフェッチビューの通知

1回の配信呼び出しを使用して、1つまたは複数の通知を送信できます。 追跡する必要のある指標が、通知のタイプを正しく反映できるように、各mboxの`click`または`display`のどちらかであるかを判断します。 また、[!UICONTROL Adobe Target Delivery API]を通じて通知が正しく送信されたかどうかを判断できるように、各通知に`id`を渡します。 タイムスタンプは、レポート用に特定のビューで`click`または`display`がいつ発生したかを示すために[!DNL Target]に転送することも重要です。

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

上記の呼び出しの例では、`notifications` リクエストが正常に処理されたことを示す応答が返されます。

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

[!DNL Target]に送信されたすべての`notifications`が正しく処理されると、応答の`notifications`配列に表示されます。 ただし、`notifications` `id`が見つからない場合、その特定の通知は実行されませんでした。 このシナリオでは、正常な通知の応答が取得されるまで、再試行ロジックを配置できます。 API呼び出しがブロックされず、パフォーマンス遅延が発生しないように、再試行ロジックにタイムアウトが指定されていることを確認します。
