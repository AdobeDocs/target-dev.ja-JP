---
title: Adobe Target Delivery API による訪問者の識別
description: 内でユーザーを識別する方法 [!DNL Adobe Target]?
keywords: 配信 api
exl-id: 5b8c28aa-caad-44a9-880a-3c5f844e47b2
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 10%

---

# 訪問者の特定

訪問者を識別する方法は複数あります。 [!DNL Adobe Target].

Target では、次の 3 つの識別子を使用します。

| フィールド名 | 説明 |
| --- | --- |
| `tntId` | The `tntId` は、 [!DNL Target] を設定します。 この ID または [!DNL Target] リクエストに含まれていない場合は自動生成されます。 |
| `thirdPartyId` | The `thirdPartyId` は、呼び出しごとに送信できるユーザーの会社識別子です。 ユーザーが会社のサイトにログインすると、通常、会社は訪問者のアカウント、ロイヤルティカード、メンバーシップ番号、またはその会社で適用可能なその他の識別子に結び付けられる ID を作成します。 |
| `marketingCloudVisitorId` | The `marketingCloudVisitorId` は、異なるデータソリューション間でデータを結合および共有するためにAdobeされます。 The `marketingCloudVisitorId` は、Adobe AnalyticsとAdobe Audience Managerとの統合に必要です。 |
| `customerIds` | Experience Cloud訪問者 ID と共に、 [顧客 ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja) また、各訪問者の認証済みステータスを利用できます。 |

## [!DNL Target] ID

The [!DNL Target] ID または `tntId` はデバイス ID と見なすことができます。 この `tntId` が次の項目で自動的に生成されます： [!DNL Target] を返します。 その後、以降のリクエストには、これを含める必要があります `tntId` ユーザが使用するデバイスに適切なコンテンツを配信するため。

```http {line-numbers="true"}
curl -X POST \
'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
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

上記の呼び出し例は、 `tntId` を渡す必要はありません。 このシナリオでは、 [!DNL Target]  はを生成します。 `tntId` 次に示すように、レスポンスで指定します。

```URI {line-numbers="true"}
{
  "status": 200,
  "requestId": "5b586f83-890c-46ae-93a2-610b1caa43ef",
  "client": "demo",
  "id": {
      "tntId": "10abf6304b2714215b1fd39a870f01afc.28_20"
  },
  "edgeHost": "mboxedge28.tt.omtrdc.net",
  ...
}
```

生成された `tntId` 次に該当 `10abf6304b2714215b1fd39a870f01afc.28_20`. これに注意してください `tntId` を呼び出す際に使用する必要がある [!UICONTROL Adobe Target Delivery API] セッションをまたいで同じユーザーに対して

## Marketing Cloud 訪問者 ID

The `marketingCloudVisitorId` は、Experience Cloud内のすべてのソリューションにわたって訪問者を識別する、普遍的、永続的な ID です。 組織が ID サービスを実装している場合、この ID を使用すると、Adobe Target、Adobe Analytics、Adobe Audience Managerなどの異なるExperience Cloudソリューションで、同じサイト訪問者とそのデータを識別できます。 なお、 `marketingCloudVisitorId` は、Analytics や Analytics との活用および統合時に必要です。Audience Manager

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "marketingCloudVisitorId": "10527837386392355901041112038610706884"
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

上記の呼び出し例は、 `marketingCloudVisitorId` Experience CloudID サービスから取得されたデータがAdobe Targetに渡されます。 このシナリオでは、 [!DNL Target] はを生成します。 `tntId` これは、指定された `marketingCloudVisitorId` 以下の応答で示すように。

## サードパーティ ID

組織で ID を使用して訪問者を識別している場合、 `thirdPartyID` コンテンツを配信します。 ただし、 `thirdPartyID` 次の期間ごと [!UICONTROL Adobe Target Delivery API] 呼び出しを行う。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=10abf6304b2714215b1fd39a870f01afc#1555632114' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -d '{
  "id": {
    "thirdPartyId": "B234A029348"
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

上記の呼び出しの例は、 `thirdPartyId`：ビジネスが Web、モバイル、IoT の各チャネルからビジネスとやり取りしているかどうかに関係なく、エンドユーザーを識別するために使用する永続的な ID です。 つまり、 `thirdPartyId` は、チャネル間で使用できるユーザープロファイルデータを参照します。 このシナリオでは、 [!DNL Target] はを生成します。 `tntId`、元の呼び出しに渡されなかったので、指定された `thirdPartyId` 以下の応答で示すように。

```
{
    "status": 200,
    "requestId": "55de9886-bd14-4dee-819c-7d1633b79b90",
    "client": "demo",
    "id": {
        "tntId": "10abf6304b2714215b1fd39a870f01afc.28_20",
        "thirdPartyId": "B234A029348"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    ...
}
```

## 顧客 ID

[顧客 ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja) を追加し、訪問者訪問者 ID に関連付けることができますExperience Cloud訪問者 ID。 送信時 `customerIds` の `marketingCloudVisitorId` また、を指定する必要があります。 また、各 `customerId` 各訪問者に対して 次の認証ステータスを考慮に入れることができます。

| 認証状態 | ユーザーステータス |
| --- | --- |
| `unknown` | 不明または認証なし。この状態は、表示広告をクリックしてサイトにランディングした訪問者などのシナリオに使用できます。 |
| `authenticated` | ユーザーは現在、Web サイトまたはアプリのアクティブセッションで認証されています。 |
| `logged_out` | ユーザーは過去に認証されましたが、アクティブにログアウトしました。ユーザーは意図的に認証済みの状態から切断しました。ユーザーは、認証済みとして扱われることを希望していません。 |

なお、顧客 ID が `authenticated` state は、顧客 id に保存され、リンクされているユーザープロファイルデータを Target が参照します。 顧客 ID が `unknown` または `logged_out` の場合、顧客 id は無視され、それに関連付けられる可能性のあるユーザープロファイルデータは、オーディエンスのターゲティングに利用されません。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e044f14e1faeeba02d6ab23439914e' \
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
        "marketingCloudVisitorId" : "2304820394812039",
        "customerIds": [{
          "id": "134325423",
          "integrationCode" : "crm_data",
          "authenticatedState" : "authenticated"
        }]
      },
      "property" : {
        "token": "08b62abd-c3e7-dfb2-da93-96b3aa724d81"
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

上記の呼び出し例は、 `customerId` と `authenticatedState`. 送信時に `customerId`、 `integrationCode`, `id`、および `authenticatedState` 同様に `marketingCloudVisitorId` が必要です。 The `integrationCode` は、 [顧客属性ファイル](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html?lang=ja) CRS を通じて提供された。

## 結合プロファイル

組み合わせ可能な `tntId`, `thirdPartyID`、および `marketingCloudVisitorId` 同じリクエストに含まれます。 このシナリオでは、Adobe Targetは、これらすべての ID のマッピングを維持し、訪問者にピン留めします。 プロファイルの概要 [リアルタイムで結合および同期されました](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html) 様々な識別子を使用している。

```
curl -X POST \
  'https://demo.tt.omtrdc.net/rest/v1/delivery?client=demo&sessionId=d359234570e044f14e1faeeba02d6ab23439914e' \
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
        "marketingCloudVisitorId" : "2304820394812039",
        "tntId": "d359234570e044f14e1faeeba02d6ab23439914e.28_78",
        "thirdPartyId":"23423432"
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

上記の呼び出し例は、 `tntId`, `thirdPartyID`、および `marketingCloudVisitorId` 同じリクエストに含まれます。 3 つの ID すべても応答で返されます。
