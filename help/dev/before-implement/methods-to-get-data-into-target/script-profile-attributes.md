---
keywords: 実装，実装，設定，設定，スクリプトプロファイル属性
description: データをに取り込む [!DNL Target] スクリプトプロファイル属性を使用する。
title: データをに取り込む方法 [!DNL Target] script プロファイル属性を使用する場合
feature: Implementation
exl-id: ba11f1de-e68b-4505-8e3e-cd4d46ef59a2
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '294'
ht-degree: 69%

---

# スクリプトプロファイル属性

スクリプトプロファイル属性は、 [!DNL Adobe Target] 解決策。 値は、サーバー呼び出しごとに、Target サーバーで JavaScript スニペットが実行されることで決まります。

訪問者がオーディエンスやアクティビティメンバーシップの条件を満たしているかが評価される前に、ユーザーは、mbox 呼び出しごとに実行する簡単なコードスニペットを記述します。

## 形式

スクリプトプロファイル属性は、Target の「オーディエンス」セクションで作成します。任意の属性名が有効で、値は [!DNL Target] ユーザー。 Target でページ内プロファイル属性と区別するために、属性名の先頭には「user.」内 [!DNL Target] を使用して、ページ内プロファイル属性と区別します。

コードスニペットは Rhino JS 言語で記述し、トークンやその他の値を参照できます。

## 使用例

* **買い物かごの放棄**：訪問者が買い物かごにアクセスしたときに、プロファイルスクリプトを 1 に設定します。訪問者がコンバージョンに至ったら 0 にリセットします。この値が 1 の訪問者は、買い物かごに商品が入っていることになります。
* **訪問数**：新たな訪問のたびに 1 ずつ加算し、訪問者のサイト再訪問頻度を追跡します。

## 方法の利点

ページコードの更新が不要です。

オーディエンスとアクティビティメンバーシップの判断の前に実行し、単一のサーバー呼び出しでこれらのプロファイルスクリプト属性でメンバーシップに影響を与えることができます。

非常に強力です。スクリプトごとに 2,000 個の命令を実行できます。

## 注意事項

JavaScript に関する知識が必要です。

プロファイルスクリプトの実行の順番は保証されているわけではないので、相互に依存することはできません。

デバッグが困難な場合があります。

## コードの例

プロファイルスクリプトは非常に柔軟です。

```
user.purchase_recency: var dayInMillis = 3600 * 24 * 1000; if (mbox.name == 'orderThankyouPage') {  user.setLocal('lastPurchaseTime', new Date().getTime()); } var lastPurchaseTime = user.getLocal('lastPurchaseTime'); if (lastPurchaseTime) {  return ((new Date()).getTime()-lastPurchaseTime)/dayInMillis; }
```

### 関連情報へのリンク

[プロファイルスクリプト属性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html#concept_8C07AEAB0A144FECA8B4FEB091AED4D2)
