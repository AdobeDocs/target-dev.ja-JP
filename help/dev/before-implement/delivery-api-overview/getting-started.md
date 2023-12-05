---
title: Adobe Target Delivery API はじめに
description: 使用方法 [!UICONTROL Adobe Target Delivery API]?
keywords: 配信 api
exl-id: 142ec3be-b017-4cdc-9079-b1cc173a710a
feature: APIs/SDKs
source-git-commit: e5a1c38d448cb7446b7b26cd0dc882976ba94dd3
workflow-type: tm+mt
source-wordcount: '112'
ht-degree: 0%

---

# の概要 [!UICONTROL Adobe Target Delivery API]

A [!UICONTROL Target 配信 API] 呼び出しは次のようになります。

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

The `clientCode` は、 [!DNL Target] に移動して UI を表示 **[!UICONTROL 管理]** > **[!UICONTROL 実装]**.

を作成する前に [!UICONTROL Target 配信 API] を呼び出す場合は、次の手順に従って、応答にエンドユーザーに表示する関連するエクスペリエンスが含まれていることを確認します。

1. の作成 [!DNL Target] アクティビティ (A/B、XT、AP またはRecommendations) [フォームベースのコンポーザー](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html?lang=en) または [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html).
1. 配信 API を使用して、 [!DNL Target] 手順 2 で作成したアクティビティ。
1. 訪問者にエクスペリエンスを提示します。
