---
title: Adobe Target Profiles API
description: Adobe Target Profile API を使用して訪問者データを  [!DNL Target] に送信する方法を説明します。
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: 480cbbbe-4822-48c3-80d4-53628dee57b0
source-git-commit: e2462d12cf58ab5a588c13a96df5e6abafb9d675
workflow-type: tm+mt
source-wordcount: '104'
ht-degree: 1%

---

# [!DNL Adobe Target Profiles API] の概要

[!DNL Adobe Target] は、個々のユーザーごとにプロファイルを作成および管理します。 このプロファイルは [!DNL Target] エッジクラスターに保存され、訪問のたびにリアルタイムで更新されます。

## [!DNL Target] プロファイルの構造

Target プロファイルは、次のオブジェクトで構成されます。

| オブジェクト | 詳細 |
| --- | --- |
| `clientcode` | プロファイルが関連付けられている [!DNL Target] クライアントコード。 |
| `visitorId` | プロファイルの識別子。 `tntid`、`thirdpartyid` または `marketingcloudvisitorid` を指定できます。 |
| `modifiedAt` | プロファイルが最後に更新されたときのタイムスタンプ。 |
| `profileAttributes` | 個々のプロファイルにキーと値のペアとして保存されているすべてのプロファイル属性のリスト。 |

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
