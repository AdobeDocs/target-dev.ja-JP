---
title: オンデバイス判定ルールアーティファクトについて
description: ルールアーティファクト（JSON 表現）の使用方法を説明します。 [!DNL Adobe Target] [!UICONTROL オンデバイス判定] アクティビティ。
feature: APIs/SDKs
exl-id: 3dfb08df-eaa9-43d4-b009-e5f64c3a96d7
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '280'
ht-degree: 0%

---

# ルールアーティファクトの概要

ルールアーティファクトは、 [!DNL Adobe Target] [!UICONTROL オンデバイス判定] アクティビティ。 これは次の方法で生成されます。 [!DNL Adobe Target] Akamai CDN に反映され、エンドユーザーにできる限り近い場所にルールアーティファクトが存在することを確認します。 アクティビティの正確な実行と配信を保証するメタデータと、イベントトラッキングを介したリアルタイムの分析を可能にするメタデータが含まれています。 The [!DNL Adobe Target] SDK は、ルールアーティファクトを自動的に管理できるように設定できます。ユーザーが指定した時間間隔に従って、SDK をダウンロードまたは更新できます。 さらに、のような分散メモリキャッシュシステムを使用して、ルールアーティファクトの独自のローカルコピーを維持することもできます。 [Memcached](https://memcached.org/) 初期設定 [!DNL Adobe Target] SDK を使用することで、ステートレスサーバーはリクエストを即座に処理できます。 これらのオプションの詳細については、次のガイドを参照してください。

* [を使用したルールアーティファクトの自動的なダウンロード、保存および更新 [!DNL Adobe Target] SDK](rule-artifact-sdk.md)
* [JSON ペイロードを使用したルールアーティファクトのダウンロード、保存、更新](rule-artifact-json.md)

## ルールアーティファクトの例

以下の例については、ここをクリックしてください： [ルールアーティファクト](rule-artifact-example.md).

## クライアントのルールアーティファクトを表示する方法

トレースを有効にすると、次の追加情報が出力されます： [!DNL Adobe Target] ルールアーティファクトに関しては、具体的には URL です。

1. Target UI に移動します。

   &lt;!— image-target-ui-1.png を挿入 —>
   ![代替画像](assets/asset-rule-artifact-1.png)

1. に移動します。 **[!UICONTROL 管理]** > **[!UICONTROL 実装]** をクリックします。 **[!UICONTROL 新しい認証トークンを生成]**.

   &lt;!— image-target-ui-2.png を挿入 —>
   ![代替画像](assets/asset-rule-artifact-2.png)

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

1. ターミナルを介して Target Trace を出力し、アーティファクトに関する詳細を表示します。 URL には、 `artifactLocation` 変数を使用します。

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
