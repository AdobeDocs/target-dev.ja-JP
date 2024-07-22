---
title: ' [!DNL Adobe Target] Java SDK へのプロキシ設定の実装'
description: ' [!DNL Adobe Target] Java SDK で TargetClient プロキシ設定を行う方法を説明します。'
feature: APIs/SDKs
exl-id: 32e8277d-3bba-4621-b9c7-3a49ac48a466
source-git-commit: 59ab3f53e2efcbb9f7b1b2073060bbd6a173e380
workflow-type: tm+mt
source-wordcount: '170'
ht-degree: 1%

---

# プロキシ設定（Java）

## 基本プロキシ

SDK を実行しているアプリケーションがインターネットへのアクセスにプロキシを必要とする場合、次のようにプロキシ設定を使用して `TargetClient` を設定する必要があります。

### 基本的なプロキシ設定

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## Authentication

プロキシ認証が必要な場合、次の例に示すように、資格情報をパラメーターとして `ClientProxyConfig` コンストラクターに渡すことができます。 これは、単純なユーザー名/パスワードプロキシ認証でのみ機能することに注意してください。

### 基本プロキシ認証

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port,username,password))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## オンデバイス判定

ルールアーティファクトを取得するリクエストの場合、応答をキャッシュしないようにプロキシを設定する必要があります。 ただし、そのリクエストに対してプロキシのキャッシュメカニズムを設定できない場合は、設定オプションを使用してプロキシレベルのキャッシュを回避します。 この回避策は、空の文字列値を持つ `Authorization` ヘッダーをルールリクエストに追加します。これは、応答をキャッシュできないことをプロキシに示す必要があります。

この回避策を有効にするには、次の設定を行います。

```java {line-numbers="true"}
ClientConfig.builder()
    .shouldArtifactRequestBypassProxyCache(true)
    .build();
```


