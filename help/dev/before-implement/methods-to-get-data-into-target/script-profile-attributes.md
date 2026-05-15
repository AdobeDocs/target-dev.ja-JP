---
keywords: 実装、実装、設定、設定、スクリプトプロファイル属性
description: スクリプト プロファイル属性を使用して [!DNL Target] にデータを取り込みます。
title: スクリプト プロファイル属性を使用して [!DNL Target] にデータを取り込むにはどうすればよいですか？
feature: Implementation
exl-id: ba11f1de-e68b-4505-8e3e-cd4d46ef59a2
TQID: https://experienceleague.adobe.com/bRsl4ipgw0OeuVD0d69wmnHmrVvcGt9o0KmkhrEECZQ
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 292
ht-degree: 71%

---

# スクリプトプロファイル属性

スクリプト プロファイル属性は、[!DNL Adobe Target] ソリューションで定義された名前と値のペアです。 値は、サーバー呼び出しごとに、Target サーバーで JavaScript スニペットが実行されることで決まります。

訪問者がオーディエンスやアクティビティメンバーシップの条件を満たしているかが評価される前に、ユーザーは、mbox 呼び出しごとに実行する簡単なコードスニペットを記述します。

## 形式

スクリプトプロファイル属性は、Target の「オーディエンス」セクションで作成します。 任意の属性名は有効で、値は[!DNL Target] ユーザーが記述したJavaScript関数の結果です。 Target でページ内プロファイル属性と区別するために、属性名の接頭辞「user. [!DNL Target]の「」を使用して、ページ内のプロファイル属性と区別します。

コードスニペットは Rhino JS 言語で記述し、トークンやその他の値を参照できます。

## 使用例

* **買い物かごの放棄**：訪問者が買い物かごにアクセスしたときに、プロファイルスクリプトを 1 に設定します。 訪問者がコンバージョンに至ったら 0 にリセットします。 この値が 1 の訪問者は、買い物かごに商品が入っていることになります。
* **訪問数**：新たな訪問のたびに 1 ずつ加算し、訪問者のサイト再訪問頻度を追跡します。

## メソッドの利点

ページコードの更新が不要です。

オーディエンスとアクティビティメンバーシップの判断の前に実行し、単一のサーバー呼び出しでこれらのプロファイルスクリプト属性でメンバーシップに影響を与えることができます。

非常に強力です。 スクリプトごとに 2,000 個の命令を実行できます。

## 注意事項

JavaScript に関する知識が必要です。

プロファイルスクリプトの実行の順番は保証されているわけではないので、相互に依存することはできません。

デバッグが困難な場合があります。

## コード例

プロファイルスクリプトは非常に柔軟です。

```
user.purchase_recency: var dayInMillis = 3600 * 24 * 1000; if (mbox.name == 'orderThankyouPage') {  user.setLocal('lastPurchaseTime', new Date().getTime()); } var lastPurchaseTime = user.getLocal('lastPurchaseTime'); if (lastPurchaseTime) {  return ((new Date()).getTime()-lastPurchaseTime)/dayInMillis; }
```

### 関連情報へのリンク

[プロファイルスクリプト属性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html#concept_8C07AEAB0A144FECA8B4FEB091AED4D2)
