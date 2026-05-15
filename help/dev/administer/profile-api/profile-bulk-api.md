---
title: Adobe Target Bulk Profile Update API
description: ' [!DNL Adobe Target] [!UICONTROL Bulk Profile Update API]を使用して、ターゲティングで使用するために複数の訪問者のプロファイルデータを [!DNL Target] に送信する方法について説明します。'
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 0f38d109-5273-4f73-9488-80eca115d44d
TQID: https://experienceleague.adobe.com/EVlP71oFI-NIFoTe9fyx2Xzsr9v-sZq0JGdpti1XI64
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1063
ht-degree: 7%

---

# [!DNL Adobe Target Bulk Profile Update API]

[!DNL Adobe Target] [!UICONTROL Bulk Profile Update API]では、バッチファイルを使用して、複数の訪問者のユーザープロファイルをWeb サイトに一括で更新できます。

[!UICONTROL Bulk Profile Update API]を使用すると、多くのユーザーのプロファイルパラメーターの形式で詳細な訪問者プロファイルデータを任意の外部ソースから[!DNL Target]に簡単に送信できます。 外部ソースには、顧客関係管理（CRM）システムや販売時点情報管理（POS）システムなどがあります。これらのシステムは、web ページでは通常は利用できません。

| バージョン | URLの例 | 機能 |
| --- | --- | --- |
| v1 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/profile/batchUpdate` | プロファイルの一括更新のみサポートします。 |
| v2 | `http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate` | <ul><li>見つからない場合はプロファイルを作成します。</li><li>行ごとのステータス更新。</li></ul> |

>[!NOTE]
>
>[!DNL Bulk Profile Update API]のバージョン 2 （v2）は現在のバージョンです。 ただし、[!DNL Target]は引き続きバージョン 1 （v1）をサポートします。
>
>* [!DNL Target]実装で[!DNL Experience Cloud ID] （ECID）を匿名訪問者のプロファイル IDの1つとして使用している場合は、バージョン 2 （v2）のバッチファイルで`pcId`をキーとして使用しないでください。 `pcId`を[!DNL Bulk Profile Update API]のv2と共に使用することは、ECIDに依存しないスタンドアロン [!DNL Target]実装のみを目的としています。
>
>* 実装でプロファイル識別にECIDを使用し、バッチファイルのキーとして`pcId`を使用する場合は、APIのバージョン 1 （v1）を使用します。
>
>* 実装でプロファイル識別に`thirdPartyId`を使用する場合は、`thirdPartyId`をキーとしてAPIのバージョン 2 （v2）を使用します。

## [!UICONTROL Bulk Profile Update API]の利点

* プロファイル属性の数に上限がありません。
* サイト経由で送信されたプロファイル属性は、APIやその逆の方法で更新できます。

## 注意事項

* バッチファイルの容量は 50 MB 未満にする必要があります。 また、1 回にアップロードできる行数は 50 万行までです。
* 更新は通常1時間以内に行われますが、反映されるまでに24時間かかる場合があります。
* 以降のバッチで24時間にわたってアップロードできる行数に制限はありません。 ただし、他のプロセスを効率的に実行するために、営業時間中は取り込みプロセスが調整される場合があります。
* 同じthirdPartyIdの間にmbox呼び出しがない連続したv2 バッチアップデート呼び出しは、最初のバッチアップデート呼び出しで更新されたプロパティを上書きします。
* [!DNL Adobe]は、バッチプロファイルデータの100%がTargetにオンボーディングされて保持され、ターゲティングで使用できることを保証しません。 現在の設計では、少数のデータ（大規模な生産バッチの最大0.1%）がオンボーディングまたは保持されない可能性があります。

## バッチファイル

プロファイルデータを一括更新するには、バッチファイルを作成します。 バッチファイルは、次のサンプルファイルと同様に、値をコンマで区切ったテキストファイルです。

```
batch=pcId,param1,param2,param3,param4
123,value1
124,value1,,,value4
125,,value2
126,value1,value2,value3,value4
```

>[!NOTE]
>
>`batch=` パラメーターが必要です。ファイルの先頭で指定する必要があります。

POST呼び出しでこのファイルを参照して、[!DNL Target] サーバーにファイルを処理します。 バッチファイルを作成する際は、次の点を考慮してください。

* ファイルの最初の行では、列ヘッダーを指定する必要があります。
* 最初のヘッダーは`pcId`または`thirdPartyId`のいずれかである必要があります。 [!UICONTROL Marketing Cloud visitor ID]はサポートされていません。 [!UICONTROL pcId]は[!DNL Target]が生成した訪問者IDです。 `thirdPartyId`はクライアントアプリケーションで指定されたIDで、mbox呼び出しを通じて[!DNL Target]に`mbox3rdPartyId`として渡されます。 ここで`thirdPartyId`と呼ぶ必要があります。
* バッチファイルで指定するパラメーターと値は、セキュリティ上の理由から、UTF-8を使用してURL エンコードする必要があります。 パラメーターと値は、HTTP リクエストを通じて処理するために他のエッジノードに転送できます。
* パラメーターの形式は`paramName`のみにする必要があります。 パラメーターは[!DNL Target]に`profile.paramName`として表示されます。
* [!UICONTROL Bulk Profile Update API] v2を使用している場合は、各`pcId`にパラメーター値をすべて指定する必要はありません。 プロファイルは、[!DNL Target]に見つからない`pcId`または`mbox3rdPartyId`に対して作成されます。 v1を使用している場合、欠落しているpcIdまたはmbox3rdPartyIdのプロファイルは作成されません。 詳しくは、以下の [!DNL Bulk Profile Update API][&#128279;](#empty)の空の値の処理を参照してください。
* バッチファイルの容量は 50 MB 未満にする必要があります。 さらに、行の合計数は500,000を超えてはなりません。 この制限により、サーバーに多すぎるリクエストが溢れるのを防ぐことができます。
* アップロードできる属性の数に制限はありません。 ただし、顧客属性、プロファイル API、Mbox内プロファイルパラメーター、プロファイルスクリプト出力を含む外部プロファイルデータの合計サイズは、64 KBを超えてはなりません。
* パラメーターと値は、大文字と小文字を区別します。

## HTTP POST リクエスト

[!DNL Target] エッジサーバーにHTTP POST リクエストを送信して、ファイルを処理します。 次に、curl コマンドを使用したbatch.txt ファイルのHTTP POST リクエストの例を示します。

```
curl -X POST --data-binary @BATCH.TXT http://CLIENTCODE.tt.omtrdc.net/m2/CLIENTCODE/v2/profile/batchUpdate
```

要素の説明：

BATCH.TXTはファイル名です。 CLIENTCODEは[!DNL Target] クライアント コードです。

クライアントコードがわからない場合は、[!DNL Target] ユーザーインターフェイスで&#x200B;**[!UICONTROL Administration]** > **[!UICONTROL Implementation]**&#x200B;をクリックします。 クライアントコードは、[!UICONTROL Account Details] セクションに表示されます。

### 応答を調べる

プロファイル APIは、「batchStatus」の下のリンクと共に、特定のバッチジョブの全体的なステータスを示す別のURLに処理用のバッチの送信ステータスを返します。

### API応答の例

次のコードは、Profiles API応答の例です。

```
<response>
    <success>true</success>
    <batchStatus>http://mboxedge45.tt.omtrdc.net/m2/demo/profile/batchStatus?batchId=demo-1701473848678-13029383</batchStatus>
    <message>Batch submitted for processing</message>
</response>
```

エラーが発生した場合、応答には`success=false`とエラーの詳細メッセージが含まれます。

### デフォルトのバッチステータス応答

上記の`batchStatus` URL リンクをクリックした場合のデフォルトの応答が成功すると、次のようになります。

```
<response><batchId>demo4-1701473848678-13029383</batchId><status>complete</status><batchSize>1</batchSize></response>
```

ステータスフィールドの期待値は次のとおりです。

| ステータス | 詳細 |
| --- | --- |
| [!UICONTROL complete] | プロファイルのバッチ更新リクエストが正常に完了しました。 |
| [!UICONTROL incomplete] | プロファイルのバッチ更新リクエストはまだ処理中で、完了していません。 |
| [!UICONTROL stuck] | プロファイルのバッチ更新リクエストが停止しているため完了できませんでした。 |

### 詳細なバッチステータス URL応答

上記の`batchStatus` URLにパラメーター`showDetails=true`を渡すことにより、より詳細な応答を取得できます。

次に例を示します。

```
http://mboxedge45.tt.omtrdc.net/m2/demo/profile/batchStatus?batchId=demo-1701473848678-13029383&showDetails=true
```

#### 詳細な回答

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

## [!DNL Bulk Profile Update API]での空の値の処理 {#empty}

[!DNL Target] [!DNL Bulk Profile Update API] （v1またはv2）を使用する場合は、システムが空のパラメーターまたは属性値をどのように処理するかを理解することが重要です。

### 期待される動作

既存のパラメーターまたは属性に空の値（&quot;&quot;&quot;、null、または不足しているフィールド）を送信しても、プロファイルストアでそれらの値がリセットまたは削除されません。 これは意図的なものです。

* **空の値は無視されます**: APIは、不要または意味のない更新を避けるために、処理中に空の値をフィルタリングします。

* **既存のデータを消去しません**: パラメーターに既に値がある場合、空の値を送信すると、値は変更されません。

* **空のみのバッチはスキップされます**: バッチに空またはnull値のみが含まれる場合、完全に無視され、更新は適用されません。

### 追加情報

この動作は、[!DNL Bulk Profile Update API]のv1とv2の両方に適用されます。

空の値を送信して属性をクリアまたは削除しようとすると、何の影響もありません。

明示的な属性削除のサポートは、APIの将来のバージョン（v3）で計画されていますが、現在は利用できません。
