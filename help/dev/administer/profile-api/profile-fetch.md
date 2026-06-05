---
title: プロファイルを取得
description: Adobe Target Profile APIを使用して、 [!DNL Target]で使用する訪問者データを取得する方法を説明します。
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: b422ae68-49b3-4d60-9ea4-0fa67b6934b0
TQID: https://experienceleague.adobe.com/sCVfAY8W0oYu2ak-W4MYvcWSoUiAuaU3762JEhocZSE
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 297
ht-degree: 0%

---

# プロファイルを取得

[!DNL Target] プロファイルは、次の3つの方法で取得できます。`[!DNL Experience Cloud Visitor ID]` （`ECID`）、`tntid`または`thirdPartyId`を使用します。

## [!DNL Experience Cloud Visitor ID] （ECID）の使用

`ECID`に基づいてプロファイルを取得できます。 HTTP メソッドはGETである必要があります。

URLは次の例のようになります。

```
https://<clientCode>.tt.omtrdc.net/rest/v1/profiles/marketingCloudVisitorId/<ECID>?client=<clientCode>
```

`<clientCode>`を[!DNL Target] [!UICONTROL  クライアントコード ]に、`<ECID>`を[!DNL Experience Cloud Visitor ID] （[!DNL Marketing Cloud Visitor ID]）に置き換えます。

## tntidの使用

[!DNL Target]は、リクエストごとに`tntid`を自動的に割り当てます。

次の例は、`tntid`を使用してプロファイルを取得するリクエスト形式を示しています。

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/your-tnt-id?client=<your-client-code>
```

`<your-client-code>`と`your-tnt-id`を置き換えて、GET リクエストを実行します。 次に、`tntid`を使用したプロファイルフェッチ呼び出しの例を示します。

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/111492025094307-353046?client=<your-client-code>
```

## thirdPartyIdの使用

[!DNL Adobe Target] プロファイルは、独自の識別子（CRM ID、`uuid`、メンバーシップ番号など）で強化できます。

プロファイルに`thirdPartyId`を添付する方法については、[ プロファイルの更新](/help/dev/administer/profile-api/profile-api-overview.md)を参照してください。

次の例は、`thirdPartyId`を使用してプロファイルを取得するリクエスト形式を示しています。

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/your-thirdpartyid?client=<your-client-code>
```

`<your-client-code>`と`your-thirdpartyid`を置き換えて、GET リクエストを実行します。 [!UICONTROL thirdpartyid]を使用したプロファイル取得呼び出しの例を次に示します。

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/a1-mbox3rdPartyId?client=<your-client-code>
```

この呼び出しが行われると、[!DNL Target]は、edge リクエストに記載されているクラスター内で最初にプロファイルを見つけるか、プロファイルが配置されている場所でプロファイルを見つけてコンテンツを返そうとします。 プロファイルの内容はJSON形式で返されます。

## Authentication

[!DNL Target Profile API]は、[!DNL Target] UIから認証をオンにすることで保護できます（こちらを参照）。 認証がオンになると、すべてのプロファイル API リクエストにリクエストヘッダーでプロファイル認証トークンを設定する必要があります。 トークン自体は、[!DNL Target] UIを使用するか、[ プロファイル認証トークン ](https://developers.adobetarget.com/api/#authentication-tokens){target=_blank} セクションで前述した手順を使用して生成できます。

## 計量

これらの呼び出しは、mbox呼び出しにカウントされません。

## エラー処理

無効または期限切れの`thirdPartyId`が指定された`/thirdPartyId`への呼び出しの場合：

```
{"status" : 404, "message" : "No profile found for client <client_code> with third party id=<third_party_id>"}
```

プロファイルが見つからないか、有効期限が切れている場合：

```
{"status" : 404, "message" : "No profile found for client <client_code> with mboxPC=<mbox_pc>"}
```
