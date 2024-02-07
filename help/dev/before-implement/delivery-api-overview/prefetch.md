---
title: Adobe Target Delivery API Prefetch
description: でのプリフェッチの使用方法 [!UICONTROL Adobe Target Delivery API]?
keywords: 配信 api
exl-id: eab88e3a-442c-440b-a83d-f4512fc73e75
feature: APIs/SDKs
source-git-commit: 91592a86957770c4d189115fd3ebda61ed52dd38
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---

# プリフェッチ

プリフェッチを使用すると、モバイルアプリやサーバーなどのクライアントは、1 回のリクエストで複数の mbox やビューのコンテンツを取得し、ローカルにキャッシュして、後で通知できます。 [!DNL Target] ユーザーがこれらの mbox またはビューを訪問したとき。

プリフェッチを利用する場合、次の用語に精通しておくことが重要です。

| フィールド名 | 説明 |
| --- | --- |
| `prefetch` | 取得するが訪問済みとしてマークすべきでない mbox とビューのリスト。 The [!DNL Target] Edge は、 `eventToke`n は、プリフェッチ配列内に存在する mbox またはビューごとに生成されます。 |
| `notifications` | 以前にプリフェッチされ、訪問済みとしてマークされる必要がある mbox とビューのリスト。 |
| `eventToken` | コンテンツがプリフェッチされた際に返される、ハッシュ化された暗号化トークン。 このトークンは、に返送する必要があります [!DNL Target] （内） `notifications` 配列。 |

## Mbox のプリフェッチ

モバイルアプリやサーバーなどのクライアントは、1 つのセッション内で特定のユーザーに対して複数の mbox をプリフェッチし、 [!UICONTROL Adobe Target Delivery API].

```
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=7abf6304b2714215b1fd39a870f01afc#1555632114' \
-H 'Content-Type: application/json' \
-H 'cache-control: no-cache' \
-d '
{
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
    "prefetch": {
    "mboxes" : [
      {
        "name" : "SummerOffer",
        "index" : 1
      },
      {
        "name" : "SummerShoesOffer",
        "index" : 2
      },
      {
        "name" : "SummerDressOffer",
        "index" : 3
      }      
    ]
  }
}'
```

内 `prefetch` フィールドに、1 つ以上の `mboxes` セッション内のユーザーに対して、一度ににプリフェッチする必要がある。 これらをプリフェッチした後 `mboxes` 次の応答を受け取ります。

```
{
    "status": 200,
    "requestId": "5efee0d8-3779-4b12-a74e-e04848faf191",
    "client": "demo",
    "id": {
        "tntId": "abcdefghijkl00023.1_1"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "prefetch": {
        "mboxes": [
            {
                "index": 1,
                "name": "SummerOffer",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                        "type": "html",
                        "eventToken": "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
                    }
                ]
            },
            {
                "index": 2,
                "name": "SummerShoesOffer",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next shoe purchase</b></p>"
                        "type": "html",
                        "eventToken": "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
                    }
                ]
            },
            {
                "index": 3,
                "name": "SummerDressOffer",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next dress purchase</b></p>"
                        "type": "html",
                        "eventToken": "GcvBXDhdJFNR9E9r1tgjfmqipfsIHvVzTQxHolz2IpSCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
                    }
                ]
            }
        ]
    }
}
```

応答内に、 `content` 特定ののユーザーに表示するエクスペリエンスを含むフィールド `mbox`. これは、ユーザーがセッション内で Web またはモバイルアプリケーションを操作し、 `mbox` アプリケーションの特定のページでは、別のページを作成する代わりに、エクスペリエンスをキャッシュから配信できます [!UICONTROL Adobe Target Delivery API] を呼び出します。 ただし、 `mbox`, a `notification` は、インプレッションのログが記録されるように、Delivery API 呼び出しを介して送信されます。 これは、 `prefetch` の呼び出しがキャッシュされます。つまり、ユーザーは、 `prefetch` 呼び出しが発生します。 詳しくは、 `notification` プロセス、を参照してください。 [通知](notifications.md).

## 使用時に clickTrack 指標を使用して mbox をプリフェッチ [!UICONTROL Analytics for Target] (A4T)

[[!UICONTROL Adobe Analytics for Target]](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html){target=_blank} (A4T) は、 [!DNL Analytics] コンバージョン指標およびオーディエンスセグメント。

以下のコードスニペットを使用して、を含む mbox をプリフェッチできます。 `clickTrack` 通知する指標 [!DNL Analytics] オファーがクリックされたことを示します。

```
{
  "prefetch": {
    "mboxes": [
      {
        "index": 0,
        "name": "<mboxName>",
        "options": [
           ...
        ],
        "metrics": [
          {
            "type": "click",
            "eventToken": "<eventToken>",
             "analytics": {
               "payload": {
                 "pe": "tnt",
                 "tnta": "..."
               }
             }
          },
          }
        ],
        "analytics": {
          "payload": {
            "pe": "tnt",
            "tnta": "347565:1:0|2,347565:1:0|1"
          }
        }
      }
    ]
  }
}
```

>[!NOTE]
>
>mbox のプリフェッチには、 [!DNL Analytics] 認定されたアクティビティのみのペイロード。 まだ認定されていないアクティビティの成功指標をプリフェッチすると、レポートの不整合が生じます。

## プリフェッチビュー数

ビューは、シングルページアプリケーション (SPA) とモバイルアプリケーションをよりシームレスにサポートします。 ビューは、ビジュアル要素の論理的なグループと見なすことができ、まとめてSPAまたは Mobile エクスペリエンスを構成します。 これで、VEC は Delivery API を通じて AB および XT アクティビティを作成し、 [SPAの表示](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md) これで、をプリフェッチできるようになりました。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=a3e7368c62d944c0855d424cd7a03ab0' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
  },
  "context": {
    "channel": "web",
    "window": {
      "width": 1819,
      "height": 842
    },
    "browser": {
      "host": "target.enablementadobe.com"
    },
    "address": {
      "url": "https://target.enablementadobe.com/react/demo/#/"
    }
  },
  "prefetch": {
    "views": [{}]
  }
}'
```

上記の呼び出し例では、Web 用に表示する AB および XT アクティビティ用にSPA VEC で作成されたすべてのビューをプリフェッチします `channel`. 呼び出しでは、訪問者が持つ AB または XT アクティビティからすべてのビューをプリフェッチすることに注意してください。 `tntId`:`84e8d0e211054f18af365d65f45e902b.28_131` 誰が訪問しているか `url`:`https://target.enablementadobe.com/react/demo/#/` 資格を満たしている。

```
{
    "status": 200,
    "requestId": "14ce028e-d2d2-4504-b3da-32740fa8dd61",
    "client": "demo",
    "id": {
        "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "prefetch": {
        "views": [
            {
                "id": 228,
                "name": "checkout-express",
                "key": "checkout-express",
                "state": "Vqfb6kYGAmzWOLf9W6E+Q/0LyS+SYe2h5tuTXzRNnkjKkZaZZr2ijp41/6AwK6fdFgADhFNC7l5efUCs9shgTw==",
                "options": [
                    {
                        "content": [
                            {
                                "type": "setHtml",
                                "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(0) > FORM.col-md-4:eq(0) > DIV:nth-of-type(1) > DIV.mb-3:eq(2)",
                                "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(1) > FORM:nth-of-type(2) > DIV:nth-of-type(1) > DIV:nth-of-type(3)",
                                "content": "<span style=\"color:#000080;\"><strong>*We charge an additional fee of $12.34 for faster delivery. If you choose express delivery get 15% off on your next order.</strong></span>"
                            }
                        ],
                        "type": "actions",
                        "eventToken": "N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                    }
                ]
            },
            {
                "id": 5,
                "name": "home",
                "key": "home",
                "state": "Vqfb6kYGAmzWOLf9W6E+Q/0LyS+SYe2h5tuTXzRNnkjKkZaZZr2ijp41/6AwK6fdFgADhFNC7l5efUCs9shgTw==",
                "options": [
                    {
                        "content": [
                            {
                                "type": "setHtml",
                                "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(1) > DIV.heading:eq(0) > H1.title:eq(0)",
                                "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(2) > DIV:nth-of-type(1) > H1:nth-of-type(1)",
                                "content": "<span style=\"color:#800000;\"><strong>Trending Items</strong></span>"
                            }
                        ],
                        "type": "actions",
                        "eventToken": "N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                    }
                ]
            },
            {
                "id": 6,
                "name": "products",
                "key": "products",
                "state": "Vqfb6kYGAmzWOLf9W6E+Q/0LyS+SYe2h5tuTXzRNnkjKkZaZZr2ijp41/6AwK6fdFgADhFNC7l5efUCs9shgTw==",
                "options": [
                    {
                        "content": [
                            {
                                "type": "setStyle",
                                "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(0) > DIV.heading:eq(0) > BUTTON.btn:eq(0)",
                                "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(1) > BUTTON:nth-of-type(1)",
                                "content": {
                                    "background-color": "rgba(191,0,0,1)",
                                    "priority": "important"
                                }
                            }
                        ],
                        "type": "actions",
                        "eventToken": "N3C13I0M2PH8iaKtONJlFJNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="
                    }
                ]
            }
        ]
    }
}
```

Adobe Analytics の `content` 応答のフィールドには、次のようなメタデータをメモします。 `type`, `selector`, `cssSelector`、および `content`：ユーザーがページを訪問したときに、エクスペリエンスをエンドユーザーにレンダリングするために使用します。 なお、 `prefetched` コンテンツは、必要に応じてキャッシュし、ユーザーにレンダリングできます。
