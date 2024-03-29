---
title: Adobe Target Delivery API Prefetch
description: でのプリフェッチの使用方法 [!UICONTROL Adobe Target Delivery API]?
keywords: 配信 api
exl-id: eab88e3a-442c-440b-a83d-f4512fc73e75
feature: APIs/SDKs
source-git-commit: 4ff2746b8b485fe3d845337f06b5b0c1c8d411ad
workflow-type: tm+mt
source-wordcount: '549'
ht-degree: 0%

---

# プリフェッチ

プリフェッチを使用すると、モバイルアプリやサーバーなどのクライアントは、1 回のリクエストで複数の mbox やビューのコンテンツを取得し、ローカルにキャッシュして、後で通知できます。 [!DNL Target] 訪問者がこれらの mbox またはビューを訪問したとき。

プリフェッチを使用する場合、次の用語に精通しておくことが重要です。

| フィールド名 | 説明 |
| --- | --- |
| `prefetch` | 取得するが訪問済みとしてマークすべきでない mbox とビューのリスト。 The [!DNL Target] Edge は、 `eventToken` プリフェッチ配列内に存在する mbox またはビューごとに。 |
| `notifications` | 以前にプリフェッチされ、訪問済みとしてマークされる必要がある mbox とビューのリスト。 |
| `eventToken` | コンテンツがプリフェッチされた際に返される、ハッシュ化された暗号化トークン。 このトークンは、に返送する必要があります [!DNL Target] （内） `notifications` 配列。 |

## Mbox のプリフェッチ

モバイルアプリやサーバーなどのクライアントは、1 回のセッション内で特定の訪問者に対して複数の mbox をプリフェッチし、それらをキャッシュすることで、 [!UICONTROL Adobe Target Delivery API].

```shell shell-session
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

内 `prefetch` フィールドに、1 つ以上の `mboxes` 1 つのセッション内の訪問者に対して少なくとも 1 回プリフェッチする必要がある場合。 プリフェッチ後 `mboxes`に設定すると、次の応答を受け取ります。

```JSON {line-numbers="true"}
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

応答内に、 `content` 特定の `mbox`. これは、訪問者がセッション内で Web またはモバイルアプリケーションを操作し、 `mbox` アプリケーションの特定のページでは、別のページを作成する代わりに、エクスペリエンスをキャッシュから配信できます [!UICONTROL Adobe Target Delivery API] を呼び出します。 ただし、 `mbox`, a `notification` は、インプレッションログを記録するために、Delivery API 呼び出しを介して送信されます。 これは、 `prefetch` の呼び出しがキャッシュされます。つまり、訪問者は、 `prefetch` 呼び出しが発生します。 詳しくは、 `notification` プロセス、詳しくは、 [通知](notifications.md).

## での mbox のプリフェッチ `clickTrack` 指標を使用する場合 [!UICONTROL Analytics for Target] (A4T)

[[!UICONTROL Adobe Analytics for Target]](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html){target=_blank} (A4T) は、 [!DNL Analytics] コンバージョン指標およびオーディエンスセグメント。

次のコードスニペットは、を含む mbox のプリフェッチからの応答です。 `clickTrack` 通知する指標 [!DNL Analytics] オファーがクリックされたことを示します。

```JSON {line-numbers="true"}
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

ビューは、シングルページアプリケーション (SPA) とモバイルアプリケーションをよりシームレスにサポートします。 ビューは、ビジュアル要素の論理的なグループと見なすことができ、まとめてSPAまたは Mobile エクスペリエンスを構成します。 今回は、VEC が作成した Delivery API を通じて、 [[!UICONTROL A/B テスト]](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html){target=_blank} and [[!UICONTROL Experience Targeting]](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html){target=_blank} (X) 変更を加えた T 活動 [SPAの表示](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md) これで、をプリフェッチできるようになりました。

```shell  {line-numbers="true"}
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

上記の呼び出しの例では、SPA VEC で作成されたすべてのビューを事前取得し、 [!UICONTROL A/B テスト] および Web 用に表示する XT アクティビティ `channel`. この呼び出しは、 [!UICONTROL A/B テスト] または訪問者が `tntId`:`84e8d0e211054f18af365d65f45e902b.28_131` 誰が訪問しているか `url`:`https://target.enablementadobe.com/react/demo/#/` 資格を満たしている。

```JSON  {line-numbers="true"}
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

Adobe Analytics の `content` 応答のフィールドには、次のようなメタデータをメモします。 `type`, `selector`, `cssSelector`、および `content`：ユーザーがページを訪問したときに、エクスペリエンスを訪問者にレンダリングするために使用します。 なお、 `prefetched` コンテンツは、必要に応じてキャッシュし、ユーザーにレンダリングできます。
