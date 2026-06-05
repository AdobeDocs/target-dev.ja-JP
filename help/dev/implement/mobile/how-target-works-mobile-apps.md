---
description: ' [!DNL Adobe Mobile SDK] を使用して、モバイルアプリ訪問者に最適なエクスペリエンスを表示する方法について説明します。'
title: モバイルアプリでの [!DNL Target] の動作について
feature: Implement Mobile
exl-id: 33001f01-fde6-48cb-ac02-d1a632b2150d
TQID: https://experienceleague.adobe.com/R3B-i9BFKaoTkbfzVLOU-j8VV2K-MpNrf0WTCkMceT8
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2: id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
topic_v2: id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 237
ht-degree: 21%

---

# モバイルアプリでの[!DNL Target]の仕組み

[!DNL Adobe Mobile SDK]は、[!DNL Target] サーバーに連絡して、コンテンツと他のデータポイントを取得し、適切なエクスペリエンスをユーザーに表示します。

>[!IMPORTANT]
>
>[!DNL Adobe Mobile] バージョン 4.*x* SDKのサポートは2021年8月31日（PT）をもって終了し、[!DNL Adobe Target] モバイルユーザーには推奨されなくなりました。
>
>[Adobe Experience Platform SDK モバイル版アプリ ](https://developer.adobe.com/client-sdks/documentation/){target=_blank}は、モバイル アプリで[!DNL Adobe Experience Cloud]のソリューションとサービスを強化するために推奨されるソリューションです。

## [!DNL Target]の場所と成功指標

*ターゲットの場所* はmboxとも呼ばれます。 アプリで識別した場所は、テストとパーソナライゼーション（例えば、ホーム画面のようこそメッセージ）のために有効になります。 これらの場所は、テスト作成プロセスの間に識別されます。

*[成功指標](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html)*&#x200B;は、特定のアクティビティ （サインアップ、購入、チケットの予約など）が成功したかどうかを識別するためにユーザーが実行するアクションです。

![alt画像](assets/mobile-target-location.png)

* **[!DNL Target]の場所：**&#x200B;登録ボタンの下に表示されるコンテンツ。

  このユーザーは、18 時まで送料無料の提供を受けています。 この場所は、A/B テストとパーソナライゼーションを実行するために、複数の[!DNL Target] アクティビティで再利用できます。

* **成功指標：** ユーザーが登録ボタンをタップした際に実行したアクション。

**SDKでの[!DNL Target]の仕組みを理解**

![alt画像](assets/how-target-mobile-works.png)
