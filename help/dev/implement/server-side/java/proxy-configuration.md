---
title: でのプロキシ設定の実装 [!DNL Adobe Target] Java SDK
description: TargetClient プロキシを設定する方法については、 [!DNL Adobe Target] Java SDK。
feature: APIs/SDKs
exl-id: 32e8277d-3bba-4621-b9c7-3a49ac48a466
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '88'
ht-degree: 1%

---

# プロキシ設定 (Java)

## 基本プロキシ

SDK を実行するアプリケーションがプロキシでインターネットにアクセスする必要がある場合、 `TargetClient` は、次のようにプロキシ設定で設定する必要があります。

### 基本プロキシ設定

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## Authentication

プロキシ認証が必要な場合は、資格情報をパラメーターとしてに渡すことができます。 `ClientProxyConfig` コンストラクタ。次の例のとおりです。 これは、単純なユーザー名/パスワードプロキシ認証に対してのみ機能します。

### 基本プロキシ認証

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port,username,password))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```
