---
keywords: 実装，実装，設定，設定，ページパラメーター
description: ペ  [!DNL Target]  ジ内プロファイル属性を使用して、データをに取り込みます。
title: ページ内プロファイル属性を使用して  [!DNL Target]  データをに取り込むにはどうすればよいですか？
feature: Implementation
exl-id: c19fd746-21a2-4eb5-8c2a-c24806e09324
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 43%

---

# ページ内プロファイル属性

[!DNL Adobe Target] のページ内プロファイル属性（「mbox 内プロファイル属性」とも呼ばれます）は、ページコードを介して直接渡される名前と値のペアで、今後の使用のために訪問者のプロファイルに保存されます。

ページ内プロファイル属性を利用すると、ユーザー固有のデータを Target のプロファイルに保管し、以降のターゲティングやセグメント化に利用できます。

## 形式

ページ内プロファイル属性は、サーバーコールによって、プレフィックス「profile」が付いた文字列名/値のペアとして [!DNL Target] に渡されます。 Target に渡されます。

属性の名前と値はカスタマイズできます（ただし、特定用途向けに「予約されている名前」もあります）。

ページ内プロファイル属性の例を次に示します。

* `profile.membershipLevel=silver`
* `profile.visitCount=3`

## 使用例

* **ログイン情報**：ユーザーのログイン情報に基づいて、PII （個人を特定できる情報）以外のデータを [!DNL Target] に共有します。 このデータには、メンバーシップのステータスや注文履歴などがあります。
* **店舗情報**：ユーザーがどの場所の店舗を選んだのかを追跡します。
* **過去のインタラクション**：サイトでのユーザーの過去の行動を追跡し、以降のパーソナライゼーションに生かします。

## メソッドの利点

データはリアルタイムで [!DNL Target] に送信され、データが取り込まれるのと同じサーバーコールで使用できます。

## 注意事項

ページコードの更新が必要です（直接更新するか、タグ管理システムを使用します）。

属性と値はサーバー呼び出しで確認できるので、訪問者は値を確認できます。信用範囲やその他の潜在的に個人的な情報などの情報を共有する場合は、この方法は最適なアプローチではない可能性があります。

## コードの例

targetPageParamsAll（ページのすべての mbox 呼び出しに属性を追加します）：

`function targetPageParamsAll() { return "profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

targetPageParams（ページのグローバル mbox に属性を追加します）：

`function targetPageParams() { return profile.param1=value1&profile.param2=value2&profile.p3=hello%20world"; }`

mboxCreate コードの属性：

`<div class="mboxDefault"> default content to replace by offer </div> <script> mboxCreate('mboxName','profile.param1=value1','profile.param2=value2'); </script>`

## 関連情報へのリンク

[プロファイル属性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html?lang=ja)

[訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html?lang=ja)
