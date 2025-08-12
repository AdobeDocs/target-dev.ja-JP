---
title: Experience Platform Web SDKでのAdobe Analytics for Target （A4T）ログ
description: Experience Platform web SDKを使用して、Adobe Analytics for Target （A4T）データの収集を制御する方法について説明します。
seo-title: Adobe Analytics for Target (A4T) Logging in the Experience Platform Web SDK
seo-description: Learn how to control the collection of Adobe Analytics for Target (A4T) data using the Experience Platform Web SDK.
keywords: a4t；ログ；analytics;sdk;web sdk;
feature: Implementation
source-git-commit: b7638f7ab3fe9a6551c9d542a990e22ddb2b27a2
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# [!DNL Adobe Analytics for Target] への [!DNL Experience Platform Web SDK] （A4T）のログイン

パーソナライゼーションに [!DNL Adobe Target] を使用する場合、パフォーマンス測定に使用するシステムを選択できます。 各 [Target アクティビティ ](https://experienceleague.adobe.com/docs/target/using/activities/target-activities-guide.html) を使用すると、レポートを作成する [!DNL Target] とAdobe [!DNL Analytics] レポートを選択できます。

[!DNL Analytics] レポートを使用する場合は、[!DNL Target] に次の [!DNL Analytics] とを伝える必要があります。

* 訪問者が入力した [!DNL Target] アクティビティ
* どのエクスペリエンスを見たか
* どのコンバージョンに到達したか

[!DNL Adobe Experience Platform Web SDK] は、[!DNL Analytics] （A4T）のユースケースに対して、次の 2 種類の [!UICONTROL Analytics for Target] ログをサポートしています。

| ログメソッド | 説明 |
| --- | --- |
| サーバーサイド [!DNL Analytics] ログ | Edge Networkを介して送信される [!DNL Analytics] のすべてのヒットは、ヒットのステッチプロセスを経ることなく、サーバー側で [!DNL Target] の詳細で拡張されます。 |
| クライアントサイド [!DNL Analytics] ログ | [!DNL Target] のデータはクライアントサイドで返されるので、[!DNL Analytics]Data Insertion API[ を使用して手動でデータを補強し、](https://experienceleague.adobe.com/docs/analytics/import/c-data-insertion-api.html) に送信できます。 |

ログの方法は、設定した [!DNL Adobe Analytics]datastream[ で ](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/overview) が有効になっているかどうかによって決まります。

![ メソッドの決定フローの記録 ](/help/dev/implement/a4t/assets/analytics-logging.png)

## 次の手順

このドキュメントでは、web SDKでの A4T データの様々なログ取得方法について簡単に説明しました。 これらの各方法について詳しくは、次のドキュメントを参照してください。

* [Experience Platform Web SDKでの A4T データのサーバーサイドログ](/help/dev/implement/a4t/client-side-logging.md)
* [Experience Platform Web SDKでの A4T データのクライアントサイドログ](/help/dev/implement/a4t/client-side-logging.md)