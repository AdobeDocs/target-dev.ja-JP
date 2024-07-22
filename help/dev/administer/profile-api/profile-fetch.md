---
title: プロファイルの取得
description: Adobe Target Profile API を使用して、 [!DNL Target] で使用する訪問者データを取得する方法を説明します。
contributors: https://github.com/icaraps
feature: APIs/SDKs
exl-id: b422ae68-49b3-4d60-9ea4-0fa67b6934b0
source-git-commit: b8ccfdcaff6aa17a325727df0a9ffd716e44519b
workflow-type: tm+mt
source-wordcount: '290'
ht-degree: 0%

---

# プロファイルの取得

[!DNL Target] プロファイルは、`[!DNL Experience Cloud Visitor ID]` （`ECID`）、`tntid`、`thirdPartyId` の 3 つの方法で取得できます。

## [!DNL Experience Cloud Visitor ID] （ECID）の使用

`ECID` に基づいてプロファイルを取得できます。 HTTP メソッドはGETである必要があります。

URL は次の例のようになります。

```
https://<clientCode>.tt.omtrdc.net/rest/v1/profiles/marketingCloudVisitorId/<ECID>?client=<clientCode>
```

`<clientCode>` を [!DNL Target] [!UICONTROL Client Code] に、`<ECID>` を [!DNL Experience Cloud Visitor ID] （[!DNL Marketing Cloud Visitor ID]）に置き換えます。

## tntid の使用

[!DNL Target] は、リクエストごとに自動的に `tntid` を割り当てます。

`tntid` を使用してプロファイルを取得するリクエストの形式の例を次に示します。

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/your-tnt-id?client=<your-client-code>
```

`<your-client-code>` と `your-tnt-id` を置き換えて、GETリクエストを実行します。 `tntid` を使用したプロファイル取得呼び出しの例を次に示します。

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/111492025094307-353046?client=<your-client-code>
```

## サードパーティ ID の使用

[!DNL Adobe Target] プロファイルは、独自の識別子（CRM ID、`uuid`、メンバーシップ番号など）で拡張できます。

プロファイルに `thirdPartyId` を添付する方法については、[ プロファイルの更新 ](/help/dev/administer/profile-api/profile-api-overview.md) を参照してください。

`thirdPartyId` を使用してプロファイルを取得するリクエストの形式の例を次に示します。

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/your-thirdpartyid?client=<your-client-code>
```

`<your-client-code>` と `your-thirdpartyid` を置き換えて、GETリクエストを実行します。 [!UICONTROL thirdpartyid] を使用したプロファイル取得呼び出しの例を次に示します。

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/a1-mbox3rdPartyId?client=<your-client-code>
```

この呼び出しが行われると、[!DNL Target] は、エッジリクエストで示されたクラスター内、またはプロファイルが存在する場所で最初にプロファイルを見つけて、コンテンツを返そうとします。 プロファイルコンテンツは JSON 形式で返されます。

## Authentication

[!DNL Target Profile API] は、ここで説明するように、[!DNL Target] UI から認証をオンにすることで保護できます。 認証をオンにすると、すべてのプロファイル API リクエストのリクエストヘッダーにプロファイル認証トークンが設定される必要があります。 トークン自体は、[!DNL Target] UI を使用するか、前述の [ プロファイル認証トークン ](https://developers.adobetarget.com/api/#authentication-tokens){target=_blank} の節で説明している手順を使用して生成できます。

## 計測

これらの呼び出しは、mbox 呼び出しにはカウントされません。

## エラー処理

`/thirdPartyId` への呼び出しで、無効な呼び出しまたは期限切れの `thirdPartyId` が指定された場合：

```
{"status" : 404, "message" : "No profile found for client <client_code> with third party id=<third_party_id>"}
```

プロファイルが見つからないか、有効期限が切れている場合：

```
{"status" : 404, "message" : "No profile found for client <client_code> with mboxPC=<mbox_pc>"}
```
