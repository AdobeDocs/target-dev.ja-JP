---
title: プロファイルを取得
description: Adobe Target Profile API を使用して、で使用する訪問者データを取得する方法を説明します。 [!DNL Target].
contributors: https://github.com/icaraps
feature: APIs/SDKs
source-git-commit: 49acf92bbe06dbcee36fef2b7394acd7ce37baad
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 1%

---

# プロファイルを更新

A [!DNL Target] プロファイルは、次の 3 つの方法で取得できます。 `[!DNL Experience Cloud Visitor ID]` (`ECID`), `tntid` または `thirdPartyId`.

## の使用 [!DNL Experience Cloud Visitor ID] (ECID)

に基づいてプロファイルを取得できます `ECID`. HTTP メソッドはGETです。

URL は、次の例のようになります。

```
https://<clientCode>.tt.omtrdc.net/rest/v1/profiles/marketingCloudVisitorId/<ECID>?client=<clientCode>
```

置換 `<clientCode>` を [!DNL Target] [!UICONTROL クライアントコード] および `<ECID>` を [!DNL Experience Cloud Visitor ID] ([!DNL Marketing Cloud Visitor ID]) をクリックします。

## tntid の使用

[!DNL Target] を自動的に割り当てる `tntid` リクエストごとに

次の例は、 `tntid`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/your-tnt-id?client=<your-client-code>
```

置換 `<your-client-code>` および `your-tnt-id` およびは、GETリクエストを実行します。 次に、を使用したプロファイル取得呼び出しの例を示します。 `tntid`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/111492025094307-353046?client=<your-client-code>
```

## thirdPartyId の使用

[!DNL Adobe Target] プロファイルは、独自の識別子 ( 例： CRM ID、 `uuid`、メンバーシップ番号など )。

詳しくは、 [プロファイルを更新](/help/dev/administer/profile-api/profile-api-overview.md) をアタッチする方法を学ぶには `thirdPartyId` をプロファイルに追加します。

次の例は、 `thirdPartyId`:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/your-thirdpartyid?client=<your-client-code>
```

置換 `<your-client-code>` および `your-thirdpartyid` およびは、GETリクエストを実行します。 次に、を使用したプロファイル取得呼び出しの例を示します。 [!UICONTROL thirdpartyid]:

```
https://<your-client-code>.tt.omtrdc.net/rest/v1/profiles/thirdPartyId/a1-mbox3rdPartyId?client=<your-client-code>
```

この呼び出しがおこなわれると、 [!DNL Target] は、エッジリクエストで指定されたクラスター内の最初のプロファイルを探すか、プロファイルが配置されている場所にプロファイルを配置して、コンテンツを返します。 プロファイルコンテンツは JSON 形式で返されます。

## Authentication

The [!DNL Target Profile API] 認証をオンにすることで、セキュリティを確保できます。 [!DNL Target] ここで説明する UI。 認証がオンになったら、すべてのプロファイル API リクエストで、リクエストヘッダーにプロファイル認証トークンが設定されている必要があります。 トークン自体は、 [!DNL Target] UI または上記の手順 ( [プロファイル認証トークン](https://developers.adobetarget.com/api/#authentication-tokens){target=_blank} 」セクションに入力します。

## 計量

これらの呼び出しは、mbox 呼び出しにはカウントされません。

## エラー処理

への呼び出しの場合 `/thirdPartyId` 無効な、または期限切れの `thirdPartyId` 指定：

```
{"status" : 404, "message" : "No profile found for client <client_code> with third party id=<third_party_id>"}
```

プロファイルが見つからない場合、または期限切れの場合：

```
{"status" : 404, "message" : "No profile found for client <client_code> with mboxPC=<mbox_pc>"}
```
