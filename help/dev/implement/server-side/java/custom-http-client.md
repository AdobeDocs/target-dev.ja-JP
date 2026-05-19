---
title: カスタム HTTP クライアントの設定方法を説明します
description: ClientConfig.builder （）.httpClient （）を使用してTargetClientを設定する方法を説明します。
feature: APIs/SDKs
exl-id: 7615029c-b62d-4ed1-aadb-32e364c4c654
TQID: https://experienceleague.adobe.com/SwijRIrhqSG4Mlij4sBH9Kx8tRB-6Bo7eyMoUZREOW8
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 108
ht-degree: 0%

---

# カスタム HTTP クライアント設定（Java）

SDKを実行しているアプリケーションでカスタム HTTP クライアントが必要な場合、SSLの設定やリクエストへのデフォルトヘッダーの追加などの機能を有効にするには、`TargetClient`を`ClientConfig.builder().httpClient()`を使用して設定する必要があります。

## 基本的なカスタム HTTP クライアント設定

SDKは現在、`org.apache.http.client.HttpClient` インターフェイスを実装するHTTP クライアントをサポートしています。

### 基本実装

```java {line-numbers="true"}
CloseableHttpClient httpClient = HttpClients.custom().build();
ClientConfig clientConfig = ClientConfig.builder()
    .client("acmeclient")
    .organizationId("1234567890@AdobeOrg")
    .httpClient(httpClient)
    .build();
TargetClient targetClient = TargetClient.create(clientConfig);
```

## SSL設定を使用したカスタム HTTP クライアント設定

`ClientConfig`に渡された`HttpClient`をカスタマイズして、`TargetClient`でSSLを設定する方法の例を次に示します。 次のコードスニペットでは、SSL設定に`org.apache.http.conn.ssl` パッケージのクラスを使用しています。

### SSL実装

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
