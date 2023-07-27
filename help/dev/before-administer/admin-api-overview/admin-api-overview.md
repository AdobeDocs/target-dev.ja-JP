---
title: Adobe Target Admin API の概要
description: の概要 [!DNL Adobe Target Admin API]
exl-id: 1168d376-c95b-4c5a-b7a2-c7815799a787
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1365'
ht-degree: 3%

---

# Target 管理 API の概要

この記事では、理解と使用に必要な背景情報の概要を説明します [!DNL Adobe Target Admin API]が正常に実行されました。 以下の内容は、 [認証の設定](../configure-authentication.md) 対象： [!DNL Adobe Target Admin API]s.

>[!NOTE]
>
>を管理する場合 [!DNL Target] UI を使用して、 [管理セクション *Adobe Target Business Practioner ガイド*](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=en).
>
>Admin API とプロファイル API は、多くの場合、まとめて（「管理 API」と「プロファイル API」）と呼ばれますが、別々に（「管理 API」と「プロファイル API」）と呼ぶこともできます。 Recommendations API は、 [!DNL Target] 管理 API。

## 始める前に

に示すすべてのコード例 [管理 API](../../administer/admin-api/admin-api-overview-new.md)，置換 {tenant} テナントの値と共に `your-bearer-token` を、JWT で生成し、 `your-api-key` を使用して、 [Adobe Developer Console](https://developer.adobe.com/console/home). テナントと JWT について詳しくは、 [認証の設定](../configure-authentication.md) Adobe [!DNL Target] 管理 API

## バージョン管理

すべての API には関連するバージョンがあります。 使用する API の適切なバージョンを提供することが重要です。

リクエストにペイロード (POSTまたはPUT) が含まれている場合、 `Content-Type` リクエストのヘッダーは、バージョンを指定するために使用されます。

リクエストにペイロード (GET、DELETEまたはOPTIONS) が含まれていない場合、 `Accept` ヘッダーは、バージョンを指定するために使用されます。

バージョンが指定されていない場合、呼び出しはデフォルトで V1(application/vnd.adobe.target.v1+json) に設定されます。

>[!NOTE]
>
>正しいバージョンが指定されていない場合（例えば、V2 ペイロードを使用していても Content-Type ヘッダーを指定できなかった場合）、API が後方互換性がない場合、API はサポートされていないエラーで応答します。

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

Postmanは、API 呼び出しを簡単に実行できるアプリケーションです。 この [Target 管理 API Postmanコレクション](https://developers.adobetarget.com/api/#admin-postman-collection) には、アクティビティ、オーディエンス、オファー、レポート、mbox、環境を使用した認証を必要とするすべての Target Admin API 呼び出しが含まれます。

## 応答コード

以下に、Target Admin API の一般的な応答コードを示します。

| ステータス | 意味 | 説明 |
| --- | --- | --- |
| 200 | [OK](https://www.rfc-editor.org/rfc/rfc7231#section-6.3.1) | OK |  |
| 400 | [Bad Request](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.1) | Bad Request. ほとんどの場合、リクエストで提供されたデータは無効です。 |  |
| 401 | [Unauthorized](https://www.rfc-editor.org/rfc/rfc7235#section-3.1) | ユーザーはこの操作を実行できません。 |  |
| 403 | [Forbidden](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.3) | このリソースへのアクセスは禁止されています。 |  |
| 404 | [Not Found](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.4) | 参照されたリソースが見つかりませんでした。 |  |

## アクティビティ

「 」アクティビティを使用すると、ユーザー向けのコンテンツをテストまたはパーソナライズできます。 アクティビティは、次のいずれかのタイプになります。

* [A / B](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html)
* [エクスペリエンスのターゲット設定（XT）](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html)
* [Recommendations](https://experienceleague.adobe.com/docs/target/using/activities/recommendations-activity.html)
* [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [多変量分析テスト （MVT）](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html)

## 一括更新

複数の管理 API を 1 つのバッチリクエストとして実行できます。

### バッチで呼び出しを実行

`POST /{tenant}/target/batch`

複数の API 呼び出しを積み重ねて、1 つのバッチで実行します。

バッチ処理を使用すると、1 回の HTTP リクエストで複数の操作の手順を渡すことができます。 関連する操作間の依存関係も指定できます（以下の節で説明）。 TNT は、独立した各操作（場合によっては並行して）を処理し、依存する操作を順番に処理します。 すべての操作が完了すると、統合された応答が返され、HTTP 接続が閉じられます。

バッチ API は、JSON 配列で表される論理 HTTP リクエストの配列を取り込みます。各リクエストには、メソッド (HTTP メソッドのGET、PUT、POST、DELETEなどに対応 )、relativeUrl（URL の admin/rest/後の部分）、オプションのヘッダー配列 (HTTP ヘッダーとPUTリクエスト用 ) が含まれます。 バッチ API は、JSON 配列として表される論理 HTTP 応答の配列を返します。各応答には、ステータスコード、オプションのヘッダー配列および本文（JSON エンコードされた文字列）があります。 バッチ処理されたリクエストをおこなうには、実行する個々の操作を記述する JSON オブジェクトを作成します。 許可される操作の最大数は 256 です (0 ～ 255)。

リクエスト内の操作間の依存関係の指定デフォルトでは、バッチ API リクエストで指定された操作は独立しています。サーバー上で任意の順序で実行でき、1 つの操作のエラーは他の操作の実行に影響しません。

多くの場合、リクエスト内の操作は依存します。例えば、1 つの操作の出力を次の操作の入力に使用できます。 例えば、operationId=0 で作成されたオファーは、キャンペーン作成 operationId=1 で使用する必要があります。

2 つのバッチ操作を相互にリンクするには、依存操作で必要な操作の ID を指定します（例：「dependsOnOperationId」 : 5）。 また、バッチ操作のPOSTリクエストを介して作成されたリソースの ID は、「relativeUrl」と「body」の両方の依存操作で使用できます。

#### 権限とスロットル

バッチ API アクションを実行するには、基になるユーザーに少なくとも「編集者」権限が必要です（ユーザーよりも追加の権限が必要な場合は、個々の操作ごとに個々の操作が失敗します）。 通常のスロットリング戦略は、すべての操作が個別に実行された場合と同様に、バッチ API アクションに適用されます。

すべての操作が完了すると、バッチ処理が完了し、操作が成功 (2xx statusCode)、失敗（4xx、5xx ステータスコード）になるか、依存関係操作が失敗したかスキップされたためにスキップされます。

#### リクエストオブジェクトパラメーター

| 属性 | 説明 | 制限 | デフォルト |
| --- | --- | --- | --- |
| body | HTTP バッチ操作の本文です。 は、POSTとPUTを除くすべてのアクションで無視されます。 は、以前のバッチアクションの ID を参照できます。例： &quot;offerId&quot;: &quot;{operationIdResponse:0}&quot;, &quot;segmentId&quot;: &quot;{operationIdResponse:1}&quot; | は有効な JSON である必要があります。operationIdResponse を参照する場合、参照される operationId 応答は有効な ID で、そのアクションに対するメソッドはPOSTである必要があります | 空のオブジェクト {} |  |
| dependsOnOperationIds | 指定した操作が正常に完了した場合にのみ現在の操作が実行されることを保証する制約 ID のリスト。 操作の連鎖を実現するために使用できます。 | 最大 255 個の操作が許可され、一意の値のみが許可されます。は配列内の有効な operationId を指す必要があります。循環依存関係は許可されません。 |  |  |
| headers | 特定の操作で送信するキーと値のヘッダーの配列。 バッチ API の認証が Authorization ヘッダー経由で実行された場合、個々の操作に対してもコピーされます。 | 配列内の最大ヘッダー数は 50 です | Content-Type: application/json |  |
| headers->name | ヘッダー名 | は、他のヘッダー名の中で一意である必要があります。 ヘッダーは rfc では大文字と小文字が区別されません。大文字と小文字が区別されない場合、値が相互に上書きされます。 |  |  |
| headers->value | ヘッダー値 | 該当なし | 空の文字列 |  |
| method | 使用する HTTP メソッド。 使用可能なオプション：GET、POST、PUT、PATCH、DELETE | GET、POST、PUT、PATCH、DELETEのメソッドのみ使用できます |  |  |
| operationId | 応答および参照結果のために、他の操作間で操作を識別するために使用される操作 ID。 | 他の操作の中で一意（0 ～ 255 の値） |  |  |
| 操作 | バッチで実行する操作のリスト。 注文は関連しません。 | 最大 256 個の操作が許可されます |  |  |
| relativeUrl | 管理 REST API の相対 URL（「/admin/rest/」の後の部分）。 &quot;/v2/campaigns?limit=10&amp;offset=10&quot;のようなクエリー文字列パラメーターを含めることができます。 は、以前のバッチアクションの ID を含む URL を参照できます。例： &quot;/v1/offers/{operationIdResponse:0}&quot;. クエリのパラメーターが送信される場合は、URL エンコードする必要があります。 | は/ （相対）で始まり、新しい有効な JSON API のみがサポートされます。無効な relativeURL の場合は特定の操作に対する 404 応答が返されます。operationIdResponse を参照する場合は、operationId 応答を有効な ID にし、そのアクションに対するPOSTを指定します。 |  |  |

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

#### 応答オブジェクトのパラメーター

| パラメーター | 説明 |
| --- | --- |
| operationId | 操作リクエストで送信されたのと同じ ID で、他の操作間の操作を識別するために使用されるPOSTID。 |  |
| スキップしました | 操作が実行またはスキップされたかどうかを示すブール値フラグ。 現在の操作が失敗した操作（2xx 以外の statusCode 値を返した）に依存している場合、true になります。 |  |
| statusCode | が返された場合、依存する操作はすべてスキップされます（実行されません）。 |  |
| headers | 特定の操作に対する応答として送信するキーと値のヘッダーの配列。 |  |
| headers->name | ヘッダー名 |  |
| headers->value | ヘッダー値 |  |
| body | HTTP バッチ応答操作の本文 |  |

#### レスポンスオブジェクトのサンプル

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
