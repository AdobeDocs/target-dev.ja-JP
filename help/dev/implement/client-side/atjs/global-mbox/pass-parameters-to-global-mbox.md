---
keywords: グローバル mbox パラメーター，targetPageParams, クエリ文字列，配列，json, dtm
description: '[!UICONTROL targetPageParams] 関数を使用して、追加のターゲティングまたはコンテキスト情報をグローバル mbox に渡す方法を説明  [!DNL Adobe Target]  ます。'
title: グローバル mbox にパラメーターを渡すにはどうすればよいですか？
feature: at.js
exl-id: 2a6be3e4-a618-4812-9e87-b01789705c40
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '370'
ht-degree: 62%

---

# グローバル mbox にパラメーターを渡す

JavaScript `targetPageParams` 関数は、[!DNL Adobe Target] のグローバル mbox にパラメーターを渡すために使用されます。 これは、追加のターゲティングやコンテキストの情報を [!DNL Target] に渡す必要があるすべてのシナリオで必要です。

例えば、Recommendations アクティビティでは、表示されている現在の製品またはカテゴリを示すために、これらのパラメーターを使用します。

JavaScript関数を呼び出すコードは、グローバル mbox が at.js の一部として呼び出されるか、ページコードに手動で含まれているかに関係なく、ページ上のグローバル mbox の前に置く必要があります。

>[!NOTE]
>
>グローバル mbox だけでなく、ページ上のすべての mbox にパラメーターを追加する場合は、[targetPageParamsAll （） ](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparamsall.md) 関数を使用します。

次のいずれかの方法で、`targetPageParams()` 関数を使用して `target-global-mbox` にパラメーターを渡せます。

* 配列
* JSON オブジェクト
* アンパサンド区切りのリスト

これら 3 つの方法を使用して、パラメーターが正しく渡されていることを確認できます。また、[Adobe Experience Cloud デバッガー](https://experienceleague.adobe.com/docs/debugger/using/experience-cloud-debugger.html?lang=ja)を使用して、パラメーターの受け渡しを確認することもできます。

ページにグローバル mbox を追加する前に、JavaScript 関数を定義する必要があります。名前は `targetPageParams` とします。

## クエリ文字列

```
p1=v1&p2=v2&p3=hello%20world
```

* 名前: `targetPageParams`
* 戻り値：パラメーターの値が URL エンコードされた、「&amp;」文字で区切られた一連のパラメーター

  例：

  この例では、p3 の値は `hello world` です。この値は、URL エンコードされています。

次のページコードの例について考えてみます。

```html {line-numbers="true"}
<html> 
  <head> 
    <title>Title here..</title> 
    <script type="text/javascript"> 
        function targetPageParams() { 
          return "p1=v1&p2=v2&p3=hello%20world";
        } 
    </script> 
    <script src="mbox.js" type="text/javascript"></script> 
  </head> 
  <body>Body here... 
  </body> 
</html>
```

この例では、次のデータを mbox エッジに送ります。

* p1=v1
* p2=v2
* p3=hello world

## 配列

```javascript {line-numbers="true"}
<!--window.-->targetPageParams = function() { 
  return ["a=1", "b=2", "c=hello world"]; 
}; 
```

値を URL エンコードする必要はありません。例えば、値にスペースが含まれている場合でも、スペースをエンコードする必要はありません。

この例では、次のデータを mbox エッジに送ります。

* a=1
* b=2
* c=hello world

## JSON

JSON は、パラメーターを渡すための強力な機能を備えています。[!DNL Target] では、JSON オブジェクトキーを使用して、複雑な構造を単純なパラメーターに統合します。

```json {line-numbers="true"}
<!--window.-->targetPageParams = function() { 
  return { 
    "a": 1, 
    "b": 2, 
    "profile": { 
                  "memberStatus": Gold, 
                  "country": { 
                                "city": "San Francisco" 
                            } 
              } 
  }; 
}; 
```

値を URL エンコードする必要はありません。例えば、「San Francisco」という値でも、スペースをエンコードする必要はありません。普通にスペースを挿入するだけです。

この例では、次のデータを mbox エッジに送ります。

* a=1
* b=2
* `profile.memberStatus`=Gold
* `profile.country.city`=San Francisco
