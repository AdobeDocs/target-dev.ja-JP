---
description: の使用方法を学ぶ [!DNL Adobe Mobile SDK] を使用して、モバイルアプリ訪問者に最適なエクスペリエンスを表示できます。
title: 方法 [!DNL Target] モバイルアプリで作業する場合
feature: Implement Mobile
exl-id: 33001f01-fde6-48cb-ac02-d1a632b2150d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 34%

---

# 方法 [!DNL Target] は、モバイルアプリで機能します。

The [!DNL Adobe Mobile SDK] 連絡先： [!DNL Target] サーバーを使用してコンテンツを他のデータポイントと共に取得し、ユーザーに適切なエクスペリエンスを表示します。

>[!IMPORTANT]
>
>のサポート [!DNL Adobe Mobile] バージョン 4.*x* SDK は 2021 年 8 月 31 日をもって終了しており、お勧めしません。 [!DNL Adobe Target] モバイルユーザー。
>
>The [モバイルアプリ用Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/){target=_blank} は、を強化するための推奨されるソリューションです。 [!DNL Adobe Experience Cloud] ソリューションおよびサービスをモバイルアプリに追加できます。

## [!DNL Target] 場所と成功指標

*ターゲットの場所* はmboxとも呼ばれます。アプリで識別した場所は、テストとパーソナライゼーション（例えば、ホーム画面のようこそメッセージ）のために有効になります。これらの場所は、テスト作成プロセスの間に識別されます。

*[成功指標](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html)*&#x200B;はユーザーが実行するアクションで、特定のアクティビティが成功したかどうか（サインアップ、購入、チケットの予約など）を識別します。

![代替画像](assets/mobile-target-location.png)

* **[!DNL Target]場所：** 登録ボタンの下に表示されるコンテンツ。

  このユーザーは、18 時まで送料無料の提供を受けています。この場所は複数の場所で再利用できます [!DNL Target] A/B テストとパーソナライゼーションを実行するアクティビティ

* **成功指標：** ユーザがレジスタボタンをタップしたときに実行するアクション。

**方法を理解する [!DNL Target] は SDK で機能します。**

![代替画像](assets/how-target-mobile-works.png)
