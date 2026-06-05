---
title: ' [!DNL Adobe Target] Java SDKでのプロキシ設定の実装'
description: ' [!DNL Adobe Target] Java SDKでTargetClient プロキシ設定を設定する方法を説明します。'
feature: APIs/SDKs
exl-id: 32e8277d-3bba-4621-b9c7-3a49ac48a466
TQID: https://experienceleague.adobe.com/Vo8KrM-3AGIvoO-E-iAQcAPqzXE24BM30LX7ji5E2Nk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 170
ht-degree: 2%

---

# プロキシ設定（Java）

## 基本プロキシ

SDKを実行しているアプリケーションでインターネットにアクセスするためにプロキシが必要な場合は、次のようにプロキシ設定を使用して`TargetClient`を設定する必要があります。

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

プロキシ認証が必要な場合は、次の例に従って、資格情報をパラメーターとして`ClientProxyConfig` コンストラクターに渡すことができます。 これは、単純なユーザー名/パスワードプロキシ認証でのみ機能することに注意してください。

### 基本的なプロキシ認証

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .proxyConfig(new ClientProxyConfig(host,port,username,password))
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## オンデバイス判定

ルールのアーティファクトを取得するリクエストの場合は、応答をキャッシュしないようにプロキシを設定する必要があります。 ただし、そのリクエストに対するプロキシのキャッシュメカニズムを設定できない場合は、プロキシレベルのキャッシュをバイパスする回避策として設定オプションを使用します。 この回避策では、空の文字列値を持つ`Authorization` ヘッダーをルールリクエストに追加します。これは、応答をキャッシュしないことをプロキシに示す必要があります。

この回避策を有効にするには、次のように設定します。

```java {line-numbers="true"}
ClientConfig.builder()
    .shouldArtifactRequestBypassProxyCache(true)
    .build();
```


