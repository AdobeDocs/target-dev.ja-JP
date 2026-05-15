---
title: .NET SDKで [!DNL Adobe Target] のgetAttributesを使用する
description: getAttributes （）を使用して、 [!DNL Target] から実験とパーソナライズされたエクスペリエンスを取得し、属性値を抽出する方法を説明します。
feature: APIs/SDKs
exl-id: 808da83d-3077-468b-a2ad-e35c25905f7d
TQID: https://experienceleague.adobe.com/aflHPozCwJ-6fB7X-2jLaBvs42Ohz6OzwZ7AvkahCE8
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: bcc5edb5-84c3-4940-9f84-ed88b6c16274
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 209
ht-degree: 10%

---

# 属性（.NET）を取得

## 説明

`GetAttributes()`は、[!DNL Target]から実験とパーソナライズされたエクスペリエンスを取得し、属性値を抽出するために使用されます。

## メソッド

### getAttributes

```dotnet {line-numbers="true"}
TargetAttributes TargetClient.GetAttributes(TargetDeliveryRequest targetRequest, params string[] mboxes)
```

## パラメーター

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | × | null | [ オファーの取得](get-offers.md)に使用したものと同じ&#x200B;[!DNL Target] リクエスト |
| mboxNames | パラメーター文字列[] | × | null | mbox名のパラメーター配列 |

## 結果

次のプロパティとメソッドを持つ`TargetAttributes` オブジェクトが`TargetClient.GetAttributes()`から返されます。

| プロパティ/メソッド | 戻り値の型 | 説明 |
| --- | --- | --- |
| 応答 | TargetDeliveryResponse | 通常、[Get Offers](get-offers.md)によって返される応答オブジェクトを返します |
| ToDictionary | IReadOnlyDictionary | mbox名でグループ化されたキー値のペアを持つ辞書を返します |
| ToMboxDictionary （mboxName） | IReadOnlyDictionary | 指定されたmboxのキー値のペアを持つ辞書を返します |
| GetBoolean （mboxName, key, defaultValue） | ブール | 指定したmbox名と属性キーの値を返します |
| GetString （mboxName, key, defaultValue） | string | 指定したmbox名と属性キーの値を返します |
| GetInteger （mboxName, key, defaultValue） | int | 指定したmbox名と属性キーの値を返します |
| GetDouble （mboxName, key, defaultValue） | double | 指定したmbox名と属性キーの値を返します |
| GetValue （mboxName, key, defaultValue） | T | 指定したmbox名と属性キーの値を返します |

## 例

### \.NET

```dotnet {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeClient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

var targetClient = TargetClient.Create(targetClientConfig);

var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .Build();

var offerAttributes = targetClient.GetAttributes(targetDeliveryRequest, "demo-engineering-flags");

//returns just the value of searchProviderId from the mbox offer
var searchProviderId = offerAttributes.GetString("demo-engineering-flags", "searchProviderId");

//returns a simple Dictionary representing the mbox offer
var engineeringFlags = offerAttributes.ToMboxDictionary("demo-engineering-flags");

//  the value of engineeringFlags looks like this
//  {
//      "cdnHostname": "cdn.cloud.corp.net",
//      "searchProviderId": 143,
//      "hasLegacyAccess": false
//  }

var assetUrl = $"http://{engineeringFlags["cdnHostname"]}/path/to/asset";
```
