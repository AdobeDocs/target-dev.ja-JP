---
title: Adobe Target管理 API の概要
description: の概要  [!DNL Adobe Target Admin API]
exl-id: 1168d376-c95b-4c5a-b7a2-c7815799a787
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1312'
ht-degree: 2%

---

# Target 管理 API の概要

この記事では、[!DNL Adobe Target Admin API] を正常に理解して使用するために必要な背景情報の概要を説明します。 次のコンテンツは、[!DNL Adobe Target Admin API]s の [ 認証を設定 ](../configure-authentication.md) する方法を理解していることを前提としています。

>[!NOTE]
>
>UI を使用して [!DNL Target] を管理する場合は、*Adobe Target ビジネス実践者ガイドの [ 管理*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=ja) の節を参照してください。
>
>管理 API とプロファイル API は多くの場合、「管理 API とプロファイル API」と総称されますが、別に「管理 API」と「プロファイル API」とも呼ばれることもあります。 Recommendations API は、[!DNL Target] 管理 API の具体的な実装です。

## 始める前に

[ 管理 API](../../administer/admin-api/admin-api-overview-new.md) に用意されているすべてのコード例で、{tenant} をテナント値に、`your-bearer-token` を JWT で生成するアクセストークンに、[Adobe Developer Console](https://developer.adobe.com/console/home) の API キーに `your-api-key` します。 テナントと JWT について詳しくは、管理 API を使用したAdobeの [ 認証の設定 ](../configure-authentication.md) 方法に関する記事を参照し [!DNL Target] ください。

## バージョン管理

すべての API にはバージョンが関連付けられています。 使用する API の適切なバージョンを提供することが重要です。

リクエストにペイロード（POSTまたはPUT）が含まれる場合、リクエストの `Content-Type` ヘッダーを使用してバージョンを指定します。

リクエストにペイロード（GET、DELETE、OPTIONS）が含まれていない場合は、`Accept` ヘッダーを使用してバージョンを指定します。

バージョンが指定されていない場合、呼び出しはデフォルトで V1 （application/vnd.adobe.target.v1+json）に設定されます。

>[!NOTE]
>
>正しいバージョンが指定されない場合（例えば、V2 ペイロードを使用したが Content-Type ヘッダーを指定できなかった場合）、API に後方互換性がない場合は、API はサポートされていないエラーで応答します。

サポートされていない機能に関するエラーメッセージ

```
{
    "httpStatus": 406,
    "requestId": "8752b736-cf71-4d81-86c3-94be2b5ae648",
    "requestTime": "2018-02-02T21:39:06.405Z",
    "errors": [
        {
            "errorCode": "Unsupported.Feature",
            "message": "Unsupported features detected"
        }
    ]
}
```

Admin Postman Collection

Postmanは、API 呼び出しを簡単に実行できるアプリケーションです。 この [Target 管理 API Postman コレクション ](https://developers.adobetarget.com/api/#admin-postman-collection) には、アクティビティ、オーディエンス、オファー、レポート、Mbox および環境を使用した認証を必要とするすべての Target 管理 API 呼び出しが含まれています

## 応答コード

Target 管理 API の一般的な応答コードを次に示します。

| ステータス | 意味 | 説明 |
| --- | --- | --- |
| 200 | [OK](https://www.rfc-editor.org/rfc/rfc7231#section-6.3.1) | OK |  |
| 400 | [ 無効なリクエスト ](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.1) | リクエストが正しくありません。 おそらく、リクエストで指定されたデータは無効です。 |  |
| 401 | [ 未認証 ](https://www.rfc-editor.org/rfc/rfc7235#section-3.1) | ユーザーはこの操作を実行できません。 |  |
| 403 | [ 禁止 ](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.3) | このリソースへのアクセスは禁止されています。 |  |
| 404 | [ 見つかりません ](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.4) | 参照されたリソースが見つかりませんでした。 |  |

## アクティビティ

アクティビティを使用すると、ユーザー向けにコンテンツをテストまたはパーソナライズできます。 アクティビティは、次のいずれかのタイプになります。

* [A/B](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=ja)
* [エクスペリエンスのターゲット設定（XT）](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=ja)
* [Recommendations](https://experienceleague.adobe.com/docs/target/using/activities/recommendations-activity.html?lang=ja)
* [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=ja)
* [ 多変量分析テスト（MVT） ](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html?lang=ja)

## バッチ更新

複数の管理 API を単一のバッチリクエストとして実行できます。

### 呼び出しをバッチで実行

`POST /{tenant}/target/batch`

複数の API 呼び出しをスタックし、1 つのバッチで実行する。

バッチ処理を使用すると、1 回の HTTP リクエストで複数の操作の命令を渡すことができます。 また、関連する操作間の依存関係も指定できます（以下の節で説明します）。 TNT は、独立した各操作（場合によっては並行）を処理し、独立した操作を順番に処理します。 すべての操作が完了すると、統合応答が返され、HTTP 接続が閉じられます。

バッチ API は、JSON 配列として表された論理 HTTP リクエストの配列を取り込みます。各リクエストには、メソッド（HTTP メソッドのGET/PUT/POST/DELETEなどに対応）、relativeUrl （admin/rest/の後の URL の部分）、オプションのヘッダー配列（HTTP ヘッダーに対応）、オプションの本文（POSTリクエストとPUTリクエスト用）があります。 バッチ API は、JSON 配列で表された論理 HTTP 応答の配列を返します。各応答には、ステータスコード、オプションのヘッダー配列およびオプションの本文（JSON エンコードされた文字列）が含まれます。 バッチリクエストを実行するには、実行する個々の操作を記述する JSON オブジェクトを作成します。 許可される最大操作数は 256 個（0 ～ 255）です。

リクエスト内の操作間の依存関係の指定デフォルトでは、バッチ API リクエストで指定された操作は独立しています。これらはサーバー上で任意の順序で実行でき、1 つの操作のエラーは他の操作の実行に影響を与えません。

多くの場合、リクエスト内の操作は依存します。例えば、ある操作の出力を次の操作の入力に使用できます。 例えば、operationId=0 で作成されたオファーは、キャンペーン作成 operationId=1 で使用する必要があります。

2 つのバッチ操作をリンクするには、必要な操作の ID を依存操作で指定します（例：&quot;dependsOnOperationId&quot; : 5）。 また、バッチ操作のPOSTリクエストを介して作成されたリソースの ID は、「relativeUrl」と「body」の両方の依存する操作で使用できます。

#### 権限とスロットル

バッチ API アクションを実行するには、基になるユーザーは少なくとも「エディター」権限を持っている必要があります（ユーザーの権限よりも追加の権限が必要な場合は、個々の操作が失敗します）。 通常のスロットル戦略は、すべての操作が個別に実行されたかのようにバッチ API アクションに適用されます。

バッチ処理は、すべての操作が完了すると終了します。操作は、成功（2xx statusCode）、失敗（4xx、5xx ステータスコード）、または依存関係操作が失敗したかスキップされたためにスキップされます。

#### リクエストオブジェクトパラメーター

| 属性 | 説明 | 制限 | デフォルト |
| --- | --- | --- | --- |
| 本文 | http バッチ操作の本文。 は、POSTとPUT以外のすべてのアクションで無視されます。 以前のバッチアクションからの ID を参照できます（例：&quot;offerId&quot;: &quot;{operationIdResponse:0}&quot;, &quot;segmentId&quot;: &quot;{operationIdResponse:1}&quot;） | 有効な JSON である必要があります。operationIdResponse を参照する場合、参照される operationId 応答は有効な ID であり、そのアクションのメソッドはPOSTである必要があります | 空のオブジェクト {} |  |
| dependsOnOperationIds | 指定した操作が正常に完了した場合にのみ現在の操作が実行されることを保証する制約 ID のリスト。 連結する操作に使用できます。 | 最大 255 個の操作が許可されます。一意の値のみが許可されます。配列の有効な operationId を指す必要があります。循環依存関係は許可されていません |  |  |
| ヘッダー | 特定の操作で送信されるキー値ヘッダーの配列。 バッチ API の認証が Authorization ヘッダーを使用して実行された場合、個々の操作に対してもコピーされます。 | 許可されている配列のヘッダーの最大数は 50 です | Content-Type: application/json |  |
| headers->name | ヘッダー名 | は、他のヘッダー名の中で一意である必要があります。 ヘッダーは、rfc では大文字と小文字が区別されません。そうでない場合は、値が互いに上書きされます。 |  |  |
| headers->value | ヘッダー値 | 該当なし | 空文字列 |  |
| method | 使用する HTTP メソッド。 使用可能なオプション：GET、POST、PUT、PATCH、DELETE | GET、POST、PUT、PATCH、DELETE方式のみを使用できます |  |  |
| operationId | 応答および結果の参照のために、他の操作の中から操作を識別するために使用される操作 ID。 | 他の操作の中でも一意で、0 ～ 255 の値 |  |  |
| の操作 | バッチで実行する操作のリスト。 オーダーは関係ありません。 | 最大 256 個の操作が許可されています |  |  |
| relativeUrl | 管理者 rest API の相対 URL、「/admin/rest/」の後の部分。 「/v2/campaigns?limit=10&amp;offset=10」などのクエリ文字列パラメーターを含めることができます。 は、以前のバッチアクションの ID を含んだ URL （例：「/v1/offers/{operationIdResponse:0}」）を参照できます。 クエリパラメーターを送信する場合は、URL エンコードする必要があります。 | /で始める必要があります（相対）。新しい有効な JSON API のみがサポートされます。無効な relativeURL の場合、特定のオペレーションに対する 404 レスポンスが返されます。operationIdResponse を参照する場合、参照される operationId レスポンスは有効な ID である必要があり、そのアクションのメソッドはPOSTである必要があります |  |  |

#### リクエストオブジェクトのサンプル

```
{
  "operations": [
    {
      "operationId": 1,
      "dependsOnOperationIds~": [0],
      "method": "POST",
      "relativeUrl": "/v1/offers",
      "headers~": [
        {
          "name": "Content-Type",
          "value": "application/json"
        }
      ],
      "body~": {
        "key": "value"
      }
    }
  ]
}
```

#### 応答オブジェクトパラメーター

| パラメーター | 説明 |
| --- | --- |
| operationId | オペレーション ID：他のオペレーションの中でオペレーションを識別するために使用されます。これは、POSTリクエストで送信された ID と同じです。 |  |
| スキップしました | 操作が実行またはスキップされたかどうかを示すブール値フラグ。 現在の操作が失敗した操作（2xx とは異なる statusCode 値を返した操作）に依存している場合、true になります。 |  |
| statusCode | が返された場合、依存するすべての操作はスキップされます（実行されません）。 |  |
| ヘッダー | 特定の操作に対する応答として送信されるキー値ヘッダーの配列。 |  |
| headers->name | ヘッダー名 |  |
| headers->value | ヘッダー値 |  |
| 本文 | http バッチ応答操作の本文 |  |

#### 応答オブジェクトのサンプル

```
{
  "results": [
    {
      "operationId": 1,
      "skipped~": false,
      "statusCode~": 200,
      "headers~": [
        {
          "name": "Content-Type",
          "value": "application/json; charset=UTF-8"
        }
      ],
      "body~": {
        "id": 5
      }
    }
  ]
}
```
