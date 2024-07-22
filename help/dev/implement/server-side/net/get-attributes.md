---
title: .NET SDK での getAttributes [!DNL Adobe Target]  使用
description: getAttributes （）を使用して、実験とパーソナライズされたエクスペリエンスをから取得し、属性値を抽出する方法  [!DNL Target]  説明します。
feature: APIs/SDKs
exl-id: 808da83d-3077-468b-a2ad-e35c25905f7d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 10%

---

# 属性を取得（.NET）

## 説明

`GetAttributes()` を使用して、[!DNL Target] から実験とパーソナライズされたエクスペリエンスを取得し、属性値を抽出します。

## メソッド

### getAttributes

```dotnet {line-numbers="true"}
TargetAttributes TargetClient.GetAttributes(TargetDeliveryRequest targetRequest, params string[] mboxes)
```

## パラメーター

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | × | null | [ オファーを取得&#x200B;](get-offers.md) に使用するのと同じ [!DNL Target] リクエスト |
| mboxNames | params 文字列 [] | × | null | mbox 名のパラメーター配列 |

## 結果

`TargetAttributes` オブジェクトが、次のプロパティとメソッドを持つ `TargetClient.GetAttributes()` から返されます。

| プロパティ/メソッド | 戻り値の型 | 説明 |
| --- | --- | --- |
| 応答 | TargetDeliveryResponse | [Get Offers](get-offers.md) によって通常返される応答オブジェクトを返します |
| ToDictionary | DirectOnlyDictionary | mbox 名でグループ化されたキーと値のペアを持つ辞書の辞書を返します |
| ToMboxDictionary （mboxName） | DirectOnlyDictionary | 指定された mbox のキーと値のペアを含む辞書を返します |
| GetBoolean （mboxName, key, defaultValue） | ブール | 指定された mbox 名と属性キーの値を返します |
| GetString （mboxName, key, defaultValue） | string | 指定された mbox 名と属性キーの値を返します |
| GetInteger （mboxName, key, defaultValue） | int | 指定された mbox 名と属性キーの値を返します |
| GetDouble （mboxName, key, defaultValue） | double | 指定された mbox 名と属性キーの値を返します |
| GetValue （mboxName, key, defaultValue） | T | 指定された mbox 名と属性キーの値を返します |

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
