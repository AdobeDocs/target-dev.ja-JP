---
title: で getAttributes を使用します。 [!DNL Adobe Target] .NET SDK を使用
description: getAttributes() を使用して、次の場所から実験とパーソナライズされたエクスペリエンスを取得する方法を説明します。 [!DNL Target] 属性値を抽出します。
feature: APIs/SDKs
exl-id: 808da83d-3077-468b-a2ad-e35c25905f7d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '211'
ht-degree: 10%

---

# 属性の取得 (.NET)

## 説明

`GetAttributes()` は、次の場所から実験とパーソナライズされたエクスペリエンスを取得するために使用されます。 [!DNL Target] 属性値を抽出します。

## メソッド

### getAttributes

```dotnet {line-numbers="true"}
TargetAttributes TargetClient.GetAttributes(TargetDeliveryRequest targetRequest, params string[] mboxes)
```

## パラメーター

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- | --- | --- | --- | --- |
| targetRequest | TargetDeliveryRequest | × | null | 同じ [!DNL Target] 次に対して使用されるリクエスト [オファーを取得&#x200B;する](get-offers.md) |
| mboxNames | params 文字列[] | × | null | mbox 名のパラメーター配列 |

## 結果

A `TargetAttributes` オブジェクトが `TargetClient.GetAttributes()` これは、次のプロパティとメソッドを持ちます。

| プロパティ/メソッド | 戻り値の型 | 説明 |
| --- | --- | --- |
| 応答 | TargetDeliveryResponse | が返す応答オブジェクトを返します。 [オファーを取得](get-offers.md) |
| ToDictionary | IReadOnlyDictionary | mbox 名でグループ化されたキーと値のペアを持つ辞書の辞書を返します |
| ToMboxDictionary(mboxName) | IReadOnlyDictionary | 指定された mbox のキーと値のペアを持つ辞書を返します |
| GetBoolean(mboxName, key, defaultValue) | bool | 指定された mbox 名と属性キーの値を返します |
| GetString(mboxName, key, defaultValue) | string | 指定された mbox 名と属性キーの値を返します |
| GetInteger(mboxName, key, defaultValue) | int | 指定された mbox 名と属性キーの値を返します |
| GetDouble(mboxName, key, defaultValue) | double | 指定された mbox 名と属性キーの値を返します |
| GetValue(mboxName, key, defaultValue) | T | 指定された mbox 名と属性キーの値を返します |

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
