---
title: Adobe Target Delivery APIのシングルまたはバッチ配信
description: '[!UICONTROL Adobe Target Delivery API]のシングルまたはバッチ配信呼び出しを使用するにはどうすればよいですか？'
keywords: Delivery API
exl-id: 525cd1f2-616a-486c-8f49-8117615500bb
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/NMNCubmUyiVOWfq2MnkONSrQCZRqNEh0VJTfFBGptOk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 460
ht-degree: 0%

---

# シングルまたはバッチ配信

[!UICONTROL Adobe Target Delivery API]は、1回またはバッチ方式の配信呼び出しをサポートしています。 単一または複数のmboxのコンテンツに対して、サーバーリクエストを行うことができます。

単一呼出しと一括呼出しの比較を決定する際のパフォーマンスコストを測定する。 ユーザーに表示する必要があるすべてのコンテンツを把握している場合は、複数の配信呼び出しを行わないようにするために、1回のバッチ配信呼び出しですべてのmboxのコンテンツを取得することをお勧めします。

## 単一配信コール

[!UICONTROL Adobe Target Delivery API]を介して、1つのmboxのユーザーに表示するエクスペリエンスを取得できます。 1回の配信呼び出しを行う場合は、ユーザーのmboxの追加コンテンツを取得するために、別のサーバー呼び出しを開始する必要があります。 これは、時間の経過とともに非常にコストが高くなる可能性があるため、単一の配信API呼び出しを使用する場合は、必ずアプローチを評価してください。

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

上記の単一の配信呼び出しの例では、エクスペリエンスが取得され、web チャネル上の`mbox`:`SummerOffer`の`tntId`: `abcdefghijkl00023.1_1`を持つユーザーに表示されます。 この単一の配信呼び出しでは、次の応答が生成されます。

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

応答では、`content` フィールドには、SummerOffer mboxに対応するwebのユーザーに表示されるエクスペリエンスを示すHTMLが含まれていることに注意してください。

### ページ読み込みを実行

フッターまたはヘッダーにあるフォントのAB テストなど、web チャネルでページの読み込みが発生したときに表示されるエクスペリエンスがある場合は、`execute` フィールドに`pageLoad`を指定して、適用する必要があるすべての変更を取得できます。

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

上記のサンプル呼び出しは、ページ `https://target.enablementadobe.com/react/demo/#/`が読み込まれたときにユーザーを表示するエクスペリエンスを取得します。

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

`content` フィールドでは、ページ読み込み時に適用する必要がある変更を取得できます。 上記の例では、ヘッダーのリンクには&#x200B;*変更ホーム*&#x200B;という名前を付ける必要があることに注意してください。

## バッチ配信呼び出し

各呼び出しで1つのmboxを使用して複数の配信呼び出しを行う代わりに、mboxのバッチを使用して1つの配信呼び出しを行うと、不要なサーバー呼び出しを減らすことができます。 パフォーマンスを高めるには、サーバーコールの呼び出しを可能な限り最小限に抑える必要があります。

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

上記の一括配信呼び出しの例では、複数の`mbox`:`SummerOffer`、`SummerShoesOffer`、および`SummerDressOffer`に対して`tntId`: `abcdefghijkl00023.1_1`のユーザーに対して表示するエクスペリエンスが取得されます。 このユーザーに対して複数のmboxのエクスペリエンスを表示する必要があることがわかっているので、これらのリクエストをバッチ処理し、3つの個別の配信呼び出しではなく1つのサーバーコールを行うことができます。

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

上記の回答では、各mboxの`content` フィールド内で、各mboxのユーザーに表示するエクスペリエンスのHTML表現が取得可能であることがわかります。
