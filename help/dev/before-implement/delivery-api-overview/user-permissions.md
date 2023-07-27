---
title: Adobe Target Delivery API のユーザー権限
description: Adobe Target Delivery API のユーザー権限
badgePremium: label="Premium" type="Positive" url="https://experienceleague.adobe.com/docs/target/using/introduction/intro.html?lang=ja#premium newtab=true" tooltip="Target Premium に含まれる機能を確認してください。"
keywords: 配信 api
exl-id: 332f90bd-4079-4653-aa38-b35837631c94
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '182'
ht-degree: 10%

---

# ユーザー権限 (Premium)

[!DNL Adobe] を使用すると、ユーザーはAdobe Targetの使用時にユーザーの権限を管理できます。 を成功に導くために [!UICONTROL Adobe Target Delivery API] を呼び出す場合、適切な権限を持つトークンを API 呼び出し内で渡す必要があります。 ユーザー権限とトークン訪問の取得方法の詳細 [このドキュメント](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html).

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

対応するトークンを取得したら、に渡します。 `property` -> `token` を呼び出すたびに使用します。 次の場合、 `property` -> `token` が API 呼び出しのたびに渡されない場合、 `content` Adobe Targetから戻る

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

上記のように、 `property` -> `token`に値を指定しない場合、コンテンツは返されません。 API 呼び出しの内容を期待しているが、応答から何も取得していない場合、おそらく  `property` -> `token` が指定されていないか、正しい権限がない状態で渡されています。
