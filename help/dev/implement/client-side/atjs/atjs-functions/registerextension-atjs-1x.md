---
keywords: registerExtension, registerextension, register extension, at.js, functions, function, clientCode, serverDomain, globalMboxName, globalMboxAutoCreate, timeout, registerExtension2
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリの[!UICONTROL registerExtension （） &#x200B;]関数を使用して、特定の拡張機能を登録します。 （at.js 1.x）'
title: '[!UICONTROL registerExtension （） &#x200B;]関数の使用方法を教えてください。'
feature: at.js
exl-id: 71decf00-84c5-4914-b0cd-bb061fa6265f
TQID: https://experienceleague.adobe.com/qTWubp0dNesN-8vsooz8pdbjfSw1W1ktm-0bG6YRzJw
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
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 277
ht-degree: 62%

---

# [!UICONTROL registerExtension （） &#x200B;] - at.js 1.x

特定の拡張を登録するための標準的な方法を提供します。

>[!NOTE]
>
>この関数は、at.js バージョン 1.*x*&#x200B;でのみ使用できます。 この関数は、at.js 2.*x*&#x200B;のリリースで廃止されました。 この関数は、at.js 2.xで使用した場合、デフォルトコンテンツを返します。

options パラメーターは必須で、以下の構造を持ちます。

| キー | タイプ | 必須 | 説明 |
|--- |--- |--- |--- |
| name | 文字列 | ○ | 拡張名です。 |
| モジュール | Array[String] | ○ | リクエストしたモジュール名を示す文字列の配列です。 |
| 登録 | 関数 | ○ | 拡張の初期化および構築に使用される関数です。 この関数では、モジュール配列に基づいた引数を受け取ります。 |

メモ:

* パラメーターのいずれかが指定されていない場合は、例外が発生します。
* モジュール配列が空白の場合は、例外が発生します。

`[!UICONTROL registerExtension]`の使用方法の詳細と例については、GitHubの[Adobe Experience Cloud Target拡張機能](https://github.com/Adobe-Marketing-Cloud/target-atjs-extensions) ページを参照してください。

## モジュールメソッドの設定

| キー | タイプ | 説明 |
|--- |--- |--- |
| clientCode | 文字列 | クライアントコード |
| serverDomain | 文字列 | Edge サーバードメイン |
| globalMboxName | 文字列 | [!DNL Target] グローバル mbox名 |
| globalMboxAutoCreate | ブール値 | 自動作成が有効化されているかどうかを示します。 |
| timeout | 数値 | リクエストのタイムアウト |

## Logger Module メソッド

| キー | タイプ | 説明 |
|--- |--- |--- |
| log | 関数 | ブラウザーコンソールに対する引数の変数リストを記録します（存在する場合）。 `mboxDebug=true` が URL に渡される場合にのみアクティブ化されます。 |
| error | 関数 | ブラウザーコンソールに対する引数の変数リストを記録します。 ネットワークのタイムアウトや、HTML ノードが見つからない場合など、重大なエラーが発生した場合にのみアクティブ化されます。 |
