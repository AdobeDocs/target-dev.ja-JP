---
keywords: mboxCreate, mboxcreate, mbox create, at.js，関数
description: 以下を使用します。 [!UICONTROL mboxCreate()] 関数 [!DNL Adobe Target] at.js JavaScript ライブラリを使用して、mboxDefault クラス名を持つ最も近い DIV にオファーを適用できます。 (at.js 1.x)
title: 使用方法 [!UICONTROL mboxCreate()] 機能？
feature: at.js
exl-id: 86eba1fc-4e1d-4793-94e7-898bf81f8945
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 55%

---

# [!UICONTROL mboxCreate(mbox,params)] - at.js 1.x

リクエストを実行し、mboxDefault クラス名を持つ最も近い DIV にオファーを適用します。

>[!NOTE]
>
>この関数は at.js バージョン 1.*x* のみで使用できます。この関数は at.js 2.x のリリースで廃止されました。at.js 2.x で使用する場合、この関数はデフォルトコンテンツを返します。

この関数は、mbox.js（現在は非推奨）から at.js への移行の多くを簡単にするために、at.js に組み込まれています。 新しく `[!UICONTROL mboxCreate()]` に代わるものは、`[!UICONTROL adobe.target.applyOffer()]`/`[!UICONTROL adobe.target.getOffer()]` または Angular ディレクティブです。

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

`mboxCreate()` は、「標準」エンドポイントではなく「json」エンドポイントを使用し、非同期で実行されます。理由は以下のとおりです。

* [デバッグ](/help/dev/implement/client-side/target-debugging-atjs/target-debugging-atjs.md) が少し異なる。
* オファーコードが同期を必要としたり、呼び出しをブロックするのを避ける。

  例えば、サイトコードまたは後でページに表示される他の mbox によって使用される JavaScript 変数を設定するオファー。

* 必ず `<div class="mboxDefault"></div>`呼び出し前 `[!UICONTROL mboxCreate()]`at.js によって追加されないので、できません。

* 空の、ページの先頭にある `[!UICONTROL mboxCreate()]` 関数は、グローバル mbox としてお勧めしません。

  at.js で自動作成されたグローバル mbox は、 `<head>` およびは、より早くコンテンツを返すことができます。
