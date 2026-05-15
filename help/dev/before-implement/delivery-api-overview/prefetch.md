---
title: Adobe Target Delivery APIのプリフェッチ
description: '[!UICONTROL Adobe Target Delivery API]でプリフェッチを使用するにはどうすればよいですか？'
keywords: Delivery API
exl-id: eab88e3a-442c-440b-a83d-f4512fc73e75
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/gthn2vJrIjEkmQdpsf4J818OrzFiLpeRvXXRAUp2SiY
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c18d9e03-ac7d-4811-9c92-3e92ddc70ade
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 548
ht-degree: 0%

---

# 先行取得

プリフェッチを使用すると、モバイルアプリやサーバーなどのクライアントは、1回のリクエストで複数のmboxまたはビューのコンテンツを取得し、ローカルにキャッシュして、訪問者がこれらのmboxまたはビューにアクセスしたときに後で[!DNL Target]に通知できます。

プリフェッチを使用する場合は、次の用語に精通しておくことが重要です。

| フィールド名 | 説明 |
| --- | --- |
| `prefetch` | 取得する必要がありますが、訪問済みとしてマークされるべきではないmboxとビューのリスト。 [!DNL Target] Edgeは、プリフェッチ配列に存在する各mboxまたはビューに対して`eventToken`を返します。 |
| `notifications` | 以前にプリフェッチされ、訪問済みとしてマークする必要があるmboxとビューのリスト。 |
| `eventToken` | コンテンツがプリフェッチされたときに返される、ハッシュ化された暗号化トークン。 このトークンは、`notifications`配列の[!DNL Target]に送り返す必要があります。 |

## Mboxの先行取得

モバイルアプリやサーバーなどのクライアントは、セッション内の特定の訪問者に対して複数のmboxをプリフェッチし、それらをキャッシュして[!UICONTROL Adobe Target Delivery API]への複数の呼び出しを回避できます。

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

`prefetch` フィールド内で、セッション内の訪問者に対して少なくとも1回はプリフェッチする1つ以上の`mboxes`を追加します。 これらの`mboxes`を先行取得すると、次の応答が返されます。

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

応答内に、特定の`mbox`の訪問者に表示するエクスペリエンスを含む`content` フィールドが表示されます。 これは、サーバーにキャッシュした場合に非常に便利です。訪問者がセッション内でwebまたはモバイルアプリケーションと対話し、アプリケーションの特定のページで`mbox`にアクセスすると、別の[!UICONTROL Adobe Target Delivery API]呼び出しを行う代わりにキャッシュからエクスペリエンスを配信できます。 ただし、エクスペリエンスが`mbox`から訪問者に配信されると、インプレッションのログ記録が発生するように、配信API呼び出しを介して`notification`が送信されます。 これは、`prefetch`呼び出しの応答がキャッシュされているためです。つまり、`prefetch`呼び出しの発生時に、訪問者はエクスペリエンスを見ていません。 `notification` プロセスについて詳しくは、[通知](notifications.md)を参照してください。

## [!UICONTROL Analytics for Target] （A4T）を使用する場合、`clickTrack`指標を持つmboxを先行取得します

[[!UICONTROL Adobe Analytics for Target]](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html){target=_blank} （A4T）は、[!DNL Analytics]個のコンバージョン指標とオーディエンスセグメントに基づいてアクティビティを作成できるソリューション間の統合です。

次のコードスニペットは、`clickTrack`指標を含むmboxの先行取得からの応答で、[!DNL Analytics]にオファーがクリックされたことを通知します。

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
>mboxのプリフェッチには、適格なアクティビティに対してのみ[!DNL Analytics] ペイロードが含まれます。 まだ選定されていないアクティビティの成功指標を先行取得すると、レポートの不整合が発生します。

## 先行取得

ビューは、シングルページアプリケーション（SPA）とモバイルアプリケーションをよりシームレスにサポートします。 ビューは、SPAまたはモバイルエクスペリエンスを構成するビジュアル要素の論理的なグループとして見ることができます。 これで、配信APIを通じて、VECが作成した[[!UICONTROL A/B Test]](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html){target=_blank}および[[!UICONTROL Experience Targeting]](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html){target=_blank} （X） T アクティビティと、SPA[&#128279;](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)の ビューの変更をプリフェッチできるようになりました。

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

上記の呼び出しの例では、[!UICONTROL A/B Test]用のSPA VECを通じて作成されたすべてのビューと、Web `channel`用に表示するXT アクティビティをプリフェッチします。 呼び出しは、`url`:`https://target.enablementadobe.com/react/demo/#/`を訪問している`tntId`:`84e8d0e211054f18af365d65f45e902b.28_131`の訪問者が対象とする[!UICONTROL A/B Test]またはXT アクティビティのすべてのビューをプリフェッチすることに注意してください。

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

応答の`content` フィールドに、`type`、`selector`、`cssSelector`、`content`などのメタデータを書き留めます。これらのメタデータは、ユーザーがページにアクセスしたときに訪問者にエクスペリエンスをレンダリングするために使用されます。 `prefetched` コンテンツは、必要に応じてキャッシュしてユーザーにレンダリングできます。
