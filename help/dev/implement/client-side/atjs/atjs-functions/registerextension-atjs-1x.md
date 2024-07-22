---
keywords: registerExtension, registerextension，登録拡張，at.js，関数，clientCode, serverDomain, globalMboxName, globalMboxAutoCreate, timeout, registerExtension2
description: 特定の拡張機能を登録するには、at [!DNL Adobe Target] js JavaScript ライブラリの [!UICONTROL registerExtension()] 関数を使用します。 （at.js 1.x）
title: '[!UICONTROL registerExtension()] 関数の使用方法'
feature: at.js
exl-id: 71decf00-84c5-4914-b0cd-bb061fa6265f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '265'
ht-degree: 66%

---

# [!UICONTROL registerExtension()] - at.js 1.x

特定の拡張を登録するための標準的な方法を提供します。

>[!NOTE]
>
>この関数は、at.js バージョン 1 で使用できます。*x* のみで使用できます。この関数は、at.js 2 のリリースで廃止されました。*x* を通じてクロスドメイントラッキングを使用している場合です。at.js 2.x で使用した場合、この関数はデフォルトコンテンツを返します。

options パラメーターは必須で、以下の構造を持ちます。

| キー | タイプ | 必須 | 説明 |
|--- |--- |--- |--- |
| name | 文字列 | ○ | 拡張名です。 |
| モジュール | Array[String] | ○ | リクエストしたモジュール名を示す文字列の配列です。 |
| 登録 | 関数 | ○ | 拡張の初期化および構築に使用される関数です。この関数では、モジュール配列に基づいた引数を受け取ります。 |

メモ:

* パラメーターのいずれかが指定されていない場合は、例外が発生します。
* モジュール配列が空白の場合は、例外が発生します。

`[!UICONTROL registerExtension]` の使用方法の詳細と例については、GitHub の [Adobe Experience Cloud Target atjs Extensions](https://github.com/Adobe-Marketing-Cloud/target-atjs-extensions) ページを参照してください。

## モジュールメソッドの設定

| キー | タイプ | 説明 |
|--- |--- |--- |
| clientCode | 文字列 | クライアントコード |
| serverDomain | 文字列 | Edge サーバードメイン |
| globalMboxName | 文字列 | グロ [!DNL Target] バル mbox 名 |
| globalMboxAutoCreate | ブール値 | 自動作成が有効化されているかどうかを示します。 |
| timeout | 数値 | リクエストのタイムアウト |

## Logger Module メソッド

| キー | タイプ | 説明 |
|--- |--- |--- |
| log | 関数 | ブラウザーコンソールに対する引数の変数リストを記録します（存在する場合）。`mboxDebug=true` が URL に渡される場合にのみアクティブ化されます。 |
| error | 関数 | ブラウザーコンソールに対する引数の変数リストを記録します。ネットワークのタイムアウトや、HTML ノードが見つからない場合など、重大なエラーが発生した場合にのみアクティブ化されます。 |
