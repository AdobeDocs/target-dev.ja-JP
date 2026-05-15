---
title: Adobe Target Profiles API
description: Adobe Target Profile APIを使用して訪問者データを [!DNL Target]に送信する方法を説明します。
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: 480cbbbe-4822-48c3-80d4-53628dee57b0
TQID: https://experienceleague.adobe.com/zmA1vGqBRL8gD-cFjJC0idogx7UF1e22iLEfNT9OHZ4
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 105
ht-degree: 2%

---

# [!DNL Adobe Target Profiles API] の概要

[!DNL Adobe Target]は、個々のユーザーごとにプロファイルを作成および管理します。 このプロファイルは[!DNL Target] エッジクラスターに保存され、訪問ごとにリアルタイムで更新されます。

## [!DNL Target] プロファイルの構造

ターゲットプロファイルは、次のオブジェクトで構成されます。

| オブジェクト | 詳細 |
| --- | --- |
| `clientcode` | プロファイルが関連付けられている[!DNL Target] クライアント コード。 |
| `visitorId` | プロファイルの識別子。 これは、`tntid`、`thirdpartyid`または`marketingcloudvisitorid`です。 |
| `modifiedAt` | プロファイルが最後に更新されたときのタイムスタンプ。 |
| `profileAttributes` | 個々のプロファイルにキーと値のペアとして保存されているすべてのプロファイル属性のリスト。 |

### プロファイル構造の例

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
