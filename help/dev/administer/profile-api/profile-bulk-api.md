---
title: Adobe Target プロファイル一括更新 API
description: の使用方法を学ぶ [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] 複数の訪問者のプロファイルデータをに送信するには [!DNL Target] ターゲティングで使用します。
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 0f38d109-5273-4f73-9488-80eca115d44d
source-git-commit: 2934fbaa1dc3cd92bc5a434937e5db9a617009a9
workflow-type: tm+mt
source-wordcount: '828'
ht-degree: 8%

---

# [!DNL Adobe Target Bulk Profile Update API]

この [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] では、Web サイトへの複数の訪問者のユーザープロファイルをバッチファイルで一括更新できます。

使用， [!UICONTROL Bulk Profile Update API]を使用すると、多くのユーザーが、プロファイルパラメーターの形式で詳細な訪問者プロファイルデータを次の目的で簡単に送信できます [!DNL Target] 任意の外部ソースから。 外部ソースには顧客関係管理（CRM）システムや販売時点（POS）システムを含めますが、これらのシステムは通常、web ページでは使用できません。

| バージョン | URL の例 | 機能 |
| --- | --- | --- |
| v1 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/profile/batchUpdate` | プロファイルの一括更新のみをサポートします。 |
| v2 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate` | <ul><li>見つからない場合はプロファイルを作成</li><li>行ごとのステータスの更新。</li></ul> |

>[!NOTE]
>
>のバージョン 2 （v2） [!UICONTROL Bulk Profile Update API] は現在のバージョンです。 ただし、 [!DNL Target] は、バージョン 1 （v1）を引き続きサポートします。

## Bulk Profile Update API の利点

* プロファイル属性の数に上限がありません。
* サイトを介して送信されたプロファイル属性は、API を使用して更新できます。その逆も可能です。

## 注意事項

* バッチファイルの容量は 50 MB 未満にする必要があります。また、1 回にアップロードできる行数は 50 万行までです。
* 更新は通常 1 時間以内に行われますが、反映されるまでに 24 時間かかる場合があります。
* 後続のバッチで 24 時間にわたってアップロードできるアップロード行数に制限はありません。 ただし、他のプロセスを効率的に実行するために、営業時間中は取り込みプロセスが調整される場合があります。
* 同じ thirdPartyIds の間に mbox 呼び出しを含まない v2 バッチ更新呼び出しが連続して発生すると、最初のバッチ更新呼び出しで更新されたプロパティが上書きされます。
* [!DNL Adobe] バッチプロファイルデータの 100% が Target に転送され保持され、ターゲティングで使用できることを、は保証しません。 現在の設計では、わずかな割合のデータ（大規模な生産バッチの 0.1% 以下）は、オンボードまたは保持されない可能性があります。

## バッチファイル

プロファイルデータを一括更新するには、バッチファイルを作成します。 バッチファイルは、次のサンプルファイルと同様に、コンマで区切られた値を持つテキストファイルです。

``````
batch=pcId,param1,param2,param3,param4
123,value1
124,value1,,,value4
125,,value2
126,value1,value2,value3,value4
``````

>[!NOTE]
>
>この `batch=` パラメーターが必要であり、ファイルの先頭で指定する必要があります。

このファイルは、へのPOST呼び出しで参照されます [!DNL Target] ファイルを処理するサーバー。 バッチファイルを作成する場合は、次の点を考慮してください。

* ファイルの最初の行では、列ヘッダーを指定する必要があります。
* 最初のヘッダーはでなければなりません `pcId` または `thirdPartyId`. この [!UICONTROL Marketing Cloud visitor ID] はサポートされていません。 [!UICONTROL pcId] が [!DNL Target] – 生成された visitorID。 `thirdPartyId` は、クライアントアプリケーションによって指定された ID で、に渡されます。 [!DNL Target] ～としての mbox 呼び出しを通じて `mbox3rdPartyId`. ここでは、次のように参照する必要があります。 `thirdPartyId`.
* セキュリティ上の理由から、バッチファイルで指定するパラメーターと値は、UTF-8 を使用して URL エンコードする必要があります。 パラメーターと値は、HTTP リクエストを通じて処理するために他のエッジノードに転送できます。
* パラメーターはの形式にする必要があります `paramName` のみ。 パラメーターは次の場所に表示されます。 [!DNL Target] as `profile.paramName`.
* を使用している場合 [!UICONTROL Bulk Profile Update API] v2。それぞれのパラメータ値をすべて指定する必要はありません `pcId`. プロファイルは次のいずれかに対して作成されます `pcId` または `mbox3rdPartyId` 次の場所にありません： [!DNL Target]. v1 を使用している場合、pcId または mbox3rdPartyIds が見つからないプロファイルは作成されません。
* バッチファイルの容量は 50 MB 未満にする必要があります。また、合計行数は 500,000 を超えないようにする必要があります。 この制限により、サーバーに大量のリクエストが送られないようにすることができます。
* 複数のファイルを送信できます。 ただし、1 日に送信するすべてのファイルの行の合計は、各クライアントで 100 万行を超えないようにしてください。
* アップロードする属性の数に制限はありません。 ただし、システムデータを含むプロファイルの全体のサイズは、2000 KB を超えないようにしてください。 [!DNL Adobe] では、プロファイル属性に使用するストレージは 1,000 KB 未満にすることをお勧めします。
* パラメーターと値は、大文字と小文字を区別します。

## HTTP POSTリクエスト

に HTTPPOSTリクエストを送信します。 [!DNL Target] ファイルを処理するエッジサーバー。 curl コマンドを使用した batch.txt ファイルの HTTPPOSTリクエストの例を次に示します。

``````
curl -X POST --data-binary @BATCH.TXT http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate
``````

要素の説明：

BATCH.TXT はファイル名です。 CLIENTCODE は [!DNL Target] クライアントコード。

クライアントコードがわからない場合は、 [!DNL Target] ユーザーインターフェイスのクリック **[!UICONTROL Administration]** > **[!UICONTROL Implementation]**. クライアントコードはに表示されます。 [!UICONTROL Account Details] セクション。

### 応答をInspect

プロファイル API は、「batchStatus」の下にあるリンクと共に、処理対象のバッチの送信ステータスを、特定のバッチジョブの全体的なステータスを表示する別の URL に返します。

### API 応答の例

次のコードは、Profiles API 応答の例です。

```
<response>
    <success>true</success>
    <batchStatus>http://mboxedge45.tt.omtrdc.net/m2/demo/profile/batchStatus?batchId=demo-1701473848678-13029383</batchStatus>
    <message>Batch submitted for processing</message>
</response>
```

エラーが発生した場合、応答にはが含まれます `success=false` とエラーの詳細メッセージ。

### デフォルトのバッチステータス応答

上記の場合に成功したデフォルト応答 `batchStatus` URL リンクのクリックは次のようになります。

```
<response><batchId>demo4-1701473848678-13029383</batchId><status>complete</status><batchSize>1</batchSize></response>
```

ステータスフィールドに想定される値は次のとおりです。

| ステータス | 詳細 |
| --- | --- |
| [!UICONTROL complete] | プロファイルバッチ更新リクエストが正常に完了しました。 |
| [!UICONTROL incomplete] | プロファイルバッチ更新リクエストは現在も処理中で、完了していません。 |
| [!UICONTROL stuck] | プロファイルバッチ更新リクエストが停止し、完了できませんでした。 |

### 詳細なバッチステータス URL 応答

パラメーターを渡すことで、より詳細な応答を取得できます `showDetails=true` に `batchStatus` 上記の URL。

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
