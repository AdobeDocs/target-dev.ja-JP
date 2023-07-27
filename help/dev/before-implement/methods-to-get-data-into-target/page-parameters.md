---
keywords: 実装，実装，設定，設定，ページパラメーター
description: データをに取り込む [!DNL Target] ページパラメーターを使用します。
title: データをに取り込む方法 [!DNL Target] ページパラメーターを使用している場合
feature: Implementation
exl-id: 9bb7157e-a938-4150-8a15-c9bf0a0e2296
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '394'
ht-degree: 38%

---

# ページのパラメーター

ページパラメーター（「mbox パラメーター」とも呼ばれます）は、ページコードを介して直接渡される名前と値のペアで、今後の使用のために訪問者のプロファイルに保存されません。

ページパラメーターは、ページデータをに送信するのに役立ちます。 [!DNL Adobe Target] 今後のターゲティングで使用するために、訪問者のプロファイルと共に保存する必要はありません。 これらの値は、ページまたはユーザーが特定のページでおこなったアクションの記述に使用されます。

## 形式

ページパラメーターが [!DNL Target] 文字列の名前と値のペアとしてのサーバー呼び出しを介して渡す。 パラメーターの名前と値はカスタマイズできます（ただし、特定用途向けに「予約されている名前」もあります）。

次に、ページパラメーターの例を示します。

* `page=productPage`

* `categoryId=homeLoans`

## 使用例

* **製品ページ**：閲覧された特定の製品に関する情報を送信します ( このメソッドはRecommendationsの仕組みです )
* **注文の詳細**：注文データの収集のために注文 ID、orderTotal などを送信します。
* **カテゴリ親和性**[!DNL Target]：特定のサイトカテゴリに対するユーザーの親和性に関するデータを構築するために、カテゴリの閲覧情報を に送信します。
* **サードパーティデータ**：天気ターゲティングプロバイダー、アカウントデータ（例：DemandBase）、デモグラフィックデータ（例：Experian）など、サードパーティのデータソースからの情報を送信します。

## 方法の利点

データの送信先 [!DNL Target] リアルタイムで、およびを同じサーバーで使用して、同じサーバーが受け取るデータを呼び出すことができます。

## 注意事項

* ページコードの更新が必要です（直接更新するか、タグ管理システムを使用します）。
* 後続のページまたはサーバー呼び出しでターゲティングにデータを使用する必要がある場合は、データをプロファイルスクリプトに変換する必要があります。
* [Internet Engineering Task Force（IETF）標準](https://www.ietf.org/rfc/rfc3986.txt)では、クエリ文字列に文字のみを含めることができます。

  IETF のサイトで言及されている文字に加えて、 [!DNL Target] では、クエリ文字列に次の文字を使用できます。

  ```< > # % " { } | \ ^ [ ] ` ``` {line-numbers=&quot;true&quot;}

  これ以外の文字はすべて URL エンコードする必要があります。標準では、次のフォーマット ( [https://www.ietf.org/rfc/rfc1738.txt](https://www.ietf.org/rfc/rfc1738.txt) ) です。以下の図を参照してください。

  ![代替画像](assets/ietf1.png)

  または、簡略化した完全なリスト：

  ![代替画像](assets/ietf2.png)

## コードの例

targetPageParamsAll（ページのすべての mbox 呼び出しにパラメーターを追加します）：

`function targetPageParamsAll() { return "param1=value1&param2=value2&p3=hello%20world";`

targetPageParams（ページのグローバル mbox にパラメーターを追加します）：

`function targetPageParams() { return "param1=value1&param2=value2&p3=hello%20world";`

## 関連情報へのリンク

Recommendations：[ページタイプに従った実装](https://experienceleague.adobe.com/docs/target/using/recommendations/plan-implement.html)

注文の確認：[コンバージョンの追跡](../../implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)

カテゴリ親和性：[カテゴリ親和性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/category-affinity.html)
