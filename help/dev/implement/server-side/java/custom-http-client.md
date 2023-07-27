---
title: カスタム HTTP クライアントの設定方法を説明します
description: ClientConfig.builder().httpClient() を使用して TargetClient を設定する方法を説明します。
feature: APIs/SDKs
exl-id: 7615029c-b62d-4ed1-aadb-32e364c4c654
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '108'
ht-degree: 0%

---

# カスタム HTTP クライアント設定 (Java)

SDK を実行するアプリケーションにカスタム HTTP クライアントが必要な場合、SSL の設定やリクエストへのデフォルトヘッダーの追加などの機能を有効にするには、 `TargetClient` を使用して設定する必要があります `ClientConfig.builder().httpClient()`:

## 基本的なカスタム HTTP クライアント設定

SDK は現在、 `org.apache.http.client.HttpClient` インターフェイス。

### 基本的な実装

```java {line-numbers="true"}
CloseableHttpClient httpClient = HttpClients.custom().build();
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .httpClient(httpClient)
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## SSL 設定を使用したカスタム HTTP クライアント設定

次に、 `TargetClient` カスタマイズする `HttpClient` ～に渡される `ClientConfig`. 次のコードスニペットでは、 `org.apache.http.conn.ssl` SSL 設定用のパッケージ。

### SSL 実装

```java {line-numbers="true"}
SSLContext context = SSLContextBuilder.create().build();
SSLConnectionSocketFactory sslSocketFactory = new SSLConnectionSocketFactory(context);
CloseableHttpClient httpClient = HttpClients.custom().setSSLSocketFactory(sslSocketFactory).build();
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .httpClient(httpClient)
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```
