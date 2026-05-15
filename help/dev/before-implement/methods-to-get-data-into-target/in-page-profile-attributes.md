---
keywords: 実装、実装、設定、設定、ページパラメーター
description: ページ内プロファイル属性を使用して [!DNL Target] にデータを取り込みます。
title: ページ内プロファイル属性を使用して [!DNL Target] にデータを取り込むにはどうすればよいですか？
feature: Implementation
exl-id: c19fd746-21a2-4eb5-8c2a-c24806e09324
TQID: https://experienceleague.adobe.com/jXWNNl7HfrR03tEoMz7KApX3onc1Zc44IAuXy4QF2tU
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 311
ht-degree: 40%

---

# ページ内プロファイル属性

[!DNL Adobe Target]のページ内プロファイル属性（「mbox内プロファイル属性」とも呼ばれます）は、後で使用するために訪問者のプロファイルに保存されるページコードを通じて直接渡される名前と値のペアです。

ページ内プロファイル属性を利用すると、ユーザー固有のデータを Target のプロファイルに保管し、以降のターゲティングやセグメント化に利用できます。

## 形式

ページ内プロファイル属性は、プレフィックス「profile」を持つ文字列名と値のペアとしてサーバーコールを介して[!DNL Target]に渡されます。 Target に渡されます。

属性の名前と値はカスタマイズできます（ただし、特定用途向けに「予約されている名前」もあります）。

ここでは、ページ内のプロファイル属性の例をいくつか紹介します。

* `profile.membershipLevel=silver`
* `profile.visitCount=3`

## 使用例

* **ログイン情報**：ユーザーのログイン情報に基づいて、PII （個人を特定できる情報）以外のデータを[!DNL Target]に共有します。 こうしたデータには、メンバーシップのステータスや注文履歴などが含まれます。
* **店舗情報**：ユーザーがどの場所の店舗を選んだのかを追跡します。
* **過去のインタラクション**：サイトでのユーザーの過去の行動を追跡し、以降のパーソナライゼーションに生かします。

## メソッドの利点

データはリアルタイムで[!DNL Target]に送信され、データを取り込むサーバーコールと同じサーバーコールで使用できます。

## 注意事項

ページコードの更新が必要です（直接更新するか、タグ管理システムを使用します）。

属性と値はサーバー呼び出しで確認できるので、訪問者は値を確認できます。 クレジット範囲やその他の個人情報の可能性がある情報を共有する場合、この方法は最適なアプローチではない可能性があります。

## コード例

targetPageParamsAll（ページのすべての mbox 呼び出しに属性を追加します）：

`function targetPageParamsAll() { return "profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

targetPageParams（ページのグローバル mbox に属性を追加します）：

`function targetPageParams() { return profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

mboxCreate コードの属性：

`<div class="mboxDefault"> default content to replace by offer </div> <script> mboxCreate('mboxName','profile.param1=value1','profile.param2=value2'); </script>`

## 関連情報へのリンク

[プロファイル属性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html?lang=ja)

[訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=ja)
