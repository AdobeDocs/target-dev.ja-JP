---
title: Java SDK のインストール
description: ' [!DNL Adobe Target] Java SDK のインストール方法を説明します。'
feature: APIs/SDKs
exl-id: 5828d5b3-c487-49bf-9458-7ef94374e32d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '46'
ht-degree: 0%

---

# Java SDK のインストール

Java SDK は、[Maven Central](https://search.maven.org/artifact/com.adobe.target/target-java-sdk) によって配布されます。 開始するには、`gradle` または `maven` にをインストールして、依存関係として追加します。

>[!BEGINTABS]

>[!TAB Gradle]

```javascript {line-numbers="true"}
compile 'com.adobe.target:java-sdk:1.0'
```

>[!TAB Maven]

```javascript {line-numbers="true"}
<dependency>
    <groupId>com.adobe.target</groupId>
    <artifactId>java-sdk</artifactId>
    <version>2.0</version>
</dependency>
```

>[!ENDTABS]

オープンソースコードは、<https://github.com/adobe/target-java-sdk> にあります。
