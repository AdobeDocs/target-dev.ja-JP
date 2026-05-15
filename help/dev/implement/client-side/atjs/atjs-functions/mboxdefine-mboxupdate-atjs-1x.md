---
keywords: mboxDefine、mboxdefine、mbox define、mboxUpdate、mboxupdate、mbox update、at.js、functions、function、mboxDefine0
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリの[!UICONTROL mboxDefine()]関数と[!UICONTROL mboxUpdate()]関数を使用して、mboxを定義または更新します。 （at.js 1.x）'
title: '[!UICONTROL mboxDefine()]および[!UICONTROL mboxUpdate()]関数の使用方法を教えてください。'
feature: at.js
exl-id: 0a7dbea2-1cbd-4a5b-ba68-4c76a88d65c4
TQID: https://experienceleague.adobe.com/Fn-Ej8jk2AMEn79tOtRoP9GQc36Ugy6FtXyn6x7jkmA
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 202
ht-degree: 47%

---

# [!UICONTROL mboxDefine()]および[!UICONTROL mboxUpdate()] - at.js 1.x

[!DNL Adobe Target]でmboxを定義および更新します。

>[!NOTE]
>
>これらの関数は、at.js バージョン 1.*x*&#x200B;でのみ使用できます。 これらの関数は、at.js 2.*x*&#x200B;のリリースで廃止されました。 これらの関数は、at.js 2.*x*&#x200B;で使用した場合、デフォルトコンテンツを返します。

`[!UICONTROL mboxDefine()]` と `[!UICONTROL mboxCreate()]` は、オファーが表示される HTML DIV 要素に結び付けられています。 これらの HTML DIV 要素は、`mboxDefault` クラスを持つ必要があります。 HTML 要素にこのクラスが付加されない場合、顕著なちらつきが表示される可能性があります。

## mboxDefine

nodeId と mbox 名の間の内部マッピングを作成しますが、リクエストを実行しません。 `[!UICONTROL mboxUpdate()]` () と共に使用されます。 at.jsに組み込まれているのは、主にmbox.js （現在は非推奨）からat.jsへの移行を容易にするためです。

## mboxUpdate

リクエストを実行し、`nodeId` () の `mboxDefine()` によって識別される要素にオファーを適用します。 また、`[!UICONTROL mboxCreate]` によって開始された mbox を更新するために使用できます。 at.jsに組み込まれているのは、主にmbox.js （現在は非推奨）からat.jsへの移行を容易にするためです。 `mboxDefine()`／`mboxUpdate()` は、セレクターオプションを使用する [adobe.target.getOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) および [adobe.target.applyOffer()](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md) に置き換えられます。

## 例

```javascript {line-numbers="true"}
<div id="someId" class="mboxDefault"></div> 
<script> 
 mboxDefine('someId','mboxName','param1=value1','param2=value2'); 
 mboxUpdate('mboxName','param3=value3','param4=value4'); 
</script>
```
