---
title: Adobe Target Delivery APIの概要
description: '[!UICONTROL Adobe Target Delivery API]の使用方法を教えてください。'
keywords: Delivery API
exl-id: 142ec3be-b017-4cdc-9079-b1cc173a710a
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/DC-YVq6VfAaqMU1utmIMw73gzp4PIJgQjaS0a8FQEO4
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 132
ht-degree: 1%

---

# [!UICONTROL Adobe Target Delivery API]の概要

[!UICONTROL Target Delivery API]呼び出しは次のようになります。

```
curl -X POST \
  'https://`clientCode`.tt.omtrdc.net/rest/v1/delivery?client=`clientCode`&sessionId=d359234570e04f14e1faeeba02d6ab9914e' \
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
            "name" : "homepage",
            "index" : 1
          }
        ]
      }
    }'
```

`clientCode`を[!DNL Target] UIから取得するには、**[!UICONTROL 管理]** > **[!UICONTROL 実装]**&#x200B;に移動します。

[!UICONTROL Target Delivery API]呼び出しを行う前に、次の手順に従って、応答にエンドユーザーに表示する関連するエクスペリエンスが含まれていることを確認します。

1. [&#x200B; フォームベースのコンポーザー](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=en)または[Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html)を使用して、[!DNL Target] アクティビティ（A/B、XT、APまたはRecommendations）を作成します。
1. 配信APIを使用して、手順2で作成した[!DNL Target] アクティビティで使用されるmboxの応答を取得します。
1. 訪問者にエクスペリエンスを提示します。
