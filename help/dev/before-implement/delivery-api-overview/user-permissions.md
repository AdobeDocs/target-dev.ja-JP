---
title: Adobe Target Delivery APIのユーザー権限
description: Adobe Target Delivery APIのユーザー権限
badgePremium: label="Premium" type="Positive" url="https://experienceleague.adobe.com/docs/target/using/introduction/intro.html?lang=ja#premium newtab=true" tooltip="Target Premium に含まれる機能を確認してください。"
keywords: Delivery API
exl-id: 332f90bd-4079-4653-aa38-b35837631c94
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/V7F8WjDNUMJJySyep0nVCg0wMK05ZfdV4XPtMjXOBvM
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 184
ht-degree: 11%

---

# ユーザー権限（プレミアム）

[!DNL Adobe]では、お客様がAdobe Targetを使用する際に、ユーザーの権限を管理できます。 [!UICONTROL Adobe Target Delivery API]呼び出しを成功させるには、API呼び出し内で適切な権限を持つトークンを渡す必要があります。 ユーザーの許可とトークンの取得方法について詳しくは、[このドキュメント &#x200B;](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html)を参照してください。

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

対応するトークンを取得したら、作成したすべてのAPI呼び出しについて、`property` -> `token`に渡します。 すべてのAPI呼び出し内で`property` -> `token`が渡されない場合、Adobe Targetから`content`は返されません。

```
{
    "status": 200,
    "requestId": "07ce783d-58b9-461c-9f4c-6873aeb00c01",
    "client": "demo",
    "id": {
        "tntId": "d359234570e04f14e1faeeba02d6ab9914e.28_7"
    },
    "edgeHost": "mboxedge28.tt.omtrdc.net",
    "execute": {
        "mboxes": [
            {
                "index": 1,
                "name": "homepage"
            }
        ]
    }
}
```

上記のように、`property` -> `token`を渡さずにコンテンツを返すことはできません。 API呼び出しからコンテンツを期待するが、応答からコンテンツを取得しない場合は、`property` -> `token`が提供されていないか、正しい権限なしで渡されている可能性が高くなります。
