---
keywords: cookie、cookie、cookieの削除、cookieの削除 [!DNL Target] cookie、google chrome、chrome、mozilla firefox、firefox、microsoft edge、safari、cookie1
description: エクスペリエンスを検証できるように、 [!DNL Target]  ブラウザーCookieを削除する方法について説明します。
title: ' [!DNL Target] Cookieを削除するにはどうすればよいですか？'
feature: Privacy & Security
exl-id: c975c47f-8d81-4abe-aa89-f65275a73002
TQID: https://experienceleague.adobe.com/t4ieDzmphu8NHTM9eGnaZMoeXk-Y1G05E4K6spdSs6Y
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: d095671a-1355-40aa-8b5f-06c33c68080bid: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 352
ht-degree: 3%

---

# [!DNL Target] Cookieを削除

テスト中にすべてのエクスペリエンスを検証できるように、[!DNL Adobe Target] ブラウザーのCookie （mbox）を削除できます。

[!DNL Target] Cookie （mbox）がない場合、新しい訪問者とみなされ、新しいエクスペリエンスが表示されます。 すべてのブラウザーCookieを削除せずにmboxを削除する方法はいくつかあります。

>[!NOTE]
>
>以下の手順は、リストされているブラウザーとバージョンに対して正しいです。 インターネットで特定のブラウザーまたはバージョンの手順を検索します。

## Google Chromeから[!DNL Target] Cookieを削除します

バージョン 84.0.4147.105

1. **[!UICONTROL Chrome]** メニュー> **[!UICONTROL Preferences]**&#x200B;をクリックします。
1. 「**[!UICONTROL Privacy and Security]**」タブをクリックします。
1. **[!UICONTROL Cookies and other site data]** をクリックします。
1. **[!UICONTROL See all cookies and site data]** をクリックします。
1. 「`adobe.com`」セクションを展開し、**mbox** Cookieを選択してから、削除アイコン （X）をクリックします。

## Mozilla Firefoxから[!DNL Target] Cookieを削除します

バージョン 79.0

### `adobe.com`に関連付けられたすべてのCookieを削除

1. **[!UICONTROL Firefox]** メニュー> **[!UICONTROL Preferences]**&#x200B;をクリックします。
1. 「**[!UICONTROL Privacy and Security]**」タブをクリックします。
1. 「** Cookieとサイトデータ *」で、**[!UICONTROL Manage Data]**&#x200B;をクリックします。
1. `adobe.com` サイトを選択し、**[!UICONTROL Remove Selected]**&#x200B;をクリックします。

>[!WARNING]
>
>これにより、`adobe.com` サイトに関連付けられたすべてのCookieが削除されます。 サイトの個別のCookieを削除する場合は、次の手順に従います。

### 個別のCookieの削除（mbox）

1. Firefoで、**[!UICONTROL Tools]** > **[!UICONTROL Web Developer]** > **[!UICONTROL Storage Inspector]**&#x200B;をクリックします。
1. 「**[!UICONTROL Advanced]**」タブをクリックします。
1. 削除するCookieを含むweb ページに移動します。
1. **[!UICONTROL Cookies]** セクションを展開し、`https://experience.adobe.com`をクリックします。
1. **[!UICONTROL mbox]** Cookieを右クリックし、**[!UICONTROL Delete]**&#x200B;をクリックします。

## Microsoft Edgeから[!DNL Target] Cookieを削除します

バージョン 84.0.522.52

1. **[!UICONTROL Microsoft Edge]** メニュー> **[!UICONTROL Preferences]**&#x200B;をクリックします。
1. 「**[!UICONTROL Site Permissions]**」タブをクリックします。
1. **[!UICONTROL Cookies and site data]** をクリックします。
1. **[!UICONTROL See all cookies and site data]** をクリックします。
1. 「`adobe.com`」セクションを展開し、**mbox** Cookieを選択してから、削除アイコン （X）をクリックします。

## Apple Safariから[!DNL Target] Cookieを削除します

バージョン 13.1.2

### `adobe.com`に関連付けられたすべてのCookieを削除

1. **[!UICONTROL Safari]** メニュー> **[!UICONTROL Preferences]**&#x200B;をクリックします。
1. 「**[!UICONTROL Privacy]**」タブをクリックします。
1. **[!UICONTROL Manage Website Data]** をクリックします。
1. 削除するCookieのサイトを選択し、**[!UICONTROL Remove]**&#x200B;をクリックします。

>[!WARNING]
>
>これにより、`adobe.com` サイトに関連付けられたすべてのCookieが削除されます。 サイトの個別のCookieを削除する場合は、次の手順に従います。

### 個別のCookieの削除（mbox）

1. **[!UICONTROL Safari]** メニュー> **[!UICONTROL Preferences]**&#x200B;をクリックします。
1. 「**[!UICONTROL Advanced]**」タブをクリックします。
1. 「**[!UICONTROL Show Develop menu in menu bar]**」オプションを選択します。
1. 削除するCookieを含むweb ページに移動します。
1. **[!UICONTROL Develop]** メニュー> **[!UICONTROL Show Web Inspector]**&#x200B;をクリックします。
1. 「**[!UICONTROL Storage]**」タブをクリックします。
1. **[!UICONTROL Cookies]** セクションを展開し、`www.adobe.com`をクリックします。
1. **mbox** Cookieを右クリックし、**[!UICONTROL Delete]**&#x200B;をクリックします。
