---
title: Adobe Target配信 API のユーザー権限
description: Adobe Target配信 API のユーザー権限
badgePremium: label="Premium" type="Positive" url="https://experienceleague.adobe.com/docs/target/using/introduction/intro.html?lang=ja#premium newtab=true" tooltip="Target Premium に含まれる機能を確認してください。"
keywords: 配信 api
exl-id: 332f90bd-4079-4653-aa38-b35837631c94
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '161'
ht-degree: 6%

---

# ユーザー権限（Premium）

[!DNL Adobe] を使用すると、ユーザーがAdobe Targetを使用する際にユーザーの権限を管理できます。 [!UICONTROL Adobe Target Delivery API] 呼び出しを成功させるには、適切な権限を持つトークンを API 呼び出し内で渡す必要があります。 ユーザー権限とトークンの取得方法について詳しくは、[ このドキュメント ](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/properties-overview.html) を参照してください。

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

対応するトークンを取得したら、実行されるすべての API 呼び出しで `property` -> `token` に渡します。 すべての API 呼び出し内で `property` -> `token` が渡されない場合、Adobe Targetから `content` が返されることはありません。

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

上記のように、`property` -> `token` を渡さずに、コンテンツを取得することはできません。 API 呼び出しからコンテンツを期待しても、応答からコンテンツが取得されない場合、`property` -> `token` が指定されていないか、正しい権限なしで渡されていることが原因である可能性が高いです。
