---
keywords: targetPageParams、targetpageparams、pageParams、pageparams、ページパラメーター、at.js、functions、function、targetPageParams0
description: at.js JavaScript ライブラリの [!UICONTROL targetPageParams()] 関数を使用して  [!DNL Adobe Target]  リクエストコードの外部からグローバル mbox にパラメーターを添付します。
title: '[!UICONTROL targetPageParams()] 関数の使用方法'
feature: at.js
exl-id: 274e4d1f-843a-443b-ad98-7139dc4a13f8
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '159'
ht-degree: 68%

---

# [!UICONTROL targetPageParams()]

このメソッドにより、リクエストコードの外部からグローバル mbox にパラメーターを付加できます。

この関数は、同じパラメーターのセットを複数の mbox 呼び出しに含めるのに非常に便利です。この関数は、お客様によって定義される必要があります。グローバル mbox リクエストにのみ渡されるパラメーターの配列を返す必要があります。この関数は、at.js が読み込まれる前か、**[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Edit]** > **[!UICONTROL Library Header]** で定義できます。

次のいずれかの方法で、`[!UICONTROL targetPageParams()]` 関数を使用して target-global-mbox にパラメーターを渡せます。

* アンパサンド区切りのリスト
* 配列
* JSON オブジェクト

## 例

アンパサンド区切りのリスト（値は URL エンコードされている必要がある）：

```javascript {line-numbers="true"}
function targetPageParams() { 
    return "param1=value1&param2=value2&p3=hello%20world"; 
}
```

配列（値を URL エンコードする必要はない）：

```javascript {line-numbers="true"}
targetPageParams = function() { 
     return ["a=1", "b=2", "c=hello world"]; 
};
```

JSON（値を URL エンコードする必要はない）：

```javascript {line-numbers="true"}
targetPageParams = function() { 
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
