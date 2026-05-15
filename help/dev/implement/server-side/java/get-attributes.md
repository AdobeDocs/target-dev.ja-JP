---
title: Java SDKで [!DNL Adobe Target] のgetAttributesを使用する
description: getAttributes （）を使用して、 [!DNL Target] から実験とパーソナライズされたエクスペリエンスを取得し、属性値を抽出する方法を説明します。
feature: APIs/SDKs
exl-id: e493e1b9-7180-4a7c-b98d-be84cc3a57c3
TQID: https://experienceleague.adobe.com/ZZy9nUXiyR-qwBmOgv-TPS6ZuilvAuW850gH1Doqquo
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: bcc5edb5-84c3-4940-9f84-ed88b6c16274
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 169
ht-degree: 13%

---

# 属性の取得（Java）

## 説明

`getAttributes()`は、[!DNL Target]から実験とパーソナライズされたエクスペリエンスを取得し、属性値を抽出するために使用されます。

## メソッド

### getAttributes

```javascript {line-numbers="true"}
Attributes TargetClient.getAttributes(TargetDeliveryRequest targetRequest, String ...mboxes)
```

## パラメーター

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | ○ | None | [Get Offers&#x200B;](get-offers.md)に使用されているものと同じターゲットリクエスト |
| mboxNames | var-args配列 | × | None | mbox名のvar-args配列 |


## 結果

次のメソッドを持つ`TargetClient.getAttributes()`から`Attributes` オブジェクトが返されます：

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| getBoolean （mboxName, key） | ブール値 | 指定したmbox名と属性キーの値を返します |
| getString （mboxName, key） | 文字列 | 指定したmbox名と属性キーの値を返します |
| getInteger （mboxName, key） | 整数 | 指定したmbox名と属性キーの値を返します |
| getDouble （mboxName, key） | ダブル | 指定したmbox名と属性キーの値を返します |
| toMboxMap （mboxName） | マップ | キーと値のペアを持つ単純なMapを返します |
| getResponse （） | TargetDeliveryResponse | 通常getOffersによって返される応答オブジェクトを返します |

## 例

### Java

```javascript {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
        .client("acmeclient")
        .organizationId("1234567890@AdobeOrg")
        .build();

TargetClient targetJavaClient = TargetClient.create(clientConfig);

TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
        .context(new Context().channel(ChannelType.WEB))
        .build();

Attributes offerAttributes = targetJavaClient.getAttributes(targetDeliveryRequest, "demo-engineering-flags");

//returns just the value of searchProviderId from the mbox offer
String searchProviderId = offerAttributes.getString("demo-engineering-flags", "searchProviderId");

//returns a simple Map representing the mbox offer
Map<String, Object> engineeringFlags = offerAttributes.toMboxMap("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

String assetUrl = "http://" + engineeringFlags.cdnHostname + "/path/to/asset";
```
