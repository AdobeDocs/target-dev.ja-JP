---
keywords: トラブルシューティング, よくある質問, FAQ, グローバル, グローバル mbox
description: Adobe [!DNL Target] global mboxに関するよくある質問（FAQ）と回答をご覧ください。
title: グローバル mboxに関するよくある質問は何ですか？
feature: at.js
exl-id: 7bcd1b67-809a-466a-b648-6e0e44386157
TQID: https://experienceleague.adobe.com/bxsjCqSQpp6M20StzZtMBrfxjJCKgPEPfS2OlBUP00A
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 309
ht-degree: 32%

---

# グローバル mbox に関するよくある質問

グローバル mbox に関するよくある質問（FAQ）のリストです。

## [!DNL Target] アカウントが複数のドメインに設定されている場合、複数のグローバル mboxを持つことはできますか？

アカウント全体で使用できるグローバル mbox は 1 つだけです。

アクティビティに URL ルールを追加することで、アクティビティの実行場所を制限できます。 詳しくは、[類似ページに同じエクスペリエンスを含める](https://experienceleague.adobe.com/docs/target/using/experiences/vec/temtest.html)を参照してください。

[targetPageParams](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md)を使用してページ上のパラメーターを渡し、[!UICONTROL Visual Experience Composer] （VEC）の「URLの設定」セクションでこれらのパラメーターを選択するか、[!UICONTROL Form-Based Experience Composer]で「絞り込み」としてパラメーターを追加することもできます。

## [!DNL Target] グローバル mboxで収益データを渡すにはどうすればよいですか？

target-global-mboxの収益と注文情報を収集するには、「mbox パラメーター」を[!DNL Target]に送信する必要があります。 これらのパラメーターは、詳細情報を[!DNL Target]に送信するために使用される名前と値のペアです。 [!DNL Target]は、収益データを入力するために、これらのパラメーター（予約済みの名前）を自動的に検索します。

`orderConfirmPage`には、`orderTotal`、`orderId`、`productPurchasedId`を渡す必要があります。

これらのパラメーターは、`targetPageParams()`経由でtarget-global-mboxに送信する必要があります。 詳しくは、「[グローバル mbox にパラメーターを渡す](/help/dev/implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md)」を参照してください。

また、次に示すように、注文確認ページが表示されたときに[!DNL Target]がtarget-global-mboxのコンバージョンのみをカウントするように、ターゲティングをコンバージョン部分に追加します。

![alt画像](assets/revenue1.png)

上図の「サイトのページ」セクションの選択内容は、「現在のページ」、「URL」、「次を含む」、「orderconfirm」となっています。

![alt画像](assets/revenue2.png)

上図のオプションは、以下の設定で構成されています。

* **このアクティビティでは何を測定しますか？：**&#x200B;売上高
* **レポートのデフォルトの表示：**&#x200B;訪問者あたりの売上高（RPV）
* **目標が達成されたことを示すために、オーディエンスが実行したアクションは何ですか？** mbox、target-global-mboxを表示しました
