---
title: Adobe Target一括プロファイル更新 API
description: 使用方法を学ぶ [!DNL Adobe Target] [!UICONTROL プロファイル一括更新 API] 複数の訪問者のプロファイルデータを [!DNL Target].
feature: APIs/SDKs
contributors: https://github.com/icaraps
source-git-commit: 8bc819823462fae71335ac3b6c871140158638fe
workflow-type: tm+mt
source-wordcount: '727'
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

v2 はプロファイルごとのステータスを返し、v1 は全体のステータスのみを返します。 応答には、プロファイル別成功メッセージを持つ別の URL へのリンクが含まれます。

### 応答の例

```
true http://mboxedge19.tt.omtrdc.net/m2/demo/v2/profile/batchStatus?batchId=demo-1845664501&m2Node=00 Batch submitted for processing
```

エラーが発生した場合、応答には次の情報が含まれます。 `success=false` とエラーの詳細なメッセージ。

成功した応答は次のようになります。

``````
demo-1845664501 1436187396849-250353.03_03 success 2403081156529-351655.03_03 success 2403081156529-351656.03_03 success 1436187396849-250351.01_00 success 
``````

ステータスフィールドに期待される値は次のとおりです。

**成功**：プロファイルが更新されました。 プロファイルが見つからなかった場合は、バッチの値を使用して作成されます。
**エラー**：失敗、例外、メッセージが失われたので、プロファイルは更新も作成もされませんでした。
**保留中**：プロファイルはまだ更新も作成もされていません。



