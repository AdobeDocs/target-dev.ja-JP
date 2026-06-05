---
title: Experience Cloudとの連携
description: Experience Cloudとの連携
keywords: Delivery API
source-git-commit: 67cc93cf697f8d5bca6fedb3ae974e4012347a0b
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 10%

---

<!-- The content on this page was originally pulled from the legacy Delivery API doc site, and was subsequently found to be an outdated version of information now found on [Implement > Server-side > Integration > A4T Reporting](../../implement/server-side/sdk-guides/integration-with-experience-cloud/a4t-reporting.md) and [Implement > Server-side > Integration > AAM Segments](../../implement/server-side/sdk-guides/integration-with-experience-cloud/aam-segments.md). Therefore sunsetting this page, but not deleting it from the repo immediately, pending a more thorough examination to avoid inadvertently deleting relevant content. -->


# Experience Cloudとの連携

## Adobe Analytics for Target （A4T）

Target Delivery API呼び出しがサーバーから実行されると、Adobe Targetはそのユーザーのエクスペリエンスを返します。さらに、Adobe TargetはAdobe Analytics ペイロードを呼び出し元に戻すか、Adobe Analyticsに自動的に転送します。 サーバーサイドのAdobe AnalyticsにTarget アクティビティ情報を送信するには、次の前提条件を満たす必要があります。

1. アクティビティは、レポートソースとしてAdobe Analyticsを使用してAdobe Target UIで設定され、アカウントはA4Tに対して有効になっています
1. Adobe Marketing Cloud訪問者IDはAPI ユーザーによって生成され、Target Delivery API呼び出しが発生したときに使用できます

### Adobe Targetは、Analytics ペイロードを自動転送します

次の識別子が指定されている場合、Adobe Targetは、サーバーサイドを介してAnalytics ペイロードをAdobe Analyticsに自動的に転送できます。

1. `supplementalDataId` - Adobe AnalyticsとAdobe Target間のステッチに使用されるID
1. `trackingServer` - Adobe Analytics Server Adobe TargetとAdobe Analyticsがデータを正しく結合するには、Adobe TargetとAdobe Analyticsの両方に同じ`supplementalDataId`を渡す必要があります。

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

### Adobe TargetからのAnalytics ペイロードの取得

Adobe Target Delivery APIのコンシューマーは、対応するmboxのAdobe Analytics ペイロードを取得できるため、コンシューマーは[Data Insertion API](https://github.com/AdobeDocs/analytics-1.4-apis/blob/master/docs/data-insertion-api/index.md)を介してAdobe Analyticsにペイロードを送信できます。 サーバーサイドのAdobe Target呼び出しが発生したら、リクエストの`logging` フィールドに`client_side`を渡します。 これにより、Analyticsをレポートソースとして使用しているアクティビティにmboxが存在する場合、ペイロードが返されます。

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

`logging` = `client_side`を指定すると、次に示すように、`mbox` フィールドにペイロードが表示されます。

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

Targetからのレスポンスに`analytics` -> `payload` プロパティに何かが含まれている場合は、そのままAdobe Analyticsに転送します。 Analyticsはこのペイロードの処理方法を把握しています。 これは、次の形式を使用してGET リクエストで実行できます。

```
https://{datacollectionhost.sc.omtrdc.net}/b/ss/{rsid}/0/CODEVERSION?pe=tnt&tnta={payload}&mid={mid}&vid={vid}&aid={aid}
```

### クエリー文字列パラメーターと変数

| フィールド名 | 必須 | 説明 |
| --- | --- | --- |
| `rsid` | ○ | レポートスイート ID |
| `pe` | ○ | ページイベント： 常に`tnt`に設定 |
| `tnta` | ○ | Analytics ペイロードがTarget サーバーによって`analytics` -> `payload` -> `tnta`に返されました |
| `mid` | Marketing Cloud 訪問者 ID |  |

### 必須ヘッダー値

| ヘッダー名 | ヘッダー値 |
| --- | --- |
| ホスト | Analytics データ収集サーバー（例：adobeags421.sc.omtrdc.net） |

### A4T データ挿入HTTP Get呼び出しの例

```
https://demo.sc.omtrdc.net/b/ss/myCustomRsid/0/MOBILE-1.0?pe=tnt&tnta=285408:0:0|2&mid=2304820394812039
```

## Adobe Audience Manager

Adobe Audience Manager（AAM）セグメントは、Adobe Target Delivery APIを介して活用することもできます。 AAM セグメントを活用するには、次のフィールドを指定する必要があります。

| フィールド名 | 必須 | 説明 |
| --- | --- | --- |
| `locationHint` | ○ | DCS Location Hintは、プロファイルを取得するためにヒットするAAM DCS エンドポイントを決定するために使用されます。 >= 1にする必要があります。 |
| `marketingCloudVisitorId` | ○ | Marketing Cloud 訪問者 ID |
| `blob` | ○ | AAM Blobは、AAMにデータを送信するために使用されます。 空白にせず、サイズ &lt;= 1024を指定してください。 |

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
