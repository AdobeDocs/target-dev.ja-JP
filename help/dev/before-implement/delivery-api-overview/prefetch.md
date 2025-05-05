---
title: Adobe Target配信 API プリフェッチ
description: '[!UICONTROL Adobe Target Delivery API] でのプリフェッチの使用方法'
keywords: 配信 api
exl-id: eab88e3a-442c-440b-a83d-f4512fc73e75
feature: APIs/SDKs
source-git-commit: 4ff2746b8b485fe3d845337f06b5b0c1c8d411ad
workflow-type: tm+mt
source-wordcount: '522'
ht-degree: 0%

---

# プリフェッチ

プリフェッチを使用すると、モバイルアプリやサーバーなどのクライアントは、複数の mbox またはビューのコンテンツを 1 回のリクエストで取得し、ローカルにキャッシュした後、訪問者が mbox またはビューに訪問した [!DNL Target] きに通知することができます。

プリフェッチを使用する場合、次の用語を理解しておくことが重要です。

| フィールド名 | 説明 |
| --- | --- |
| `prefetch` | 取得する必要があるが、訪問済みとしてマークしてはいけない mbox とビューのリスト。 [!DNL Target] Edgeは、プリフェッチ配列に存在する mbox またはビューごとに `eventToken` を返します。 |
| `notifications` | 以前にプリフェッチされ、訪問済みとしてマークする必要があった mbox とビューのリスト。 |
| `eventToken` | コンテンツがプリフェッチされたときに返される、ハッシュ化された暗号化トークン。 このトークンは、`notifications` 配列の [!DNL Target] に送り返される必要があります。 |

## Mbox をプリフェッチ

モバイルアプリやサーバーなどのクライアントでは、セッション内の特定の訪問者に対して複数の mbox をプリフェッチし、それらをキャッシュして、[!UICONTROL Adobe Target Delivery API] への複数の呼び出しを回避できます。

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

「`prefetch`」フィールド内に、セッション内の訪問者に `mboxes` して少なくとも 1 回プリフェッチする 1 つ以上のリクエストを追加します。 これらの `mboxes` ータをプリフェッチすると、次の応答が返されます。

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

応答内には、特定の `mbox` ーザーの訪問者に表示するエクスペリエンスを含む `content` フィールドが表示されます。 これは、サーバーにキャッシュされている場合に非常に便利です。訪問者がセッション内で web アプリケーションやモバイルアプリケーションを操作し、アプリケーションの特定のページで `mbox` を訪問すると、別の [!UICONTROL Adobe Target Delivery API] 呼び出しを行う代わりに、キャッシュからエクスペリエンスを配信できます。 ただし、エクスペリエンスが `mbox` ージから訪問者に配信されると、配信 API 呼び出しを介してエクスペリ `notification` ンスが送信され、インプレッションログが発生します。 これは、`prefetch` 呼び出しの応答がキャッシュされ、訪問者が `prefetch` 呼び出し発生時にエクスペリエンスを確認していないためです。 `notification` のプロセスについて詳しくは、[ 通知 ](notifications.md) を参照してください。

## [!UICONTROL Analytics for Target] を使用する際の `clickTrack` 指標を含む mbox のプリフェッチ（A4T）

[[!UICONTROL Adobe Analytics for Target]](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja){target=_blank} （A4T）は、コンバージョン指標とオーディエンスセグメントに基づいてアクティビティを作成でき [!DNL Analytics] クロスソリューション統合環境です。

次のコードスニペットは、オファーがクリックされたことを [!DNL Analytics] ーザーに通知する、`clickTrack` の指標を含む mbox のプリフェッチからの応答です。

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
>mbox のプリフェッチには、該当するアクティビティのみの [!DNL Analytics] ペイロードが含まれます。 まだ選定されていないアクティビティの成功指標をプリフェッチすると、レポートに不整合が生じます。

## ビューをプリフェッチ

ビューは、シングルページアプリケーション（SPA）とモバイルアプリケーションをよりシームレスにサポートします。 ビューは、SPA エクスペリエンスまたはモバイルエクスペリエンスを構成するビジュアル要素の論理的なグループと見なすことができます。 これで、配信 API を通じて、VEC で作成した [[!UICONTROL A/B Test]](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=ja){target=_blank} アクティビティと、[SPAのビュー ](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md) に変更を加えた [[!UICONTROL Experience Targeting]](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=ja){target=_blank} （X） T アクティビティをプリフェッチできるようになりました。

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

上記の呼び出し例では、SPA VEC で作成されたすべてのビュー（[!UICONTROL A/B Test] アクティビティと XT アクティビティ）をプリフェッチし、Web `channel` ージに表示します。 この呼び出しによって、`tntId`:`84e8d0e211054f18af365d65f45e902b.28_131` を持つ訪問者が `url`:`https://target.enablementadobe.com/react/demo/#/` を訪問した際に適格となる、[!UICONTROL A/B Test] または XT アクティビティのすべてのビューがプリフェッチされることに注意してください。

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

応答の `content` のフィールドに、`type`、`selector`、`cssSelector`、`content` などのメタデータをメモします。これらのメタデータは、ユーザーがページに訪問したときに訪問者にエクスペリエンスをレンダリングするために使用されます。 必要に応じて、`prefetched` コンテンツをキャッシュしてユーザーにレンダリングできます。
