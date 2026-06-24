---
keywords: targetPageParamsAll, targetpageparamsAll, PageParamsAll, pageparamsAll, pageparamsall, page parameters, at.js, functions, function, targetPageParamsAll0
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリの[!UICONTROL targetPageParamsAll （） &#x200B;]関数を使用して、リクエストコードの外部からすべてのmboxにパラメーターを添付します。'
title: '[!UICONTROL targetPageParamsAll （） &#x200B;]関数の使用方法を教えてください。'
feature: at.js
exl-id: 32045e60-6904-42a1-bf71-fd7e167a829f
TQID: https://experienceleague.adobe.com/A2sZYp7CeE3-zGcqfbvgo32auAtXBKN0dYNa84grs1Q
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 4d0e7f9f2887db71229061fa64b2633a84c6d054
workflow-type: tm+mt
source-wordcount: 171
ht-degree: 54%

---

# [!UICONTROL targetPageParamsAll （） &#x200B;]

このメソッドにより、リクエストコードの外部からすべての mbox にパラメーターを付加できます。

これは、同じパラメーターのセットを複数の mbox 呼び出しに含めるのに非常に便利です。 この関数は、お客様によって定義される必要があります。 ページでリクエストするすべての mbox に渡されるパラメーターの配列を返す必要があります。 この関数は、at.jsが読み込まれる前、または&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 実装]** > **[!UICONTROL 編集]** > **[!UICONTROL コード設定]** > **[!UICONTROL ライブラリヘッダー]**&#x200B;で定義できます。

[!UICONTROL targetPageParamsAll （） &#x200B;]関数を使用して、次のいずれかの方法でパラメーターをtarget-global-mboxに渡すことができます。

* アンパサンド区切りのリスト
* 配列
* JSON オブジェクト

## 例

アンパサンド区切りのリスト（値は URL エンコードされている必要がある）：

```javascript {line-numbers="true"}
function targetPageParamsAll() { 
    return "param1=value1&param2=value2&p3=hello%20world"; 
}
```

配列（値を URL エンコードする必要はない）：

```javascript {line-numbers="true"}
targetPageParamsAll = function() { 
     return ["a=1", "b=2", "c=hello world"]; 
};
```

JSON（値を URL エンコードする必要はない）：

```javascript {line-numbers="true"}
targetPageParamsAll = function() { 
  return { 
    "a": 1, 
    "b": 2, 
    "profile": { 
        "age": 26, 
        "country": { 
          "city": "San Francisco" 
        } 
      } 
  }; 
};
```

