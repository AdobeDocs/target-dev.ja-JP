---
title: Adobe Analytics for Target （A4T）Experience Platform Web SDKでのログ記録
description: Experience Platform Web SDKを使用して、Adobe Analytics for Target （A4T）データのコレクションを制御する方法について説明します。
seo-title: Adobe Analytics for Target (A4T) Logging in the Experience Platform Web SDK
seo-description: Learn how to control the collection of Adobe Analytics for Target (A4T) data using the Experience Platform Web SDK.
keywords: a4t；ログ；分析；sdk;web sdk;
feature: Implementation
exl-id: 886588bf-4416-4f1e-aecc-467e48c8fb23
TQID: https://experienceleague.adobe.com/cShqvj3wSialxA-ajnROnIpzjuz66pNg3CLM6l2xPLg
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: a94ced60-8199-4549-b453-ede2acb4101e
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c2be0313-b3ae-45e0-b454-d20bf54b23f2
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 265
ht-degree: 0%

---

# [!DNL Adobe Analytics for Target] （A4T） [!DNL Experience Platform Web SDK]にログイン

パーソナライゼーションに[!DNL Adobe Target]を使用する場合は、パフォーマンス測定に使用するシステムを選択できます。 各[Target アクティビティ &#x200B;](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html)では、[!DNL Target]件のレポートとAdobe [!DNL Analytics]件のレポートを選択できます。

[!DNL Analytics] レポートを使用している場合、[!DNL Target]は次の情報を[!DNL Analytics]に伝える必要があります。

* 訪問者が入力した[!DNL Target] アクティビティ
* その際に見た体験が
* コンバージョンに達したオーディエンス

[!DNL Adobe Experience Platform Web SDK]は、[!UICONTROL Analytics for Target] （A4T）のユースケースに対する[!DNL Analytics] ログ記録の2種類をサポートしています。

| ロギング方法 | 説明 |
| --- | --- |
| サーバーサイド [!DNL Analytics] ログ | Edge Networkを介して送信されたすべての[!DNL Analytics] ヒットは、ヒットのステッチプロセスを経ることなく、サーバーサイドの[!DNL Target]詳細で強化されます。 |
| クライアントサイド [!DNL Analytics] ログ | [!DNL Target]個のデータがクライアント側で返されるため、[Data Insertion API](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html)を使用して手動でデータを拡張して[!DNL Analytics]に送信できます。 |

ロギング方法は、設定した[&#x200B; データストリーム &#x200B;](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/overview)で[!DNL Adobe Analytics]が有効になっているかどうかで決まります。

![&#x200B; メソッド決定フローのログ記録](/help/dev/implement/a4t/assets/analytics-logging.png)

## 次のステップ

このドキュメントでは、Web SDKでのA4T データの様々なログ記録方法について簡単に説明しました。 これらの各方法について詳しくは、次のドキュメントを参照してください。

* [Experience Platform Web SDKでのA4T データのサーバーサイドロギング](/help/dev/implement/a4t/client-side-logging.md)
* [Experience Platform Web SDKでのA4T データのクライアントサイドログ](/help/dev/implement/a4t/client-side-logging.md)
