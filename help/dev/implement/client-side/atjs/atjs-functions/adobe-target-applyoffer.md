---
keywords: adobe.target.applyOffer, applyOffer, applyoffer, apply offer, at.js, functions, function, $8
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリの[!UICONTROL adobe.target.applyOffer （） &#x200B;]関数を使用して、応答コンテンツを適用します。'
title: '[!UICONTROL adobe.target.applyOffer （） &#x200B;]関数の使用方法を教えてください。'
feature: at.js
exl-id: 957bbe92-8012-4bd5-95d6-1ae38b72bb16
TQID: https://experienceleague.adobe.com/lrjsIl-gKu1SnrZapxYcoDObvUCGG2ht58QtWbQkYts
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
source-git-commit: 07d851e2344279caeae25e4823ca86b9c17efd63
workflow-type: tm+mt
source-wordcount: 173
ht-degree: 68%

---

# [!UICONTROL adobe.target.applyOffer （options） &#x200B;]

この関数は、[!DNL Adobe Target]を含む応答コンテンツを適用するためのものです。

>[!NOTE]
>
>`applyOffer` には `mbox` パラメーターが必要です。 mbox 名が指定されていないと、エラーが発生します。

options パラメーターは必須で、以下の構造を持ちます。

| キー | タイプ | 必須 | 説明 |
|--- |--- |--- |--- |
| mbox | 文字列 | ○ | mbox 名<br />at.js 1.3.0 以降の場合、Target では mbox キーが強制的に使用されます。 これまでもこのキーは必須でしたが、Target では、適切な検証がおこなわれ、お客様がこの関数を正しく利用するために、使用が強制されるようになりました。 |
| selector | 文字列または DOM 要素 | × | Target がオファーコンテンツを配置する必要がある HTML 要素を特定するために使用される HTML 要素または CSS セレクター。 セレクターが指定されていない場合、TargetはHTML要素がHTML HEADを使用する必要があると仮定します。 |
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


