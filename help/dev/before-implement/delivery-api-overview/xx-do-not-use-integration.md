---
title: Experience Cloudとの統合
description: Experience Cloudとの統合
keywords: 配信 api
source-git-commit: f16903556954d2b1854acd429f60fbf6fc2920de
workflow-type: tm+mt
source-wordcount: '478'
ht-degree: 8%

---

<!-- The content on this page was originally pulled from the legacy Delivery API doc site, and was subsequently found to be an outdated version of information now found on [Implement > Server-side > Integration > A4T Reporting](../../implement/server-side/sdk-guides/integration-with-experience-cloud/a4t-reporting.md) and [Implement > Server-side > Integration > AAM Segments](../../implement/server-side/sdk-guides/integration-with-experience-cloud/aam-segments.md). Therefore sunsetting this page, but not deleting it from the repo immediately, pending a more thorough examination to avoid inadvertently deleting relevant content. -->


# Experience Cloudとの統合

## Adobe Analytics for Target（A4T)

Target Delivery API 呼び出しがサーバーから実行されると、Adobe Targetはそのユーザーのエクスペリエンスを返し、それに加えて、Adobe TargetがAdobe Analyticsペイロードを呼び出し元に返すか、自動的にAdobe Analyticsに転送します。 Target のアクティビティ情報をサーバー側のAdobe Analyticsに送信するために、次の前提条件を満たす必要があります。

1. このアクティビティは、Adobe Target UI でAdobe Analyticsをレポートソースとして使用して設定され、アカウントは A4T で有効になっています
1. Adobe Marketing Cloud訪問者 ID は、API ユーザーが生成し、Target 配信 API 呼び出しが実行されたときに使用できます

### Adobe Targetが Analytics ペイロードを自動的に転送します

次の識別子が指定されている場合、Adobe Targetは、サーバー側を介して analytics ペイロードをAdobe Analyticsに自動的に転送できます。

1. `supplementalDataId` - Adobe AnalyticsとAdobe Targetの間でステッチに使用される ID。
1. `trackingServer` -Adobe分析サーバーAdobe TargetとAdobe Analyticsを正しく結び付けるために、同じように `supplementalDataId` Adobe TargetとAdobe Analyticsの両方に渡す必要があります。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
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
      "id": {
        "marketingCloudVisitorId": "2304820394812039"
      },
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
      },
      "experienceCloud": {
        "analytics": {
          "supplementalDataId" : "23423498732598234",
          "trackingServer": "ags041.sc.omtrdc.net",
          "logging": "server_side"
        }
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
            "index" : 1
          }
        ]
      }
    }'
```

### Adobe Targetから Analytics ペイロードを取得する

Adobe Target Delivery API の利用者は、対応する mbox のAdobe Analyticsペイロードを取得し、消費者が [Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md). サーバー側Adobe Target呼び出しが実行されたら、 `client_side` から `logging` フィールドに値を入力します。 Analytics をレポートソースとして使用しているアクティビティに mbox が存在する場合、これはペイロードを返します。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
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
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
      },
      "experienceCloud": {
        "analytics": {
          "logging": "client_side"
        }
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
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

指定した後 `logging` = `client_side`に設定されている場合、 `mbox` フィールドの値を指定します。

```
{
    "status": 200,
    "requestId": "4b8855a5-8354-4ac4-8ae7-c551f7c0bb8a",
    "client": "demo",
    "id": {
        "tntId": "d359234570e04f14e1faeeba02d6ab9914e.28_7"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "execute": {
        "mboxes": [
            {
                "index": 1,
                "name": "homepage",
                "options": [
                    {
                        "content": "<p><b>Enjoy this 15% discount on your next purchase</b></p>",
                        "type": "html",

                    }
                ],
                "analytics": {
                    "payload": {
                        "pe": "tnt",
                        "tnta": "285408:0:0|2"
                    }
                }
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

Target からの応答に `analytics` -> `payload` プロパティを指定し、そのままAdobe Analyticsに転送します。 Analytics は、このペイロードの処理方法を把握しています。 これは、次の形式を使用して、GETリクエストでおこなうことができます。

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/0/CODEVERSION?pe=tnt&tnta={payload}&mid={mid}&vid={vid}&aid={aid}
```

### クエリー文字列パラメーターおよび変数

| フィールド名 | 必須 | 説明 |
| --- | --- | --- |
| `rsid` | ○ | レポートスイート ID |
| `pe` | ○ | ページイベント。 常にに設定 `tnt` |
| `tnta` | ○ | で Target サーバーによって返された Analytics ペイロード `analytics` -> `payload` -> `tnta` |
| `mid` | Marketing Cloud 訪問者 ID |

### 必須ヘッダー値

| ヘッダー名 | ヘッダー値 |
| --- | --- |
| ホスト | Analytics データ収集サーバー ( 例： adobeags421.sc.omtrdc.net) |

### A4T データ挿入 HTTP Get 呼び出しの例

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0?pe=tnt&tnta=285408:0:0|2&mid=2304820394812039
```

## Adobe Audience Manager

Adobe Audience Manager(AAM) セグメントは、Adobe Target Delivery API を使用しても利用できます。 AAMセグメントを活用するには、次のフィールドを指定する必要があります。

| フィールド名 | 必須 | 説明 |
| --- | --- | --- |
| `locationHint` | ○ | DCS ロケーションヒントは、プロファイルを取得するために、どのAAM DCS エンドポイントをヒットするかを決定するために使用されます。 >= 1 である必要があります。 |
| `marketingCloudVisitorId` | ○ | Marketing Cloud 訪問者 ID |
| `blob` | ○ | AAM Blob は、追加データをAAMに送信するために使用されます。 空白およびサイズ &lt;= 1024 は指定できません。 |

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
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
      "id": {
        "marketingCloudVisitorId": "2304820394812039"
      },
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
      },
      "experienceCloud": {
        "audienceManager": {
          "locationHint": 9,
          "blob": "32fdghkjh34kj5h43"
        }
      },
        "execute": {
        "mboxes" : [
          {
            "name" : "homepage",
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
