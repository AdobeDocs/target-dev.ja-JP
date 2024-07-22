---
title: 訪問者を特定するAdobe Target配信 API
description: ' [!DNL Adobe Target] 内のユーザーを識別するにはどうすればよいですか？'
keywords: 配信 api
exl-id: 5b8c28aa-caad-44a9-880a-3c5f844e47b2
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '751'
ht-degree: 8%

---

# 訪問者の特定

[!DNL Adobe Target] 内で訪問者を識別する方法は複数あります。

Target では、次の 3 つの識別子を使用します。

| フィールド名 | 説明 |
| --- | --- |
| `tntId` | `tntId` は、ユーザーの [!DNL Target] のプライマリ識別子です。 この ID を指定できます。指定しない [!DNL Target]、リクエストに ID が含まれていない場合は自動生成されます。 |
| `thirdPartyId` | `thirdPartyId` は、すべての呼び出しで送信できるユーザーの会社の識別子です。 ユーザーが会社のサイトにログインすると、会社は通常、訪問者のアカウント、ロイヤルティカード、メンバーシップ番号、またはその会社に適用されるその他の識別子に関連付けられた ID を作成します。 |
| `marketingCloudVisitorId` | `marketingCloudVisitorId` は、異なるAdobeソリューション間でデータを結合して共有するために使用されます。 `marketingCloudVisitorId` は、Adobe AnalyticsおよびAdobe Audience Managerとの統合に必要です。 |
| `customerIds` | Experience Cloudの訪問者 ID と共に、追加の [ 顧客 ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja) および各訪問者の認証済みステータスを利用できます。 |

## [!DNL Target] ID

[!DNL Target] ID または `tntId` は、デバイス ID と見なすことができます。 この `tntId` は、リクエストで指定されていない場合、[!DNL Target] によって自動的に生成されます。 その後、ユーザーが使用するデバイスに適切なコンテンツを配信するには、後続のリクエストにこの `tntId` を含める必要があります。

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

上記の呼び出し例は、`tntId` を渡す必要がないことを示しています。 このシナリオでは、次に示 [!DNL Target] ように、が `tntId` を生成し、応答で提供します。

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

生成された `tntId` は `10abf6304b2714215b1fd39a870f01afc.28_20` です。 セッションをまたいで同じユーザーの [!UICONTROL Adobe Target Delivery API] を呼び出す場合は、この `tntId` を使用する必要があることに注意してください。

## Marketing Cloud 訪問者 ID

`marketingCloudVisitorId` は、Experience Cloud内のすべてのソリューションをまたいで訪問者を特定する、普遍的で永続的な ID です。 ID サービスを実装する場合、この ID を使用すると、Adobe Target、Adobe Analytics、Adobe Audience Managerなどの異なるExperience Cloudソリューションで、同じサイト訪問者とそのデータを識別できます。 を活用して Analytics やAudience Managerと統合する場合は、`marketingCloudVisitorId` が必要です。

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

上記の呼び出し例は、Experience CloudID サービスから取得された `marketingCloudVisitorId` をAdobe Targetに渡す方法を示しています。 このシナリオでは、[!DNL Target] は、元の呼び出しに渡されなかったので `tntId` を生成します。これは、以下の応答のように、指定された `marketingCloudVisitorId` にマッピングされます。

## サードパーティ ID

組織が ID を使用して訪問者を識別する場合は、`thirdPartyID` を使用してコンテンツを配信できます。 ただし、[!UICONTROL Adobe Target Delivery API] 呼び出しを行うたびに `thirdPartyID` を指定する必要があります。

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

上記の呼び出し例では、Web、モバイル、IoT チャネルのいずれかからビジネスとやり取りしているかどうかに関係なく、ビジネスがエンドユーザーを識別するために利用する永続的な ID である `thirdPartyId` が示されています。 つまり、`thirdPartyId` は、チャネルをまたいで利用できるユーザープロファイルデータを参照します。 このシナリオでは、元の呼び出しに渡されなかったので、[!DNL Target] は `tntId` を生成します。これは、以下の応答で見られるように、指定された `thirdPartyId` にマッピングされます。

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

[ 顧客 ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja) を追加し、Experience Cloudの訪問者 ID に関連付けることができます。 送信 `customerIds` る場合は常に、`marketingCloudVisitorId` も指定する必要があります。 さらに、各訪問者毎に、各 `customerId` と共に認証状況を提供することができる。 次の認証ステータスが考慮されます。

| 認証状態 | ユーザーステータス |
| --- | --- |
| `unknown` | 不明または認証されていません。 この状態は、ディスプレイ広告をクリックしてサイトに訪問した訪問者などのシナリオで使用できます。 |
| `authenticated` | ユーザーは現在、Web サイトまたはアプリのアクティブセッションで認証されています。 |
| `logged_out` | ユーザーは過去に認証されましたが、アクティブにログアウトしました。ユーザーは意図的に認証済みの状態から切断しました。ユーザーは、認証済みとして扱われることを希望していません。 |

顧客 ID が `authenticated` の状態にある場合にのみ、Target は、保存され、顧客 ID にリンクされているユーザープロファイルデータを参照することに注意してください。 顧客 ID が `unknown` または `logged_out` の状態の場合、その顧客 ID は無視され、それに関連付けられている可能性のあるユーザープロファイルデータは、オーディエンスのターゲティングに利用されません。

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

上記の呼び出し例は、`authenticatedState` を持つ `customerId` を送信する方法を示しています。 `customerId` を送信する場合、`integrationCode`、`id`、`authenticatedState` および `marketingCloudVisitorId` が必要です。 `integrationCode` は、CRS で指定した [ 顧客属性ファイル ](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html?lang=ja) のエイリアスです。

## 結合プロファイル

`tntId`、`thirdPartyID`、`marketingCloudVisitorId` を同じリクエストで組み合わせることができます。 このシナリオでは、Adobe Targetは、これらすべての ID のマッピングを保持し、訪問者にピン留めします。 様々な識別子を使用してプロファイルを [ リアルタイムで結合および同期 ](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html) する方法について説明します。

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

上記の呼び出し例は、`tntId`、`thirdPartyID`、`marketingCloudVisitorId` を同じリクエストで組み合わせる方法を示しています。 3 つの ID もすべて応答で返されます。
