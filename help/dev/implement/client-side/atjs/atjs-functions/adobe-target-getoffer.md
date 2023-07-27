---
keywords: adobe.target.getOffer, getOffer, getoffer, get offer, at.js，関数， $8
description: 以下を使用します。 [!UICONTROL adobe.target.getOffer()] 関数とそのオプション [!DNL Adobe Target] at.js ライブラリを使用して、 [!DNL Target] オファー。
title: 使用方法 [!UICONTROL adobe.target.getOffer()] 機能？
feature: at.js
exl-id: 7b917d42-06e8-4838-a09d-0c4872c9beaa
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '470'
ht-degree: 81%

---

# [!DNL adobe.target.getOffer(options)]

この関数は、 [!DNL Target] オファー。

`[!UICONTROL adobe.target.applyOffer()]` と併用して、応答を処理するか、独自の成功処理を使用します。options パラメーターは必須で、以下の構造を持ちます。

| キー | タイプ | 必須 | 説明 |
|--- |--- |--- |--- |
| mbox | 文字列 | ○ | mbox 名 |
| params | オブジェクト | × | mbox パラメーター。次の構造を持つキーと値のペアのオブジェクト。<P>`{ "param1": "value1", "param2": "value2"}` |
| success | 関数 | ○ | サーバーから応答を受け取ると、コールバックが実行されます。success コールバック関数は、オファーオブジェクトの配列を表す単一のパラメーターを受け取ります。次に success コールバックの例を示します。<P>`function handleSuccess(response){......}`<P>詳しくは、以下の「応答」を参照してください。 |
| error | 関数 | ○ | エラーを受け取ると、コールバックが実行されます。エラーと見なされる状況がいくつかあります。<ul><li>HTTP ステータスコードが 200 OK ではない</li><li>応答が解析できない。例えば、脆弱な構造の JSON や、JSON ではなく HTML など。</li><li>応答には「エラー」キーが含まれます。例えば、危険にさらされて例外がスローされ、リクエストが適切に処理されない可能性があります。mbox がブロックされ、そのコンテンツを取得できないなどの場合にエラーが発生する可能性があります。error コールバック関数は、status と error の 2 つのパラメーターを受け取ります。次に error コールバックの例を示します。 `function handleError(status, error){......}`</li></ul>詳しくは、以下の「エラー応答」を参照してください。 |
| timeout | 数値 | × | タイムアウト（ミリ秒）。指定しない場合は、at.js のデフォルトのタイムアウトが使用されます。<P>デフォルトのタイムアウトは、 [!DNL Target] 下の UI [!UICONTROL 管理] > [!UICONTROL 実装]. |

## 例

を使用したパラメーターの追加 [!UICONTROL getOffer()] およびを使用して [!UICONTROL applyOffer()] 成功処理の場合：

```javascript {line-numbers="true"}
adobe.target.getOffer({   
  "mbox": "target-global-mbox", 
  "params": { 
     "a": 1, 
     "b": 2 
  }, 
  "success": function(offer) {           
        adobe.target.applyOffer( {  
           "mbox": "target-global-mbox", 
           "offer": offer  
        } ); 
  },   
  "error": function(status, error) {           
      console.log('Error', status, error); 
  } 
});
```

を使用したパラメーターとプロファイルパラメーターの追加 [!UICONTROL getOffer()] およびを使用して [!UICONTROL applyOffer()] 成功処理の場合：

```javascript {line-numbers="true"}
adobe.target.getOffer({   
  "mbox": "target-global-mbox", 
  "params": { 
     "a": 1, 
     "b": 2, 
     "profile.age": 27, 
     "profile.gender": "male" 
  }, 
  "success": function(offer) {           
        adobe.target.applyOffer( {  
           "mbox": "target-global-mbox", 
           "offer": offer  
        } ); 
  },   
  "error": function(status, error) {           
      console.log('Error', status, error); 
  } 
});
```

でのカスタムタイムアウトとカスタム成功処理の使用 [!UICONTROL getOffer()]:

「YOUR_OWN_CUSTOM_HANDLING_FUNCTION」は、お客様が定義する関数のプレースホルダーです。

```javascript {line-numbers="true"}
adobe.target.getOffer({     
  "mbox": "target-global-mbox",   
  "success": function(offer) { 
    YOUR_OWN_CUSTOM_HANDLING_FUNCTION(offer);   
  }, 
  "error": function(status, error) {                 
    console.log('Error', status, error);   
  },   
  "timeout": 2000 
});
```

## 応答

success コールバックに渡された応答パラメーターはアクションの配列になります。アクションは、通常は次の形式のオブジェクトです。

| 名前 | タイプ | 説明 |
|--- |--- |--- |
| action | 文字列 | 識別された要素に適用されるアクションの種類。 |
| selector | 文字列 | Sizzle セレクターを表します。 |
| cssSelector | 文字列 | 要素を事前に非表示にするために使用される、ネイティブな DOM セレクター。 |
| content | 文字列 | 識別された要素に適用されるコンテンツ。 |

## 例

```javascript {line-numbers="true"}
{ 
    "sessionId": "1444512212156-384616", 
    "tntId": "1444512212156-384616.17_35", 
    "offers": [{ 
        "plugins": ["<script type=\"text/javascript\">\r\n/*mboxHighlight+ (1of2) v1 ==> Response Plugin*/\r\nwindow.ttMETA=(typeof(window.ttMETA)!='undefined')?window.ttMETA:[];window.ttMETA.push({'mbox':'target-global-mbox','campaign':'at: redirect ootb','experience':'Experience B','offer':'/at_redirect_ootb/experiences/1/pages/0/1442082890250'});window.ttMBX=function(x){var mbxList=[];for(i=0;i<ttMETA.length;i++){if(ttMETA[i].mbox==x.getName()){mbxList.push(ttMETA[i])}}return mbxList[x.getId()]}\r\n</script>"], 
        "actions": { 
            "content": [{ 
                "passMboxSession": false, 
                "selector": "body", 
                "action": "redirect", 
                "url": "https://example.com/04.html", 
                "includeAllUrlParameters": true 
            }] 
        } 
    }] 
}
```

## エラー応答

error コールバックに渡される「status」および「error」パラメーターは、次の形式を持ちます。

| 名前 | タイプ | 説明 |
|--- |--- |--- |
| status | 文字列 | エラーの状態を表します。このパラメーターは次の値を持つことができます。<ul><li>timeout：リクエストがタイムアウトしたことを示します。</li><li>parseerror：例えば、JSON ではなく HTML またはプレーンテキストを受け取るなど、応答が解析できなかったことを示します。</li><li>error：200 OK ではない HTTP ステータスを受け取ったなど、一般的なエラーを示します。</li></ul> |
| error | 文字列 | 例外メッセージやその他トラブルシューティングに役立つ可能性のある、追加のデータが含まれています。 |
