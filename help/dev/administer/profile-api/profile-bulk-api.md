---
title: Adobe Target一括プロファイル更新 API
description: 使用方法を学ぶ [!DNL Adobe Target] [!UICONTROL プロファイル一括更新 API] 複数の訪問者のプロファイルデータを [!DNL Target] ターゲティングで使用します。
feature: APIs/SDKs
contributors: https://github.com/icaraps
source-git-commit: a6f47c99cfc419771c1a6674990c415a2035ab4e
workflow-type: tm+mt
source-wordcount: '828'
ht-degree: 8%

---

# [!DNL Adobe Target Bulk Profile Update API]

The [!DNL Adobe Target] [!UICONTROL プロファイル一括更新 API] バッチファイルを使用して、1 つの web サイトに対する複数の訪問者のユーザープロファイルを一括で更新できます。

の使用 [!UICONTROL プロファイル一括更新 API]を使用すると、多くのユーザーが以下の操作をおこなうために、詳細な訪問者プロファイルデータをプロファイルパラメーターの形式で簡単に送信できます。 [!DNL Target] 任意の外部ソースから。 外部ソースには、顧客関係管理 (CRM) システムや POS(Point of Sale) システムが含まれますが、これらは Web ページでは通常利用できません。

| バージョン | URL の例 | 機能 |
| --- | --- | --- |
| v1 | `http://CLIENTCODE.tt.omtrdc.net/m2/ CLIENTCODE/profile/batchUpdate` | プロファイルの一括更新のみサポートされます。 |
| v2 | `http://CLIENTCODE.tt.omtrdc.net/m2/ CLIENTCODE/v2/profile/batchUpdate` | <ul><li>見つからない場合はプロファイルを作成します。</li><li>行ごとのステータス更新。</li></ul> |

>[!NOTE]
>
>のバージョン 2(v2) [!UICONTROL プロファイル一括更新 API] は現在のバージョンです。 しかし、 [!DNL Target] は、バージョン 1(v1) をサポートします。

## プロファイル一括更新 API のメリット

* プロファイル属性の数に上限がありません。
* サイトを介して送信されるプロファイル属性は、API を介して更新することも、逆の方法で更新することもできます。

## 注意事項

* バッチファイルの容量は 50 MB 未満にする必要があります。また、1 回にアップロードできる行数は 50 万行までです。
* 後続のバッチで 24 時間以内にアップロードできる行数に制限はありません。 ただし、他のプロセスを効率的に実行するために、営業時間中は取り込みプロセスが調整される場合があります。
* 同じ thirdPartyIds に対して、間に mbox を呼び出すことなく、連続する v2 一括更新呼び出しを実行すると、最初の一括更新呼び出しで更新されたプロパティは上書きされます。
* [!DNL Adobe] では、バッチプロファイルデータの 100%が Target でオンボーディングされて保持され、ターゲティングで使用できるとは限りません。 現在の設計では、データのごく一部（大規模な実稼動バッチの 0.1%まで）がオンボーディングまたは保持されない可能性があります。

## バッチファイル

プロファイルデータを一括で更新するには、バッチファイルを作成します。 バッチファイルは、次のサンプルファイルのように、値をコンマで区切ったテキストファイルです。

``````
batch=pcId, param1, param2, param3, param4 123, value1 124, value1,,, value4 125,, value2 126, value1, value2, value3, value4
``````

このファイルは、POST呼び出しで [!DNL Target] ファイルを処理するサーバー。 バッチファイルを作成する際は、次の点に注意してください。

* ファイルの最初の行で列ヘッダーを指定する必要があります。
* 最初のヘッダーは、 `pcId` または `thirdPartyId`. The [!UICONTROL Marketing Cloud訪問者 ID] はサポートされていません。 [!UICONTROL pcId] は [!DNL Target]-generated visitorID. `thirdPartyId` はクライアントアプリケーションで指定された ID で、に渡されます。 [!DNL Target] mbox 呼び出しを通じて `mbox3rdPartyId`. ここでは、以下のように呼ぶ必要があります。 `thirdPartyId`.
* セキュリティ上の理由から、バッチファイルで指定するパラメーターと値は、UTF-8 を使用して URL エンコードする必要があります。 パラメーターと値は、HTTP リクエストを通じて他のエッジノードに転送し、処理することができます。
* パラメーターは形式で指定する必要があります `paramName` のみ。 パラメーターは、 [!DNL Target] as `profile.paramName`.
* を使用している場合、 [!UICONTROL プロファイル一括更新 API] v2 の場合、 `pcId`. プロファイルは、 `pcId` または `mbox3rdPartyId` それは、 [!DNL Target]. v1 を使用している場合、pcIds または mbox3rdPartyIds が見つからないプロファイルは作成されません。
* バッチファイルの容量は 50 MB 未満にする必要があります。また、行の総数が 500,000 を超えないようにしてください。 この制限により、サーバーが多くのリクエストであふれるのを防ぐことができます。
* 複数のファイルを送信できます。 ただし、1 日に送信するすべてのファイルの合計行数は、クライアントごとに 100 万行を超えないようにする必要があります。
* アップロードできる属性の数に制限はありません。 ただし、システムデータを含むプロファイルの全体的なサイズは 2000 KB を超えないでください。 [!DNL Adobe] では、プロファイル属性に 1000 KB 未満のストレージを使用することをお勧めします。
* パラメーターおよび値の大文字と小文字は区別されます。

## HTTPPOSTリクエスト

に対する HTTPPOSTリクエスト [!DNL Target] エッジサーバーを使用してファイルを処理します。 curl コマンドを使用した batch.txt ファイルの HTTPPOSTリクエストの例を次に示します。

``````
curl -X POST --data-binary @BATCH.TXT http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate
``````

要素の説明：

BATCH.TXT はファイル名です。 CLIENTCODE は [!DNL Target] クライアントコード。

クライアントコードがわからない場合は、 [!DNL Target] ユーザーインターフェイスのクリック **[!UICONTROL 管理]** > **[!UICONTROL 実装]**. クライアントコードは、 [!UICONTROL アカウントの詳細] 」セクションに入力します。

### Inspect応答

プロファイル API は、処理するバッチの送信ステータスと、特定のバッチジョブの全体的なステータスを示す別の URL への「batchStatus」の下のリンクを返します。

### API 応答の例

次のコードスニップは、Profiles API 応答の例です。

```
<response>
    <success>true</success>
    <batchStatus>http://mboxedge45.tt.omtrdc.net/m2/demo/profile/batchStatus?batchId=demo-1701473848678-13029383</batchStatus>
    <message>Batch submitted for processing</message>
</response>
```

エラーが発生した場合、応答には次の情報が含まれます。 `success=false` とエラーの詳細なメッセージ。

### デフォルトのバッチステータス応答

上記の `batchStatus` URL リンクがクリックされると、次のようになります。

```
<response><batchId>demo4-1701473848678-13029383</batchId><status>complete</status><batchSize>1</batchSize></response>
```

ステータスフィールドに期待される値は次のとおりです。

| ステータス | 詳細 |
| --- | --- |
| [!UICONTROL complete] | プロファイルの一括更新要求が正常に完了しました。 |
| [!UICONTROL 不完全] | プロファイルのバッチ更新リクエストは、まだ処理中で、完了していません。 |
| [!UICONTROL 詰まった] | プロファイルのバッチ更新リクエストが停止しており、完了できませんでした。 |

### 詳細なバッチステータス URL 応答

より詳細な応答は、パラメーターを渡すことで取得できます `showDetails=true` から `batchStatus` url を上に置きます。

次に例を示します。

```
http://mboxedge45.tt.omtrdc.net/m2/demo/profile/batchStatus?batchId=demo-1701473848678-13029383&showDetails=true
```

#### 詳細な応答

```
<response>
    <batchId>demo4-1701473848678-13029383</batchId>
    <status>complete</status>
    <batchSize>1</batchSize>
    <consumedCount>1</consumedCount>
    <successfulUpdates>1</successfulUpdates>
    <profilesNotFound>0</profilesNotFound>
    <failedUpdates>0</failedUpdates>
</response>
```
