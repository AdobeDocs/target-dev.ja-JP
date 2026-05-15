---
title: Adobe Target Admin APIの概要
description: ' [!DNL Adobe Target Admin API]の概要'
exl-id: 1168d376-c95b-4c5a-b7a2-c7815799a787
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/pJIaDbvs5sAFD8KPsnaNAMQAoq-lowmLs-B0zRAGzDY
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1400
ht-degree: 2%

---

# Target Admin APIの概要

この記事では、[!DNL Adobe Target Admin API]sを理解して使用するために必要な背景情報の概要について説明します。 次のコンテンツは、[!DNL Adobe Target Admin API]sに対して[認証](../configure-authentication.md)を設定する方法を理解していることを前提としています。

>[!NOTE]
>
>UIを使用して[!DNL Target]を管理する場合は、*Adobe Target Business Practitioner Guide*[&#128279;](https://experienceleague.adobe.com/docs/target/using/administer/administrating-target.html?lang=ja)の管理セクションを参照してください。
>
>Admin APIとProfile APIは、多くの場合、まとめて参照されますが（「Admin and Profile API」）、個別に参照することもできます（「Admin API」と「Profile API」）。 Recommendations APIは、[!DNL Target]管理APIの特定の実装です。

## 始める前に

[管理者API](../../administer/admin-api/admin-api-overview-new.md)に提供されるすべてのコード例では、{tenant}をテナント値、`your-bearer-token`をJWTで生成したアクセストークン、および`your-api-key`を[Adobe Developer Console](https://developer.adobe.com/console/home)のAPI キーで置き換えます。 テナントとJWTについて詳しくは、[Adobe [!DNL Target]管理者APIの認証](../configure-authentication.md)を設定する方法に関する記事を参照してください。

## バージョン管理

すべてのAPIには関連するバージョンがあります。 使用するAPIの適切なバージョンを提供することが重要です。

リクエストにペイロード（POSTまたはPUT）が含まれている場合、リクエストの`Content-Type` ヘッダーを使用してバージョンを指定します。

リクエストにペイロード（GET、DELETEまたはOPTIONS）が含まれていない場合は、`Accept` ヘッダーを使用してバージョンを指定します。

バージョンが指定されていない場合、呼び出しはデフォルトでV1 （application/vnd.adobe.target.v1+json）になります。

>[!NOTE]
>
>正しいバージョンが指定されていない場合（例えば、V2 ペイロードを使用しているがContent-Type ヘッダーを指定できない場合）、APIが下位互換性がない場合、APIはサポートされていないエラーで応答します。

サポートされていない機能のエラーメッセージ

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

Postmanは、API呼び出しを簡単に実行できるアプリケーションです。 この[Target管理API Postman コレクション &#x200B;](https://developers.adobetarget.com/api/#admin-postman-collection)には、アクティビティ、オーディエンス、オファー、レポート、Mbox、および環境を使用して認証を必要とするすべてのTarget管理API呼び出しが含まれています

## 応答コード

Target Admin APIの一般的な応答コードを次に示します。

| ステータス | 意味 | 説明 |
| --- | --- | --- |
| 200 | [OK](https://www.rfc-editor.org/rfc/rfc7231#section-6.3.1) | OK |
| 400 | [不正なリクエスト &#x200B;](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.1) | リクエストが正しくありません。 おそらく、リクエストで提供されたデータが無効です。 |
| 401 | [未承認](https://www.rfc-editor.org/rfc/rfc7235#section-3.1) | ユーザーはこの操作を実行できません。 |
| 403 | [禁止](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.3) | このリソースへのアクセスは禁止されています。 |
| 404 | [見つかりません](https://www.rfc-editor.org/rfc/rfc7231#section-6.5.4) | 参照されているリソースが見つかりませんでした。 |

## アクティビティ

アクティビティを使用すると、ユーザーのコンテンツをテストまたはパーソナライズできます。 アクティビティは、次のいずれかのタイプになります。

* [A/B](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=ja)
* [エクスペリエンスのターゲット設定（XT）](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=ja)
* [レコメンデーション](https://experienceleague.adobe.com/docs/target/using/activities/recommendations-activity.html?lang=ja)
* [Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=ja)
* [多変量分析テスト（MVT）](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html?lang=ja)

## 更新のバッチ

複数の管理APIを1つのバッチリクエストとして実行できます。

### バッチで呼び出しを実行

`POST /{tenant}/target/batch`

複数のAPI呼び出しを積み重ねて、1つのバッチで実行します。

バッチ処理を使用すると、1つのHTTP リクエストで複数の操作の命令を渡すことができます。 関連する操作の依存関係を指定することもできます（以下の節で説明します）。 TNTは、独立した各操作を（場合によっては並行して）処理し、依存する操作を順次処理します。 すべての操作が完了すると、統合応答が渡され、HTTP接続が閉じられます。

バッチ APIは、JSON配列として表される論理HTTP リクエストの配列を取り込みます。各リクエストには、メソッド（HTTP メソッド GET/PUT/POST/DELETEなどに対応）、relativeUrl （admin/rest/の後のURLの部分）、オプションのヘッダー配列（HTTP ヘッダーに対応）、オプションの本文（POSTおよびPUT リクエスト用）があります。 Batch APIは、JSON配列として表される論理HTTP応答の配列を返します。各応答には、ステータスコード、オプションのヘッダー配列、オプションの本文（JSON エンコードされた文字列）が含まれます。 バッチ処理リクエストに、実行する個々の操作を記述したJSON オブジェクトを構築させる。 許可される最大操作の数は256です（0から255まで）。

リクエスト内の操作の依存関係の指定デフォルトでは、バッチ API リクエストで指定された操作は独立しています。サーバー上で任意の順序で実行でき、1つの操作のエラーは他の操作の実行には影響しません。

多くの場合、リクエスト内の操作は依存しています。たとえば、1つの操作の出力を次の操作の入力に使用できます。 例えば、operationId=0で作成されたオファーは、キャンペーン作成operationId=1で使用する必要があります。

2つのバッチ操作をリンクするには、依存操作で必要な操作のIDを指定します（例：「dependsOnOperationId」 : 5）。 また、バッチ操作のPOST リクエストを介して作成されたリソースのIDは、「relativeUrl」と「body」の両方で依存操作で使用できます。

#### 権限とスロットル

バッチ API アクションを実行するには、基盤となるユーザーに少なくとも「エディター」権限が必要です（ユーザーが個別の操作を持つよりも追加の権限が必要な場合は、個々の操作が失敗します）。 通常のスロットル戦略は、すべての操作が個別に実行されたかのように、バッチ API アクションに適用されます。

バッチ処理は、すべての操作が完了した場合、操作が成功（2xx statusCode）、失敗（4xx、5xx ステータスコード）、または依存関係の操作が失敗したかスキップされたためにスキップされる可能性があります。

#### リクエストオブジェクトパラメーター

| 属性 | 説明 | 制限 | デフォルト |
| --- | --- | --- | --- |
| 本文 | HTTP バッチ操作の本文。 POSTとPUT以外のすべてのアクションでは無視されます。 以前のバッチアクションのIDを参照できます。例：「offerId」:「{operationIdResponse:0}」、「segmentId」:「{operationIdResponse:1}」 | operationIdResponseを参照する場合、参照されるoperationId応答は有効なIDであり、そのアクションのメソッドはPOSTである必要があります | 空のオブジェクト {} |
| dependsOnOperationIds | 指定された操作が正常に完了した場合にのみ、現在の操作が実行されることを保証する制約IDのリスト。 操作の連鎖を実現するために使用できます。 | 最大255個の操作が許可され、一意の値のみが許可されます。配列の有効なoperationIdを指す必要があります。循環依存関係は許可されません |  |
| ヘッダー | 特定の操作で送信されるキー値ヘッダーの配列。 バッチ APIの認証が認証ヘッダーを介して実行された場合、個々の操作でもコピーされます。 | 許可される配列のヘッダーの最大数は50です | コンテンツタイプ：application/json |
| headers->name | ヘッダー名 | 他のヘッダー名と一意である必要があります。 ヘッダーではrfcで大文字と小文字が区別されないため、値は互いに上書きされます。 |  |
| headers->value | ヘッダー値 | 該当なし | 空の文字列 |
| method | 使用するHTTP メソッド。 利用可能なオプション：GET、POST、PUT、PATCH、DELETE | GET、POST、PUT、PATCH、DELETE メソッドのみが許可されます |  |
| operationId | 応答と結果の参照のために、他の操作の中から操作を識別するために使用される操作ID。 | その他の操作では一意。0 ～ 255の値 |  |
| 業務運営 | バッチで実行する操作のリスト。 注文は関係ありません。 | 最大256個の操作が許可されています |  |
| relativeUrl | admin rest APIの相対URL。「/admin/rest/」の後の部分。 次のようなクエリ文字列パラメーターを含めることができます。「/v2/campaigns?limit=10&amp;offset=10」 以前のバッチアクションのIDを含むURLを参照できます（例：「/v1/offers/{operationIdResponse:0}」）。 クエリパラメーターが送信される場合は、URL エンコードする必要があります。 | /で始まる必要があります。新しい有効なJSON APIのみがサポートされます。無効なrelativeURLの場合、特定の操作に対する404応答が返されます。operationIdResponseを参照する場合、参照されるoperationId応答は有効なIDで、そのアクションのメソッドはPOSTである必要があります |  |

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
| operationId | 他の操作のうち、POST リクエストで送信されたIDと同じ操作を識別するために使用される操作ID。 |
| スキップしました | 操作が実行されたかスキップされたかをマークするブール型フラグ。 現在の操作が失敗した操作に依存する場合はtrueになります（2xxとは異なるstatusCode値を返します）。 |
| statusCode | すべての依存する操作はスキップされます（実行されません）。 |
| ヘッダー | 特定の操作の応答として送信されるキー値ヘッダーの配列。 |
| headers->name | ヘッダー名 |
| headers->value | ヘッダー値 |
| 本文 | http バッチ応答操作の本文 |

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
