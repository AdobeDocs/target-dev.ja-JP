---
keywords: 実装，実装，設定，設定，ページパラメーター
description: ページパラメーターを使用して  [!DNL Target]  データをに取り込みます。
title: ページパラメーターを使用してデータをに取  [!DNL Target]  込むにはどうすればよいですか？
feature: Implementation
exl-id: 9bb7157e-a938-4150-8a15-c9bf0a0e2296
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 32%

---

# ページのパラメーター

ページパラメーター（「mbox パラメーター」とも呼ばれます）は、ページコードを介して直接渡される名前と値のペアで、今後の使用のために訪問者のプロファイルに保存されることはありません。

ページパラメーターは、今後のターゲティングの使用のために訪問者のプロファイルと共に保存する必要がない [!DNL Adobe Target] ーザーにページデータを送信する場合に役立ちます。 これらの値は、ページまたはユーザーが特定のページでおこなったアクションの記述に使用されます。

## 形式

ページパラメーターは、サーバー呼び出しを介して、文字列名と値のペアとして [!DNL Target] に渡されます。 パラメーターの名前と値はカスタマイズできます（ただし、特定用途向けに「予約されている名前」もあります）。

ページパラメーターの例をいくつか示します

* `page=productPage`

* `categoryId=homeLoans`

## 使用例

* **商品ページ**：閲覧された特定の商品に関する情報を送信します（Recommendationsの仕組みです）
* **注文詳細**：注文収集のために注文 ID、orderTotal などを送信します
* **カテゴリの親和性**：カテゴリで閲覧した情報を [!DNL Target] に送信して、特定のサイトカテゴリに対するユーザーの親和性に関する知識を構築します
* **サードパーティデータ**：天気ターゲティングプロバイダー、アカウントデータ（例：DemandBase）、デモグラフィックデータ（例：Experian）など、サードパーティのデータソースからの情報を送信します。

## メソッドの利点

データはリアルタイムで [!DNL Target] に送信され、受信したデータと同じサーバーコールで使用できます。

## 注意事項

* ページコードの更新が必要です（直接更新するか、タグ管理システムを使用します）。
* 後続のページ/サーバーコールでターゲティングにデータを使用する必要がある場合は、プロファイルスクリプトに変換する必要があります。
* クエリ文字列には、[ インターネット技術標準化委員会（IETF）標準 ](https://www.ietf.org/rfc/rfc3986.txt) に従った文字のみを含めることができます。

  IETF サイトで言及されている文字に加えて、[!DNL Target] では次の文字をクエリ文字列で使用できます。

  ```< > # % " { } | \ ^ [ ] ` ``` {line-numbers=&quot;true&quot;}

  これ以外の文字はすべて URL エンコードする必要があります。標準では、次に示すように、次の形式（[https://www.ietf.org/rfc/rfc1738.txt](https://www.ietf.org/rfc/rfc1738.txt)）を指定します。

  ![alt 画像 ](assets/ietf1.png)

  または、簡略化した完全なリスト：

  ![alt 画像 ](assets/ietf2.png)

## コードの例

targetPageParamsAll（ページのすべての mbox 呼び出しにパラメーターを追加します）：

`function targetPageParamsAll() { return "param1=value1&param2=value2&p3=hello%20world";`

targetPageParams（ページのグローバル mbox にパラメーターを追加します）：

`function targetPageParams() { return "param1=value1&param2=value2&p3=hello%20world";`

## 関連情報へのリンク

Recommendations：[ページタイプに従った実装](https://experienceleague.adobe.com/docs/target/using/recommendations/plan-implement.html)

注文の確認：[コンバージョンの追跡](../../implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)

カテゴリ親和性：[カテゴリ親和性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/category-affinity.html)
