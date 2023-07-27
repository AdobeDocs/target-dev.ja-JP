---
title: Adobe Target Delivery API の単一配信またはバッチ配信
description: 使用方法 [!UICONTROL Adobe Target Delivery API] 単一配信呼び出しとバッチ配信呼び出しのどちらですか？
keywords: 配信 api
exl-id: 525cd1f2-616a-486c-8f49-8117615500bb
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '460'
ht-degree: 0%

---

# 単一またはバッチ配信

The [!UICONTROL Adobe Target Delivery API] は、単一またはバッチ配信呼び出しをサポートします。 1 つまたは複数の mbox のコンテンツに対してサーバーリクエストを作成できます。

1 回の呼び出しとバッチ呼び出しの比較を決定する際に、パフォーマンスコストを考慮します。 ユーザーに表示する必要のあるすべてのコンテンツがわかっている場合は、1 回のバッチ配信呼び出しですべての mbox のコンテンツを取得することをお勧めします。これは、1 回の配信呼び出しを複数回おこなわないようにするためです。

## 単一の配信呼び出し

1 つの mbox に対してユーザーに表示するエクスペリエンスを、 [!UICONTROL Adobe Target Delivery API]. 配信呼び出しを 1 回だけおこなう場合は、別のサーバー呼び出しを開始して、ユーザーの mbox の追加コンテンツを取得する必要があります。 これは時間の経過と共に非常にコストがかかる可能性があるので、1 回の Delivery API 呼び出しを使用する際のアプローチを必ず評価してください。

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

上記の単一の配信呼び出しの例では、エクスペリエンスが取得され、 `tntId`: `abcdefghijkl00023.1_1` の `mbox`:`SummerOffer` （web チャネル上）。 この単一の配信呼び出しでは、次の応答が生成されます。

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

応答で、 `content` 「 」フィールドには、SummerOffer mbox に対応する web 用にユーザーに表示するエクスペリエンスを記述したHTMLが含まれます。

### ページ読み込みを実行

Web チャネルでページの読み込みが発生した場合に表示する必要があるエクスペリエンスがある場合（フッターまたはヘッダーにあるフォントを AB テストするなど）、次の項目を指定できます `pageLoad` （内） `execute` フィールドを使用して、適用するすべての変更を取得します。

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

上記の呼び出し例では、ページ上でユーザーに表示するエクスペリエンスを取得しています `https://target.enablementadobe.com/react/demo/#/` 荷重。

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

Adobe Analytics の `content` フィールドを使用する場合、ページ読み込み時に適用する必要がある変更を取得できます。 上記の例では、ヘッダー上のリンクにと名前を付ける必要があります *変更されたホーム*.

## バッチ配信呼び出し

各呼び出しで 1 つの mbox で複数の配信呼び出しをおこなう代わりに、mbox のバッチで 1 回の配信呼び出しをおこなうことで、不要なサーバー呼び出しを減らすことができます。 高いパフォーマンスを得るには、サーバー呼び出しの呼び出しをできる限り最小限に抑える必要があります。

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

上記のバッチ配信呼び出しの例では、エクスペリエンスが取得され、を持つユーザーに対して表示されます。 `tntId`: `abcdefghijkl00023.1_1` 複数の `mbox`:`SummerOffer`, `SummerShoesOffer`、および `SummerDressOffer`. このユーザーに対して複数の mbox のエクスペリエンスを表示する必要があることがわかっているので、これらのリクエストをバッチ処理して、3 回の個々の配信呼び出しの代わりに 1 回のサーバー呼び出しを実行できます。

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

上記の応答では、これが `content` 各 mbox のフィールドに対して、mbox ごとにHTMLに表示するエクスペリエンスのユーザー表現を取得できます。
