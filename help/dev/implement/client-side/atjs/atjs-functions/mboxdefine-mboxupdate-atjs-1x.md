---
keywords: mboxDefine, mboxdefine, mbox define, mboxUpdate, mboxupdate, mbox update, at.js，関数，function, mboxDefine0
description: at.js JavaScript ライブラリの [!UICONTROL mboxDefine()] 関数と [!UICONTROL mboxUpdate()] 関数を使用して  [!DNL Adobe Target] mbox を定義または更新します。 （at.js 1.x）
title: '[!UICONTROL mboxDefine()] 関数と [!UICONTROL mboxUpdate()] 関数の使用方法'
feature: at.js
exl-id: 0a7dbea2-1cbd-4a5b-ba68-4c76a88d65c4
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '201'
ht-degree: 49%

---

# [!UICONTROL mboxDefine()] と [!UICONTROL mboxUpdate()] - at.js 1.x

[!DNL Adobe Target] で mbox を定義および更新します。

>[!NOTE]
>
>これらの関数は、at.js バージョン 1 で使用できます。*x* のみで使用できます。これらの関数は、at.js 2 のリリースで非推奨（廃止予定）となりました。*x* を通じてクロスドメイントラッキングを使用している場合です。これらの関数は、at.js 2 で使用された場合、デフォルトのコンテンツを返します。**。

`[!UICONTROL mboxDefine()]` と `[!UICONTROL mboxCreate()]` は、オファーが表示される HTML DIV 要素に結び付けられています。これらの HTML DIV 要素は、`mboxDefault` クラスを持つ必要があります。HTML 要素にこのクラスが付加されない場合、顕著なちらつきが表示される可能性があります。

## mboxDefine

nodeId と mbox 名の間の内部マッピングを作成しますが、リクエストを実行しません。`[!UICONTROL mboxUpdate()]` () と共に使用されます。at.js に組み込まれているほとんどが、mbox.js （現在は非推奨）から at.js への移行を容易にするためのものです。

## mboxUpdate

リクエストを実行し、`nodeId` () の `mboxDefine()` によって識別される要素にオファーを適用します。また、`[!UICONTROL mboxCreate]` によって開始された mbox を更新するために使用できます。at.js に組み込まれているほとんどが、mbox.js （現在は非推奨）から at.js への移行を容易にするためのものです。 `mboxDefine()`／`mboxUpdate()` は、セレクターオプションを使用する [adobe.target.getOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) および [adobe.target.applyOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md) に置き換えられます。

## 例

```javascript {line-numbers="true"}
<div id="someId" class="mboxDefault"></div> 
<script> 
 mboxDefine('someId','mboxName','param1=value1','param2=value2'); 
 mboxUpdate('mboxName','param3=value3','param4=value4'); 
</script>
```
