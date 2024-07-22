---
title: Java SDK での getAttributes [!DNL Adobe Target]  使用
description: getAttributes （）を使用して、実験とパーソナライズされたエクスペリエンスをから取得し、属性値を抽出する方法  [!DNL Target]  説明します。
feature: APIs/SDKs
exl-id: e493e1b9-7180-4a7c-b98d-be84cc3a57c3
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '169'
ht-degree: 13%

---

# 属性を取得（Java）

## 説明

`getAttributes()` を使用して、[!DNL Target] から実験とパーソナライズされたエクスペリエンスを取得し、属性値を抽出します。

## メソッド

### getAttributes

```javascript {line-numbers="true"}
Attributes TargetClient.getAttributes(TargetDeliveryRequest targetRequest, String ...mboxes)
```

## パラメーター

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | ○ | None | [ オファーを取得&#x200B;](get-offers.md) に使用するのと同じターゲットリクエスト |
| mboxNames | var-args 配列 | × | None | mbox 名の var-args 配列 |


## 結果

`TargetClient.getAttributes()` から `Attributes` オブジェクトが返されます。このオブジェクトには次のメソッドがあります。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| getBoolean （mboxName, key） | ブール値 | 指定された mbox 名と属性キーの値を返します |
| getString （mboxName, key） | 文字列 | 指定された mbox 名と属性キーの値を返します |
| getInteger （mboxName, key） | 整数 | 指定された mbox 名と属性キーの値を返します |
| getDouble （mboxName, key） | ダブル | 指定された mbox 名と属性キーの値を返します |
| toMboxMap （mboxName） | マップ | キーと値のペアを含むシンプルなマップを返します |
| getResponse （） | TargetDeliveryResponse | getOffers が通常返す応答オブジェクトを返します |

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
