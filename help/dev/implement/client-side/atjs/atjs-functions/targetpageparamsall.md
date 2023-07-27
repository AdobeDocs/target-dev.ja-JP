---
keywords: targetPageParamsAll, targetpageparamsall, PageParamsAll, pageparamsall，ページパラメーター， at.js，関数， targetPageParamsAll0
description: 以下を使用します。 [!UICONTROL targetPageParamsAll()] 関数 [!DNL Adobe Target] at.js JavaScript ライブラリを使用して、リクエストコードの外部からすべての mbox にパラメーターを付加できます。
title: 使用方法 [!UICONTROL targetPageParamsAll()] 機能？
feature: at.js
exl-id: 32045e60-6904-42a1-bf71-fd7e167a829f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '168'
ht-degree: 55%

---

# [!UICONTROL targetPageParamsAll()]

このメソッドにより、リクエストコードの外部からすべての mbox にパラメーターを付加できます。

これは、同じパラメーターのセットを複数の mbox 呼び出しに含めるのに非常に便利です。この関数は、お客様によって定義される必要があります。ページでリクエストするすべての mbox に渡されるパラメーターの配列を返す必要があります。この関数は、at.js が読み込まれる前、または **[!UICONTROL 管理]** > **[!UICONTROL 実装]** > **[!UICONTROL 編集]** > **[!UICONTROL コード設定]** > **[!UICONTROL ライブラリヘッダー]**.

target-global-mbox にパラメーターを渡すには、 [!UICONTROL targetPageParamsAll()] は、次のいずれかの方法で関数を使用します。

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
