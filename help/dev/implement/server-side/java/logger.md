---
title: を初期化します。 [!DNL Adobe Target] リクエストをログに記録する Java SDK
description: リクエストを [!DNL Adobe Target] Java SDK。
feature: APIs/SDKs
exl-id: 85d1a6ef-0b08-4948-8133-740b7d6141dd
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '124'
ht-degree: 4%

---

# ロガー (Java)

## 説明

条件 [SDK の初期化](initialize-sdk.md)、には複数のオプションがあります `ClientConfig` オブジェクトに書き込みます。このオブジェクトはリクエストをログに記録するために設定できます。

| オプション | 説明 |
| --- | --- |
| `logRequests` | リクエスト本文全体と応答本文全体をログに記録します。 |
| `logRequestStatus` | リクエストの URL、ステータス、応答時間を記録します。 |

[!DNL Target] Java SDK では、 `slf4j` ログ。 ロガーの実装 ( 例： `java.util.logging`, `logback`、および `log4j`. 参照： [http://www.slf4j.org/manual.html](http://www.slf4j.org/manual.html) を参照してください。 すべてのログは、 `debug`.

## 例

次を追加： `slf4j` 依存関係。

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

を有効にします。 `DEBUG` のログを作成し、リクエストのログフラグをマークします。

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

要求、応答、および応答時間がコンソールに表示されます。
