---
keywords: mboxCreate, mboxcreate, mbox create, at.js，関数，関数
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリの[!UICONTROL mboxCreate （） ]関数を使用して、mboxDefault クラス名を持つ最も近いDIVにオファーを適用します。 （at.js 1.x）'
title: '[!UICONTROL mboxCreate （） ]関数の使用方法を教えてください。'
feature: at.js
exl-id: 86eba1fc-4e1d-4793-94e7-898bf81f8945
TQID: https://experienceleague.adobe.com/hCEKL9RPtqIbMVEouzObjU6dc7TKl1hBtKZ1jEdicRE
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: a1af9d2c909d9b3d506dd4875d1bd75149dbf636
workflow-type: tm+mt
source-wordcount: 216
ht-degree: 40%

---

# [!UICONTROL mboxCreate （mbox,params） ] - at.js 1.x

リクエストを実行し、mboxDefault クラス名を持つ最も近い DIV にオファーを適用します。

>[!NOTE]
>
>この関数は、at.js バージョン 1.*x*&#x200B;でのみ使用できます。 この関数は、at.js 2.xのリリースで廃止されました。 この関数は、at.js 2.xで使用した場合、デフォルトコンテンツを返します。

この関数は、主にmbox.js （現在は非推奨）からat.jsへの移行を容易にするためにat.jsに組み込まれています。 新しく `[!UICONTROL mboxCreate()]` に代わるものは、`[!UICONTROL adobe.target.applyOffer()]`/`[!UICONTROL adobe.target.getOffer()]` または Angular ディレクティブです。

## 例

```javascript {line-numbers="true"}
<div class="mboxDefault"> 
  default content to replace by offer 
</div> 
<script> 
  mboxCreate('mboxName','param1=value1','param2=value2'); 
</script>
```

## メモ

`mboxCreate()` は、「標準」エンドポイントではなく「json」エンドポイントを使用し、非同期で実行されます。 理由は以下のとおりです。

* [デバッグ](/help/dev/implement/client-side/target-debugging-atjs/target-debugging-atjs.md) が少し異なる。
* オファーコードが同期を必要としたり、呼び出しをブロックするのを避ける。

  例えば、サイトコードまたは後でページに表示される他の mbox によって使用される JavaScript 変数を設定するオファー。

* `[!UICONTROL mboxCreate()]`を呼び出す前に、`<div class="mboxDefault"></div>`を必ず用意してください。at.jsでは追加されないからです。

* 空の、ページの先頭にある `[!UICONTROL mboxCreate()]` 関数は、グローバル mbox としてお勧めしません。

  at.jsの自動作成グローバル mboxは、`<head>`から起動し、コンテンツを早く返すことができるため、より優れたオプションです。



