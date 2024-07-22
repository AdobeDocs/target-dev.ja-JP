---
title: カスタム HTTP クライアントの設定方法を学ぶ
description: ClientConfig.builder （）.httpClient （）を使用して TargetClient を設定する方法を説明します。
feature: APIs/SDKs
exl-id: 7615029c-b62d-4ed1-aadb-32e364c4c654
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '108'
ht-degree: 0%

---

# カスタム HTTP クライアント設定（Java）

SDK を実行するアプリケーションでカスタム HTTP クライアントが必要な場合、SSL の設定やリクエストへのデフォルトヘッダーの追加などの機能を有効にするには、`ClientConfig.builder().httpClient()` を使用して `TargetClient` を設定する必要があります。

## 基本的なカスタム HTTP クライアント設定

SDK は現在、`org.apache.http.client.HttpClient` インターフェイスを実装する HTTP クライアントをサポートしています。

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

`ClientConfig` に渡す `HttpClient` をカスタマイズして `TargetClient` で SSL を設定する方法の例を次に示します。 次のコードスニペットでは、SSL 設定用に `org.apache.http.conn.ssl` パッケージのクラスを使用しています。

### SSL の実装

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
