---
title: リクエストをログに記録するための [!DNL Adobe Target] Java SDKの初期化
description: ' [!DNL Adobe Target] Java SDKでリクエストをログに記録する方法を説明します。'
feature: APIs/SDKs
exl-id: 85d1a6ef-0b08-4948-8133-740b7d6141dd
TQID: https://experienceleague.adobe.com/xvduuV6cjVJu-yIoaxCvbPE-ZttfEViuM8B7sVczAC0
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 120
ht-degree: 4%

---

# ロガー（Java）

## 説明

SDK](initialize-sdk.md)を[初期化する際、`ClientConfig` オブジェクトには複数のオプションがあり、これはログリクエストに設定できます。

| オプション | 説明 |
| --- | --- |
| `logRequests` | リクエスト本文と応答本文をログに記録します。 |
| `logRequestStatus` | リクエストのURL、ステータス、応答時間を記録します。 |

[!DNL Target] Java SDKは`slf4j` ログを使用します。 `java.util.logging`、`logback`、`log4j`などのロガーの実装を指定する必要があります。 詳しくは、[https://www.slf4j.org/manual.html](https://www.slf4j.org/manual.html)を参照してください。 ログはすべて`debug`に印刷されます。

## 例

`slf4j`依存関係を追加します。

>[!BEGINTABS]

>[!TAB Gradle]

### Gradle

```javascript {line-numbers="true"}
compile 'org.slf4j:slf4j-simple:2.0.0-alpha0'
```

>[!TAB Maven]

```javascript {line-numbers="true"}
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-simple</artifactId>
    <version>2.0.0-alpha0</version>
</dependency>
```

>[!ENDTABS]

実装に基づいて`DEBUG` ログを有効にし、リクエストログのフラグをマークします。

### デバッグ

```javascript {line-numbers="true"}
System.setProperty(SimpleLogger.DEFAULT_LOG_LEVEL_KEY, "DEBUG");
ClientConfig config = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .logRequests(true)
        .logRequestStatus(true)
        .build();

TargetClient targetClient = TargetClient.create(config);
```

リクエスト、応答、応答時間がコンソールに表示されます。
