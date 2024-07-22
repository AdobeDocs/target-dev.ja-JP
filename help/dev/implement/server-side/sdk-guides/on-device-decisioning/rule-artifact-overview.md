---
title: オンデバイス判定ルールアーティファクトについて
description: ' [!DNL Adobe Target] [!UICONTROL on-device decisioning] アクティビティの JSON 表現であるルールアーティファクトの使用方法を説明します。'
feature: APIs/SDKs
exl-id: 3dfb08df-eaa9-43d4-b009-e5f64c3a96d7
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# ルールアーティファクトの概要

ルールアーティファクトは、[!DNL Adobe Target] [!UICONTROL on-device decisioning] アクティビティの JSON 表現です。 ルールは [!DNL Adobe Target] によって生成され、Akamai CDN に伝播されて、エンドユーザーにできるだけ近いルールアーティファクトが提供されるようにします。 アクティビティの正確な実行と配信を保証すると同時に、イベントトラッキングによるリアルタイム分析を可能にするメタデータが含まれています。 [!DNL Adobe Target] SDK は、ルールアーティファクトを自動管理できる方法で設定できます。これにより、ユーザー指定の時間間隔に従って SDK をダウンロードまたは更新できます。 さらに、[Memcached](https://memcached.org/) などの分散メモリキャッシュシステムを使用して、ルールアーティファクトの独自のローカルコピーを維持し、[!DNL Adobe Target] SDK を初期化することもできます。これにより、ステートレスサーバーがリクエストをすぐに処理できます。 これらのオプションについて詳しくは、次のガイドを参照してください。

* [ [!DNL Adobe Target] SDK を使用したルールアーティファクトの自動ダウンロード、保存および更新](rule-artifact-sdk.md)
* [JSON ペイロードを使用したルールアーティファクトのダウンロード、保存、更新](rule-artifact-json.md)

## ルールアーティファクトの例

[ ルールアーティファクト ](rule-artifact-example.md) の例については、ここをクリックしてください。

## クライアントのルールアーティファクトの表示方法

トレースを有効にすると、ルールアーティファクト（特に URL）に関する追加情報が [!DNL Adobe Target] から出力されます。

1. Target UI に移動します。

   &lt;!— image-target-ui-1.png の挿入 – >
   ![alt 画像 ](assets/asset-rule-artifact-1.png)

1. **[!UICONTROL Administration]**/**[!UICONTROL Implementation]** に移動し、「**[!UICONTROL Generate New Authorization Token]**」をクリックします。

   &lt;!— image-target-ui-2.png の挿入 – >
   ![alt 画像 ](assets/asset-rule-artifact-2.png)

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

1. アーティファクトに関する詳細を表示するには、ターミナルを介して Target Trace を出力します。 URL には、`artifactLocation` 変数を使用してアクセスできます。

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
