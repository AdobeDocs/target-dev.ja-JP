---
keywords: トラブルシューティング, よくある質問, FAQ, グローバル, グローバル mbox
description: よくある質問 (FAQ) と回答を読むAdobe [!DNL Target] グローバル mbox。
title: グローバル mbox に関するよくある質問は何ですか。
feature: at.js
exl-id: 7bcd1b67-809a-466a-b648-6e0e44386157
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '308'
ht-degree: 47%

---

# グローバル mbox に関するよくある質問

グローバル mbox に関するよくある質問（FAQ）のリストです。

## 複数のグローバル mbox を使用できる ( [!DNL Target] アカウントが複数のドメインにまたがって設定されているか。

アカウント全体で使用できるグローバル mbox は 1 つだけです。

アクティビティに URL ルールを追加することで、アクティビティの実行場所を制限できます。詳しくは、[類似のページに同じエクスペリエンスを組み込む](https://experienceleague.adobe.com/docs/target/using/experiences/vec/temtest.html)を参照してください。

また、 [targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) 次に、 [!UICONTROL Visual Experience Composer] (VEC) またはパラメーターを「絞り込み条件」として [!UICONTROL フォームベースの Experience Composer].

## 売上高データを [!DNL Target] グローバル mbox?

target-global-mbox の売上高と注文情報を収集するには、「mbox パラメーター」をに送信する必要があります。 [!DNL Target]. これらのパラメーターは、名前と値のペアで、に詳細を送信するために使用されます。 [!DNL Target]. [!DNL Target] はこれらのパラメーター（予約された名前）を自動的に検索し、売上高データを設定します。

`orderConfirmPage` の場合、`orderTotal`、`orderId`、および `productPurchasedId` を渡す必要があります。

これらのパラメーターは、 `targetPageParams()`. 詳しくは、「[グローバル mbox にパラメーターを渡す](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md)」を参照してください。

また、コンバージョンにターゲティングを追加して、 [!DNL Target] は、次に示すように、注文確認ページが表示された場合にのみ target-global-mbox のコンバージョンとしてカウントします。

![代替画像](assets/revenue1.png)

上図の「サイトのページ」セクションの選択内容は、「現在のページ」、「URL」、「次を含む」、「orderconfirm」となっています。

![代替画像](assets/revenue2.png)

上図のオプションは、以下の設定で構成されています。

* **このアクティビティでは何を測定しますか？：**&#x200B;売上高
* **レポートのデフォルトの表示：**&#x200B;訪問者あたりの売上高（RPV）
* **目標を達成したことを示すオーディエンスのアクションは何ですか？** mbox が表示された、target-global-mbox
