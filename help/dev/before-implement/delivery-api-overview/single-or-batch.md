---
title: Adobe Target配信 API シングル配信またはバッチ配信
description: 単一またはバッチ配信 [!UICONTROL Adobe Target Delivery API] 呼び出しの使用方法
keywords: 配信 api
exl-id: 525cd1f2-616a-486c-8f49-8117615500bb
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '448'
ht-degree: 0%

---

# 単一またはバッチ配信

[!UICONTROL Adobe Target Delivery API] は、単一またはバッチの配信呼び出しをサポートします。 1 つは、1 つまたは複数の mbox のコンテンツに対してサーバーリクエストを行うことができます。

1 回の呼び出しを行うことを決定する際のパフォーマンスコストとバッチ呼び出しを行う際のパフォーマンスコストを比較します。 ユーザーに表示する必要のあるすべてのコンテンツがわかっている場合は、複数のバッチ配信呼び出しを行わないように、1 回のバッチ配信呼び出しですべての mbox のコンテンツを取得することがベストプラクティスです。

## 1 回の配信呼び出し

[!UICONTROL Adobe Target Delivery API] を介して、1 つの mbox のユーザーに表示するエクスペリエンスを取得できます。 1 回の配信呼び出しを行う場合、ユーザーの mbox の追加コンテンツを取得するには、別のサーバー呼び出しを開始する必要があります。 これは時間の経過と共に非常にコストがかかる可能性があるので、1 回の配信 API 呼び出しを使用する際は、必ずアプローチを評価してください。

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
    "execute": {
    "mboxes" : [
      {
        "name" : "SummerOffer",
        "index" : 1
      }
    ]
  }
}'
```

上記の 1 回の配信呼び出しの例では、エクスペリエンスが取得され、web チャネルの `mbox`:`SummerOffer` に対して `tntId`:`abcdefghijkl00023.1_1` を使用してユーザーに表示されます。 この単一の配信呼び出しでは、次の応答が生成されます。

```
{
  "status": 200,
  "requestId": "25e0cc42-3d7b-456a-8b49-af60c1fb23d9",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.1_1"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "execute": {
      "mboxes": [
          {
              "index": 1,
              "name": "SummerOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                      "type": "html",
                  }
              ]
          }
      ]
    }
}
```

応答で、「`content`」フィールドには、SummerOffer mbox に対応する web 用にユーザーに表示するエクスペリエンスを示すHTMLが含まれていることに注意してください。

### ページ読み込みを実行

フッターやヘッダーにあるフォントを AB テストするなど、web チャネルでページの読み込みが発生した場合に表示する必要があるエクスペリエンスがある場合は、`execute` フィールドで `pageLoad` を指定して、適用するすべての変更を取得できます。

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
  "execute": {
    "pageLoad": {}
  }
}'
```

上記の呼び出し例では、ページ `https://target.enablementadobe.com/react/demo/#/` ージの読み込み時にユーザーに表示するエクスペリエンスを取得しています。

```
{
      "status": 200,
      "requestId": "355ebc47-edb6-481f-aeae-ae55d71afaca",
      "client": "demo",
      "id": {
          "tntId": "84e8d0e211054f18af365d65f45e902b.28_131"
      },
      "edgeHost": "mboxedge28.tt.omtrdc.net",
      "execute": {
          "pageLoad": {
              "options": [
                  {
                      "content": [
                          {
                              "type": "setHtml",
                              "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(1) > NAV.nav:eq(0) > DIV.container:eq(0) > DIV.nav-right:eq(0) > A.nav-item:eq(0)",
                              "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(1) > NAV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(2) > A:nth-of-type(1)",
                              "content": "Modified Home"
                          }
                      ],
                      "type": "actions"
                  }
              ],
              "metrics": [
                  {
                      "type": "click",
                      "selector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(0) > FORM.col-md-4:eq(0) > DIV.form-group:eq(0) > BUTTON.btn:eq(0)",
                      "eventToken": "QPaLjCeI9qKCBUylkRQKBg=="
                  }
              ]
          }
      }
  }
```

「`content`」フィールドでは、ページ読み込み時に適用する必要のある変更を取得できます。 上記の例では、ヘッダー上のリンクに *Modified Home* という名前を付ける必要があることに注意してください。

## バッチ配信呼び出し

各呼び出しで 1 つの mbox で複数の配信呼び出しを実行する代わりに、バッチの mbox で 1 つの配信呼び出しを実行すると、不要なサーバー呼び出しを減らすことができます。 パフォーマンスを高めるには、サーバーコールの呼び出しをできるだけ最小限に抑える必要があります。

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
    "execute": {
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

上記のバッチ配信呼び出しの例では、複数の `mbox`:`SummerOffer`、`SummerShoesOffer` および `SummerDressOffer` に対して、`tntId`:`abcdefghijkl00023.1_1` を持つユーザーに表示するエクスペリエンスが取得されます。 このユーザーには複数の mbox のエクスペリエンスを表示する必要があるので、これらのリクエストをバッチ処理し、3 回の個別の配信呼び出しの代わりに 1 回のサーバー呼び出しを行うことができます。

```
{
  "status": 200,
  "requestId": "fe15286f-effb-434f-85d8-c3db804075ce",
  "client": "demo",
  "id": {
      "tntId": "abcdefghijkl00023.28_120"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  "execute": {
      "mboxes": [
          {
              "index": 1,
              "name": "SummerOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                      "type": "html",

                  }
              ]
          },
          {
              "index": 2,
              "name": "SummerShoesOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next shoe purchase</b></p>",
                      "type": "html",
                  }
              ]
          },
          {
              "index": 3,
              "name": "SummerDressOffer",
              "options": [
                  {
                      "content": "<p><b>Enjoy this 15% discount on your next dress purchase</b></p>",
                      "type": "html",
                  }
              ]
          }
      ]
  }
}
```

上記の応答では、各 mbox の `content` フィールド内で、各 mbox でユーザーに表示するエクスペリエンスのHTML表現を取得できることがわかります。
