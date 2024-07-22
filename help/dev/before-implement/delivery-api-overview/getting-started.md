---
title: Adobe Target配信 API の概要
description: '[!UICONTROL Adobe Target Delivery API] の使い方'
keywords: 配信 api
exl-id: 142ec3be-b017-4cdc-9079-b1cc173a710a
feature: APIs/SDKs
source-git-commit: e5a1c38d448cb7446b7b26cd0dc882976ba94dd3
workflow-type: tm+mt
source-wordcount: '96'
ht-degree: 0%

---

# [!UICONTROL Adobe Target Delivery API] の基本を学ぶ

[!UICONTROL Target Delivery API] 呼び出しは、次のようになります。

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

`clientCode` は、**[!UICONTROL Administration]** / **[!UICONTROL Implementation]** に移動して、[!DNL Target] UI から取得できます。

[!UICONTROL Target Delivery API] 呼び出しを行う前に、次の手順に従って、エンドユーザーに表示する関連エクスペリエンスが応答に含まれていることを確認します。

1. [ フォームベースのコンポーザー ](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) または [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=en) を使用して、[!DNL Target] アクティビティ（A/B、XT、AP またはRecommendations）を作成します。
1. 配信 API を使用して、手順 2 で作成した [!DNL Target] アクティビティで使用されている mbox の応答を取得します。
1. 訪問者にエクスペリエンスを提示します。
