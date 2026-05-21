---
title: オンデバイス決定ルールのアーティファクトについて
description: ' [!DNL Adobe Target] [!UICONTROL on-device decisioning] アクティビティのJSON表現であるルールアーティファクトの使用方法を説明します。'
feature: APIs/SDKs
exl-id: 3dfb08df-eaa9-43d4-b009-e5f64c3a96d7
TQID: https://experienceleague.adobe.com/mPzCK-vBYFAQnslX-8FPsBaeSiYtyxjZv76anbpHWuE
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dcid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 86209eb483ca69d40615c632ba435d27fec78f36
workflow-type: tm+mt
source-wordcount: 266
ht-degree: 0%

---

# ルールアーティファクトの概要

ルールアーティファクトは、[!DNL Adobe Target] [!UICONTROL on-device decisioning] アクティビティのJSON表現です。 [!DNL Adobe Target]によって生成され、エンドユーザーにできるだけ近いルールアーティファクトが利用可能になるようにAkamai CDNに反映されます。 アクティビティを正確に実行して配信するためのメタデータと、イベントのトラッキングによるリアルタイムの分析を備えています。 [!DNL Adobe Target] SDKは、ルールアーティファクトの自動管理を可能にする方法で設定できます。これにより、ユーザーが指定した時間間隔に従ってダウンロードまたは更新できます。 さらに、[Memcached](https://memcached.org/)などの分散メモリキャッシュシステムを使用してルールアーティファクトの独自のローカルコピーを管理し、[!DNL Adobe Target] SDKを初期化して、ステートレスサーバーがリクエストをすぐに処理できるようにすることもできます。 これらのオプションについて詳しくは、次のガイドを参照してください。

* [ [!DNL Adobe Target] SDKを介したルールアーティファクトの自動ダウンロード、保存、更新](rule-artifact-sdk.md)
* [JSON ペイロードを使用したルールアーティファクトのダウンロード、保存、更新](rule-artifact-json.md)

## ルールアーティファクトの例

[ ルール アーティファクト ](rule-artifact-example.md)の例については、ここをクリックしてください。

## クライアントのルールアーティファクトの表示方法

トレースを有効にすると、ルールアーティファクト、特にURLに関する[!DNL Adobe Target]からの追加情報が出力されます。

1. Target UIに移動します。

   <!-- Insert image-target-ui-1.png -->
   ![alt画像](assets/asset-rule-artifact-1.png)

1. **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**&#x200B;に移動し、**[!UICONTROL Generate New Authorization Token]**&#x200B;をクリックします。

   <!-- Insert image-target-ui-2.png -->
   ![alt画像](assets/asset-rule-artifact-2.png)

1. 新しく生成された認証トークンをクリップボードにコピーし、Target リクエストに追加します。

   ```javascript {line-numbers="true"}
   const request = {
     trace: {
       authorizationToken: '88f1a924-6bc5-4836-8560-2f9c86aeb36b'
     },
     execute: {
       mboxes: [{
         address: getAddress(req),
         name: "node-sdk-mbox"
       }]
   }};
   ```

1. ターミナルを介してTarget Traceを出力し、アーティファクトに関する詳細を表示します。 URLには、`artifactLocation`変数からアクセスできます。

   ```
   "trace": {
     "clientCode": "your-client-code",
     "artifact": {
       "artifactLocation": "https://assets.adobetarget.com/your-client-code/production/v1/rules.bin",
       "pollingInterval": 300000,
       "pollingHalted": false,
       "artifactVersion": "1.0.0",
       "artifactRetrievalCount": 10,
       "artifactLastRetrieved": "2020-09-20T00:09:42.707Z",
        "clientCode": "your-client-code",
      "environment": "production",
       "generatedAt": "2020-09-22T17:17:59.783Z"
     },
   ```
