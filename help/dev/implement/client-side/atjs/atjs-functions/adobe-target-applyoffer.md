---
keywords: adobe.target.applyOffer, applyOffer, applyoffer, apply offer, at.js，関数， $8
description: 以下を使用します。 [!UICONTROL adobe.target.applyOffer()] 関数 [!DNL Adobe Target] at.js JavaScript ライブラリを使用して、応答コンテンツを適用します。
title: 使用方法 [!UICONTROL adobe.target.applyOffer()] 機能？
feature: at.js
exl-id: 957bbe92-8012-4bd5-95d6-1ae38b72bb16
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '173'
ht-degree: 69%

---

# [!UICONTROL adobe.target.applyOffer(options)]

この関数は、応答コンテンツを [!DNL Adobe Target].

>[!NOTE]
>
>`applyOffer` には `mbox` パラメーターが必要です。mbox 名が指定されていないと、エラーが発生します。

options パラメーターは必須で、以下の構造を持ちます。

| キー | タイプ | 必須 | 説明 |
|--- |--- |--- |--- |
| mbox | 文字列 | ○ | mbox 名<br />at.js 1.3.0 以降の場合、Target では mbox キーが強制的に使用されます。これまでもこのキーは必須でしたが、Target では、適切な検証がおこなわれ、お客様がこの関数を正しく利用するために、使用が強制されるようになりました。 |
| selector | 文字列または DOM 要素 | × | Target がオファーコンテンツを配置する必要がある HTML 要素を特定するために使用される HTML 要素または CSS セレクター。セレクターが指定されていない場合、Target は、HTML要素でセレクターHEADを使用する必要があると想定します。 |
| オファー | 配列 | ○ | 要素に適用される配列アクション。 |

## 例

次の例に、`[!UICONTROL getOffer]` と `[!UICONTROL applyOffer]` を一緒に使用する方法を示します。

```javascript {line-numbers="true"}
adobe.target.getOffer({   
  "mbox": "mbox",   
  "success": function(offers) {           
        adobe.target.applyOffer( {  
           "mbox": "mbox", 
           "offer": offers  
        } ); 
  },   
  "error": function(status, error) {           
      if (console && console.log) { 
        console.log(status); 
        console.log(error); 
      } 
  }, 
 "timeout": 5000 
}); 
```
