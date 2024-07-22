---
title: リクエストをログに記録する  [!DNL Adobe Target] Java SDK の初期化
description: Java SDK でリクエストをログに記録する方法  [!DNL Adobe Target]  説明します。
feature: APIs/SDKs
exl-id: 85d1a6ef-0b08-4948-8133-740b7d6141dd
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '118'
ht-degree: 4%

---

# ロガー（Java）

## 説明

[SDK の初期化 ](initialize-sdk.md) 時に、`ClientConfig` オブジェクトにはいくつかのオプションがあり、リクエストをログに記録するように設定できます。

| オプション | 説明 |
| --- | --- |
| `logRequests` | リクエスト本文全体と応答本文をログに記録します。 |
| `logRequestStatus` | リクエストの url、ステータスおよび応答時間をログに記録します。 |

Java SDK[!DNL Target]、`slf4j` ログを使用します。 `java.util.logging`、`logback`、`log4j` などのロガーを実装する必要があります。 詳しくは、[http://www.slf4j.org/manual.html](http://www.slf4j.org/manual.html) を参照してください。 すべてのログが `debug` に印刷されます。

## 例

`slf4j` 依存関係を追加します。

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

実装に基づいて `DEBUG` ログを有効にし、リクエストログフラグをマークします。

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

リクエスト、応答および応答時間がコンソールに表示されます。
