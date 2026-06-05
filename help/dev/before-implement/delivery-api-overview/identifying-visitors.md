---
title: Adobe Target Delivery APIによる訪問者の識別
description: ' [!DNL Adobe Target]内のユーザーを特定するにはどうすればよいですか？'
keywords: Delivery API
exl-id: 5b8c28aa-caad-44a9-880a-3c5f844e47b2
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/ciTxaPn8odyuyHzrnqhPWzdmpcU2bknOATGCt-ZtAZw
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 797
ht-degree: 11%

---

# 訪問者の特定

[!DNL Adobe Target]内で訪問者を識別する方法は複数あります。

Targetでは、次の3つの識別子を使用します。

| フィールド名 | 説明 |
| --- | --- |
| `tntId` | `tntId`は、ユーザーの[!DNL Target]のプライマリ IDです。 このIDを指定できます。リクエストにIDが含まれていない場合は、[!DNL Target]が自動生成します。 |
| `thirdPartyId` | `thirdPartyId`は、電話のたびに送信できるユーザーの会社のIDです。 ユーザーが企業のサイトにログインすると、通常、その企業は、訪問者のアカウント、ロイヤルティカード、会員番号、またはその他の該当する識別子に関連付けられたIDを作成します。 |
| `marketingCloudVisitorId` | `marketingCloudVisitorId`は、異なるAdobe ソリューション間でデータを結合して共有するために使用されます。 Adobe AnalyticsおよびAdobe Audience Managerとの統合には、`marketingCloudVisitorId`が必要です。 |
| `customerIds` | Experience Cloud訪問者IDと共に、追加の[顧客ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja)と各訪問者の認証済みステータスを使用できます。 |

## [!DNL Target] ID

[!DNL Target] IDまたは`tntId`は、デバイス IDとして表示できます。 この`tntId`は、リクエストで指定されていない場合、[!DNL Target]によって自動的に生成されます。 その後、ユーザーが使用するデバイスに適切なコンテンツを配信するために、その後のリクエストには、この`tntId`を含める必要があります。

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

上記の呼び出しの例は、`tntId`を渡す必要がないことを示しています。 このシナリオでは、[!DNL Target]は`tntId`を生成し、次に示すように、応答に提供します。

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

生成された`tntId`は`10abf6304b2714215b1fd39a870f01afc.28_20`です。 セッション間で同じユーザーに[!UICONTROL Adobe Target Delivery API]を呼び出す場合は、この`tntId`を使用する必要があります。

## Marketing Cloud 訪問者 ID

`marketingCloudVisitorId`は、Experience Cloud内のすべてのソリューションで訪問者を識別する、ユニバーサルで永続的なIDです。 組織がID サービスを実装する場合、このIDを使用すると、Adobe Target、Adobe Analytics、Adobe Audience ManagerなどのさまざまなExperience Cloud ソリューションで同じサイト訪問者とそのデータを識別できます。 AnalyticsとAudience Managerを活用して統合する場合は、`marketingCloudVisitorId`が必要です。

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

上記の呼び出しの例では、Experience Cloud ID サービスから取得した`marketingCloudVisitorId`をAdobe Targetに渡す方法を示しています。 このシナリオでは、[!DNL Target]は元の呼び出しに渡されていないため、`tntId`を生成します。これは、以下の応答に示すように、指定された`marketingCloudVisitorId`にマッピングされます。

## サードパーティ ID

組織がIDを使用して訪問者を識別する場合は、`thirdPartyID`を使用してコンテンツを配信できます。 ただし、[!UICONTROL Adobe Target Delivery API]呼び出しごとに`thirdPartyID`を指定する必要があります。

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

上記の呼び出しの例では、`thirdPartyId`を示しています。これは、web、モバイル、IoT チャネルのいずれからビジネスを利用しているかに関係なく、エンドユーザーを識別するためにビジネスが使用する永続的なIDです。 つまり、`thirdPartyId`は、チャネル全体で利用できるユーザープロファイルデータを参照します。 このシナリオでは、[!DNL Target]は元の呼び出しに渡されていないため、`tntId`を生成します。これは、以下の応答に示すように、提供された`thirdPartyId`にマッピングされます。

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

[顧客ID](https://experienceleague.adobe.com/docs/id-service/using/reference/authenticated-state.html?lang=ja)を追加し、Experience Cloud訪問者IDに関連付けることができます。 `customerIds`を送信するたびに、`marketingCloudVisitorId`も指定する必要があります。 さらに、各訪問者に対して、認証ステータスを各`customerId`と共に提供できます。 次の認証ステータスを考慮できます。

| 認証状態 | ユーザーステータス |
| --- | --- |
| `unknown` | 不明または認証なし。 この状態は、ディスプレイ広告をクリックして、サイトにアクセスした訪問者などのシナリオで使用できます。 |
| `authenticated` | ユーザーは現在、Web サイトまたはアプリのアクティブセッションで認証されています。 |
| `logged_out` | ユーザーは過去に認証されましたが、アクティブにログアウトしました。 ユーザーは意図的に認証済みの状態から切断しました。 ユーザーは、認証済みとして扱われることを希望していません。 |

顧客IDが`authenticated`状態の場合にのみ、Targetは顧客IDに保存されリンクされているユーザープロファイルデータを参照することに注意してください。 顧客IDが`unknown`または`logged_out`状態の場合、顧客IDは無視され、それに関連付けられている可能性のあるユーザープロファイルデータはオーディエンスターゲティングに活用されません。

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

上記の呼び出しの例では、`authenticatedState`を含む`customerId`を送信する方法を示しています。 `customerId`を送信する場合、`integrationCode`、`id`、`authenticatedState`および`marketingCloudVisitorId`が必要です。 `integrationCode`は、CRSを通じて指定した[顧客属性ファイル &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/working-with-customer-attributes.html?lang=ja)のエイリアスです。

## 結合プロファイル

同じリクエストで`tntId`、`thirdPartyID`および`marketingCloudVisitorId`を組み合わせることができます。 この場合、Adobe Targetは、これらすべてのIDのマッピングを管理し、訪問者に固定します。 様々な識別子を使用してプロファイルを[統合し、リアルタイムで](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html?lang=ja)同期する方法について説明します。

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

上記の呼び出しの例では、同じリクエストで`tntId`、`thirdPartyID`および`marketingCloudVisitorId`を組み合わせる方法を示しています。 3つのIDはすべて、応答でも返されます。
