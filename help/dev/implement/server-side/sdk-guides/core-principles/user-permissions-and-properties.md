---
title: ユーザーの権限とプロパティ
description: The [!DNL Target] SDK には、ユーザー権限とプロパティがサポートされています。
exl-id: 612faf1a-e8f9-4321-b831-90fba69ead3a
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '122'
ht-degree: 9%

---

# ユーザーの権限とプロパティ

The [!DNL Target] SDK には、ユーザー権限とプロパティがサポートされています。 もしあなたがどのように [!DNL Adobe Target] は、ワークスペースとプロパティを介してエンタープライズ権限を処理します。詳しくは、 [Enterprise ユーザーの権限](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=ja).

クライアントは、2 つの方法のいずれかでプロパティトークンを使用できます。

## グローバルプロパティトークン

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
    propertyToken: "8c4630b1-16db-e2fc-3391-8b3d81436cfb"
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({...})
```

>[!TAB Java]

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
    .client("emeaprod4")
    .organizationId("0DD934B85278256B0A490D44@AdobeOrg")
    .defaultPropertyToken("8c4630b1-16db-e2fc-3391-8b3d81436cfb")
    .build();

TargetClient targetClient = TargetClient.create(clientConfig);
```

>[!ENDTABS]

## getOffers 呼び出しの付随的なプロパティトークン

プロパティトークンは個々の `getOffers` を呼び出します。 これは、プロパティオブジェクトをリクエストに追加することでおこなわれます。 この方法で指定されたプロパティトークンは、設定のセットよりも優先されます。

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const CONFIG = {
    client: "acmeclient",
    organizationId: "1234567890@AdobeOrg",
};

const targetClient = TargetClient.create(CONFIG);

targetClient.getOffers({
    request: {
        execute: {
            pageLoad: {}
        },
        property: {
            token: "8c4630b1-16db-e2fc-3391-8b3d81436cfb"
        }           
    }
})
```

>[!TAB Java]

```java {line-numbers="true"}
ExecuteRequest executeRequest = new ExecuteRequest()
    .mboxes(getMboxRequests(mbox));

TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
    .context(getContext(request))
    .execute(executeRequest)
    .cookies(getTargetCookies(request.getCookies()))
    .property(new Property().token("8c4630b1-16db-e2fc-3391-8b3d81436cfb"))
    .build();

TargetDeliveryResponse targetResponse = targetClient.getOffers(targetDeliveryRequest);
```

>[!ENDTABS]
