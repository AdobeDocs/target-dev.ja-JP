---
title: Adobe Target Profiles API
description: Adobe Target Profile API を使用して訪問者データをに送信する方法を説明します。 [!DNL Target].
contributors: https://github.com/icaraps
feature: APIs/SDKs
source-git-commit: 289299a52e5611c0da341f313aa4a447fcf3666a
workflow-type: tm+mt
source-wordcount: '149'
ht-degree: 1%

---

# [!DNL Adobe Target Profiles API] 概要

[!DNL Adobe Target] は、個々のユーザーごとにプロファイルを作成し、維持します。 このプロファイルは、 [!DNL Target] エッジクラスターに格納され、訪問のたびにリアルタイムで更新されます。

## プロファイル [!DNL Postman] コレクション

[!DNL Postman] は、API 呼び出しを簡単に実行できるアプリケーションです。 この [!DNL Postman] コレクションにすべてを含む [!DNL Profile API] 呼び出し。 クリック [Postmanで実行](https://www.getpostman.com/collections/ec7376f9028977ccaa99){target=_blank} をクリックして、Profile API コレクションを読み込みます。

## 従来のプロファイル API ドキュメントです。

従来のプロファイル API のドキュメントについては、次の場所を参照してください。 [https://developers.adobetarget.com/api/#profiles](https://developers.adobetarget.com/api/#profiles){target=_blank}

## の構造 [!DNL Target] profile

Target プロファイルは、次のオブジェクトで構成されます。

| オブジェクト | 詳細 |
| --- | --- |
| `clientcode` | The [!DNL Target] プロファイルが関連付けられるクライアントコード。 |
| `visitorId` | プロファイルの識別子。 これは、 `tntid`, `thirdpartyid`または `marketingcloudvisitorid`. |
| `modifiedAt` | プロファイルが最後に更新された日時のタイムスタンプ。 |
| `profileAttributes` | 個々のプロファイルに対するキーと値のペアとして保存されているすべてのプロファイル属性のリスト。 |

### サンプルプロファイル構造

```
{
    "client": "<your-tenant-name>",
    "visitorId": "a1-mbox3rdPartyId",
    "modifiedAt": "2017-08-18T17:53:39.003-04:00",
    "profileAttributes": {
        "insurance": {
            "value": "false",
            "modifiedAt": "2017-07-31T20:34:55.625-04:00"
        },
        "country": {
            "value": "france",
            "modifiedAt": "2017-07-31T14:26:30.879-04:00"
        },
        "checking": {
            "value": "true",
            "modifiedAt": "2017-07-31T20:34:55.625-04:00"
        },
        "user.memberlevel": {
            "value": "0.0",
            "modifiedAt": "2017-08-09T18:18:04.661-04:00"
        },
        "param1": {
            "value": "value1",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "param2": {
            "value": "value2",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "firstSessionStart": {
            "value": "1500648715286",
            "modifiedAt": "2017-07-21T10:51:55.286-04:00"
        },
        "entity.name": {
            "value": "my-entityName",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        },
        "entity.id": {
            "value": "my-entityId",
            "modifiedAt": "2017-08-09T18:18:04.659-04:00"
        }
    }
}
```
