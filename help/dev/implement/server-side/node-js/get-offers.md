---
title: 用途 [!UICONTROL getOffers()] in [!DNL Adobe Target] Node.js SDK を使用する場合
description: 使用方法を学ぶ [!UICONTROL getOffers()] 決定を実行し、次からエクスペリエンスを取得する [!DNL Adobe Target].
feature: APIs/SDKs
exl-id: 3c4125ea-68d4-405e-9b9a-5fa832743153
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '336'
ht-degree: 22%

---

# [!UICONTROL オファーを取得] (Node.js)

## 説明

`[!UICONTROL getOffers()]` は、決定を実行し、次の場所からエクスペリエンスを取得するために使用されます。 [!DNL Adobe Target].


## メソッド

### getOffers

```js {line-numbers="true"}
TargetClient.getOffers(options: Object): Promise
```

## パラメーター

The `options` オブジェクトの構造は次のとおりです。

| 名前 | タイプ | 必須 | デフォルト | 説明 |
| --- |--- | --- | --- | --- |
| リクエスト | オブジェクト | ○ | None | に準拠 [[!DNL Target] 配信 API](/help/dev/implement/delivery-api/overview.md) リクエスト |
| visitorCookie | 文字列 | × | None | ECID (VisitorId)cookie |
| targetCookie | 文字列 | × | None | [!DNL Target] cookie |
| targetLocationHint | 文字列 | × | None | [!DNL Target] ロケーションヒント |
| consumerId | 文字列 | × | None | consumerIds （用） [!UICONTROL Analytics for Target] (A4T) ステッチ |
| CustomerIds | 配列 | × | None | VisitorId 互換形式の顧客 ID |
| sessionId | 文字列 | × | None | 複数の [!DNL Target] リクエスト |
| 訪問者 | オブジェクト | × | 新規訪問者 ID | 外部の VisitorId インスタンスを指定します |

## プロミス

`Promise` は次の構造を持って返します。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| request | オブジェクト | [[!UICONTROL Target 配信 API]](/help/dev/implement/delivery-api/overview.md) リクエスト |
| 応答 | オブジェクト | [[!UICONTROL Target 配信 API]](/help/dev/implement/delivery-api/overview.md) 応答 |
| visitorState | オブジェクト | 訪問者 API に渡す必要があるオブジェクト。 `getInstance()` |
| targetCookie | オブジェクト | [!DNL Target] cookie |
| targetLocationHintCookie | オブジェクト | [!DNL Target] ロケーションヒント Cookie |
| analyticsDetails | 配列 | Analytics ペイロード（クライアント側 Analytics を使用する場合） |
| responseTokens | 配列 | リスト [レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?). |
| trace | 配列 | すべてのリクエスト mbox/ビューの集計トレースデータ |
| status | オブジェクト | 応答のステータスを含むオブジェクト。 |
| decisioningMethod | 文字列 | 使用する判定方法を決定します ([オンデバイス](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md)、サーバー側、ハイブリッド ) |

`targetCookie` および `targetLocationHintCookie` データをブラウザーに渡すために使用されるオブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| name | 文字列 | cookie 名 |
| value | 任意 | Cookie の値の場合、は文字列に変換されます |
| maxAge | 数値 | The `maxAge` オプションは、現在の時刻（秒）を基準に有効期限が切れる便利な設定です。 |

The `status` ターゲット応答のステータスを示すために使用されるオブジェクトの構造は次のとおりです。

| 名前 | タイプ | 説明 |
| --- | --- | --- |
| status | 数値 | HTTP ステータスコード |
| message | 文字列 | 応答に関するメッセージ。 例えば、応答が決定されたかどうかを示す場合があります [オンデバイス](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/overview.md) またはサーバー側 |
| remoteMboxes | 配列 | 判定方法が `on-device`に値を指定する場合、完全に決定できなかった mbox 名の配列がデバイス上で提供されます。 つまり、 [[!UICONTROL Target 配信 API]](/help/dev/implement/delivery-api/overview.md) リクエストが必要です。 |

## 例

### Node.js

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const CONFIG = {
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg"
};

const targetClient = TargetClient.create(CONFIG);

const request = {
    context: {channel: "web"},
    execute: {
        mboxes: [{
            name: "a1-serverside-ab",
            index: 1
        }]
}};

const response = await targetClient.getOffers({ request });
```
