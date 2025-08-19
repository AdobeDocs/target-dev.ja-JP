---
title: Adobe Target プロファイル一括更新 API
description: ' [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] を使用して、ターゲティングで使用するために複数の訪問者のプロファイルデータをに送信する方法  [!DNL Target]  説明します。'
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 0f38d109-5273-4f73-9488-80eca115d44d
source-git-commit: dae198fd8ef3fc8473ad31807c146802339b1832
workflow-type: tm+mt
source-wordcount: '917'
ht-degree: 7%

---

# [!DNL Adobe Target Bulk Profile Update API]

[!DNL Adobe Target] [!UICONTROL Bulk Profile Update API] では、Web サイトへの複数の訪問者のユーザープロファイルをバッチファイルで一括更新できます。

[!UICONTROL Bulk Profile Update API] を使用すると、多くのユーザーが任意の外部ソースから [!DNL Target] 信できるように、プロファイルパラメーターの形式で詳細な訪問者プロファイルデータを簡単に送信できます。 外部ソースには顧客関係管理（CRM）システムや販売時点（POS）システムを含めますが、これらのシステムは通常、web ページでは使用できません。

| バージョン | URL の例 | 機能 |
| --- | --- | --- |
| v1 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/profile/batchUpdate` | プロファイルの一括更新のみをサポートします。 |
| v2 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate` | <ul><li>見つからない場合はプロファイルを作成</li><li>行ごとのステータスの更新。</li></ul> |

>[!NOTE]
>
>[!DNL Bulk Profile Update API] のバージョン 2 （v2）は現在のバージョンです。 ただし、[!DNL Target] は引き続きバージョン 1 （v1）をサポートします。
>
>* [!DNL Target] 実装で [!DNL Experience Cloud ID] （ECID）を匿名訪問者のプロファイル識別子の 1 つとして使用する場合は、バージョン 2 （v2）バッチファイルのキーとして `pcId` を使用しないでください。 `pcId` の v2 での [!DNL Bulk Profile Update API] の使用は、ECID に依存しないスタンドアロン [!DNL Target] 実装のみを目的としています。
>
>* 実装でプロファイルの識別に ECID を使用していて、`pcId` をバッチファイルのキーとして使用する場合は、API のバージョン 1 （v1）を使用します。
>
>* 実装で `thirdPartyId` をプロファイルの識別に使用する場合は、`thirdPartyId` をキーとする API のバージョン 2 （v2）を使用します。

## [!UICONTROL Bulk Profile Update API] のメリット

* プロファイル属性の数に上限がありません。
* サイトを介して送信されたプロファイル属性は、API を使用して更新できます。その逆も可能です。

## 注意事項

* バッチファイルの容量は 50 MB 未満にする必要があります。また、1 回にアップロードできる行数は 50 万行までです。
* 更新は通常 1 時間以内に行われますが、反映されるまでに 24 時間かかる場合があります。
* 後続のバッチで 24 時間にわたってアップロードできるアップロード行数に制限はありません。 ただし、他のプロセスを効率的に実行するために、営業時間中は取り込みプロセスが調整される場合があります。
* 同じ thirdPartyIds の間に mbox 呼び出しを含まない v2 バッチ更新呼び出しが連続して発生すると、最初のバッチ更新呼び出しで更新されたプロパティが上書きされます。
* [!DNL Adobe] では、バッチプロファイルデータの 100% が Target に転送されて保持され、ターゲティングで使用できることを保証しません。 現在の設計では、わずかな割合のデータ（大規模な生産バッチの 0.1% 以下）は、オンボードまたは保持されない可能性があります。

## バッチファイル

プロファイルデータを一括更新するには、バッチファイルを作成します。 バッチファイルは、次のサンプルファイルと同様に、コンマで区切られた値を持つテキストファイルです。

``` ```
batch=pcId,param1,param2,param3,param4
123,value1
124,value1,,,value4
125,,value2
126,value1,value2,value3,value4
``` ```

>[!NOTE]
>
>`batch=` パラメーターは必須であり、ファイルの先頭で指定する必要があります。

このファイルは、[!DNL Target] サーバーへの POST 呼び出しで参照し、ファイルを処理します。 バッチファイルを作成する場合は、次の点を考慮してください。

* ファイルの最初の行では、列ヘッダーを指定する必要があります。
* 最初のヘッダーは、`pcId` または `thirdPartyId` にする必要があります。 [!UICONTROL Marketing Cloud visitor ID] はサポートされていません。 [!UICONTROL pcId] は、[!DNL Target] で生成された visitorID です。 `thirdPartyId` は、クライアントアプリケーションによって指定された ID で、[!DNL Target] として mbox 呼び出しを通じて `mbox3rdPartyId` に渡されます。 ここでは、`thirdPartyId` と呼ぶ必要があります。
* セキュリティ上の理由から、バッチファイルで指定するパラメーターと値は、UTF-8 を使用して URL エンコードする必要があります。 パラメーターと値は、HTTP リクエストを通じて処理するために他のエッジノードに転送できます。
* パラメーターは、`paramName` の形式のみにする必要があります。 パラメーターは、[!DNL Target] のように `profile.paramName` に表示されます。
* [!UICONTROL Bulk Profile Update API] v2 を使用している場合は、各 `pcId` に対してすべてのパラメーター値を指定する必要はありません。 プロファイルは、`pcId` に見つからない `mbox3rdPartyId` または [!DNL Target] に対して作成されます。 v1 を使用している場合、pcId または mbox3rdPartyIds が見つからないプロファイルは作成されません。
* バッチファイルの容量は 50 MB 未満にする必要があります。また、合計行数は 500,000 を超えないようにする必要があります。 この制限により、サーバーに大量のリクエストが送られないようにすることができます。
* 複数のファイルを送信できます。 ただし、1 日に送信するすべてのファイルの行の合計は、各クライアントで 100 万行を超えないようにしてください。
* アップロードできる属性の数に制限はありません。 ただし、顧客属性、プロファイル API、In-Mbox プロファイルパラメーター、プロファイルスクリプト出力を含む外部プロファイルデータの合計サイズは、64 KB を超えないようにする必要があります。
* パラメーターと値は、大文字と小文字を区別します。

## HTTP POST リクエスト

[!DNL Target] エッジサーバーに HTTP POST リクエストを送信して、ファイルを処理します。 curl コマンドを使用した batch.txt ファイルの HTTP POST リクエストの例を次に示します。

``` ```
curl -X POST --data-binary @BATCH.TXT http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate
``` ```

要素の説明：

BATCH.TXT はファイル名です。 CLIENTCODE は、[!DNL Target] のクライアントコードです。

クライアントコードがわからない場合は、[!DNL Target] ユーザーインターフェイスで **[!UICONTROL Administration]**/**[!UICONTROL Implementation]** をクリックします。 クライアントコードは [!UICONTROL Account Details] セクションに表示されます。

### 応答を調べる

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

エラーが発生した場合、応答には `success=false` とエラーの詳細メッセージが含まれます。

### デフォルトのバッチステータス応答

上記の `batchStatus` URL リンクがクリックされた場合、デフォルトの応答が成功すると、次のようになります。

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

上記の `showDetails=true` の URL にパラメーター `batchStatus` を渡すことで、より詳細な応答を取得できます。

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
