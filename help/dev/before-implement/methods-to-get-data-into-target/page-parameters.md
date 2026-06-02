---
keywords: 実装、実装、設定、設定、ページパラメーター
description: ページパラメーターを使用して [!DNL Target] にデータを取り込みます。
title: ページパラメーターを使用して [!DNL Target] にデータを取り込むにはどうすればよいですか？
feature: Implementation
exl-id: 9bb7157e-a938-4150-8a15-c9bf0a0e2296
TQID: https://experienceleague.adobe.com/CYhZOFnli-DmREOOZGE2aGNn3x7BJ7uwGA2vfwUSnOk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: f6df325aff4a2eb9321b86778d102737493e63bb
workflow-type: tm+mt
source-wordcount: 397
ht-degree: 31%

---

# ページのパラメーター

ページパラメーター（「mbox パラメーター」とも呼ばれます）は、ページコードを通じて直接渡される名前と値のペアで、後で使用するために訪問者のプロファイルに保存されません。

ページパラメーターは、今後ターゲティングに使用するために訪問者のプロファイルと一緒に保存する必要がない[!DNL Adobe Target]にページデータを送信するのに便利です。 これらの値は、ページまたはユーザーが特定のページでおこなったアクションの記述に使用されます。

## 形式

ページパラメーターは、文字列名と値のペアとしてサーバーコールを介して[!DNL Target]に渡されます。 パラメーターの名前と値はカスタマイズできます（ただし、特定用途向けに「予約されている名前」もあります）。

ページパラメーターの例をいくつか紹介します

* `page=productPage`

* `categoryId=homeLoans`

## 使用例

* **製品ページ**：表示された特定の製品に関する情報を送信します（この方法は、Recommendationsの仕組みです）
* **注文の詳細**：注文の収集のために、注文IDやorderTotalなどを送信します
* **カテゴリの親和性**: カテゴリで表示された情報を[!DNL Target]に送信して、特定のサイトカテゴリに対するユーザーの親和性に関する知識を構築します
* **サードパーティデータ**：天気ターゲティングプロバイダー、アカウントデータ（例：DemandBase）、デモグラフィックデータ（例：Experian）など、サードパーティのデータソースからの情報を送信します。

## メソッドの利点

データはリアルタイムで[!DNL Target]に送信され、データを取り込んだ同じサーバー呼び出しで使用できます。

## 注意事項

* ページコードの更新が必要です（直接更新するか、タグ管理システムを使用します）。
* 後続のページ/サーバー呼び出しのターゲティングにデータを使用する必要がある場合は、プロファイルスクリプトに変換する必要があります。
* クエリ文字列には、[Internet Engineering Task Force （IETF）標準](https://www.ietf.org/rfc/rfc3986.txt)に従って文字のみを含めることができます。

  IETF サイトで言及されている文字に加えて、[!DNL Target]では、クエリ文字列に次の文字を使用できます。

  `< > # % " { } | \ ^ [ ] `

  これ以外の文字はすべて URL エンコードする必要があります。 標準では、以下に示すように、次の形式（[https://www.ietf.org/rfc/rfc1738.txt](https://www.ietf.org/rfc/rfc1738.txt)）を指定します。

  ![alt画像](assets/ietf1.png)

  または、簡略化した完全なリスト：

  ![alt画像](assets/ietf2.png)

## コード例

targetPageParamsAll（ページのすべての mbox 呼び出しにパラメーターを追加します）：

`function targetPageParamsAll() { return "param1=value1&param2=value2&p3=hello%20world";`

targetPageParams（ページのグローバル mbox にパラメーターを追加します）：

`function targetPageParams() { return "param1=value1&param2=value2&p3=hello%20world";`

## 関連情報へのリンク

レコメンデーション：[ページタイプに従った実装](https://experienceleague.adobe.com/docs/target/using/recommendations/plan-implement.html)

注文の確認：[コンバージョンの追跡](../../implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)

カテゴリ親和性：[カテゴリ親和性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/category-affinity.html)
