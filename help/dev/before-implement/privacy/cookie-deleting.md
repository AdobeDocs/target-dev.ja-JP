---
keywords: cookie、cookie、cookie の削除、cookie の削除  [!DNL Target] cookie、google chrome、chrome、mozilla firefox、firefox、microsoft edge、safari、cookie1
description: エクスペリエンスを検証できるように  [!DNL Target]  ブラウザーの Cookie を削除する方法について説明します。
title: ' [!DNL Target] Cookie を削除するにはどうすればよいですか？'
feature: Privacy & Security
exl-id: c975c47f-8d81-4abe-aa89-f65275a73002
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '355'
ht-degree: 3%

---

# [!DNL Target] の Cookie の削除

[!DNL Adobe Target] ブラウザーの Cookie （mbox）を削除すると、テスト中にすべてのエクスペリエンスを検証できます。

[!DNL Target] Cookie （mbox）がない場合は、新しい訪問者と見なされ、新しいエクスペリエンスが表示されます。 ブラウザーの Cookie をすべて削除せずに mbox を削除する方法はいくつかあります。

>[!NOTE]
>
>以下の手順は、リストされているブラウザーとバージョンで正しく使用されています。 特定のブラウザーまたはバージョンの手順については、インターネットを検索してください。

## Google Chromeからの [!DNL Target] Cookie の削除

バージョン 84.0.4147.105

1. **[!UICONTROL Chrome]** メニュー/**[!UICONTROL Preferences]** をクリックします。
1. 「**[!UICONTROL Privacy and Security]**」タブをクリックします。
1. **[!UICONTROL Cookies and other site data]** をクリックします。
1. **[!UICONTROL See all cookies and site data]** をクリックします。
1. 「`adobe.com`」セクションを展開し、「**mbox** cookie を選択して、削除アイコン（X）をクリックします。

## Mozilla Firefox からの [!DNL Target] Cookie の削除

バージョン 79.0

### `adobe.com` に関連付けられているすべての Cookie を削除する

1. **[!UICONTROL Firefox]** メニュー/**[!UICONTROL Preferences]** をクリックします。
1. 「**[!UICONTROL Privacy and Security]**」タブをクリックします。
1. [**Cookies and Site Data*] の下の [**[!UICONTROL Manage Data]**] をクリックします。
1. `adobe.com` サイトを選択し、「**[!UICONTROL Remove Selected]**」をクリックします。

>[!WARNING]
>
>これにより、`adobe.com` サイトに関連付けられているすべての Cookie が削除されます。 サイトの個々の Cookie を削除する場合は、次の手順に従います。

### 個々の Cookie の削除（mbox）

1. Firefo で、**[!UICONTROL Tools]**/**[!UICONTROL Web Developer]**/**[!UICONTROL Storage Inspector]** をクリックします。
1. 「**[!UICONTROL Advanced]**」タブをクリックします。
1. 削除する Cookie が含まれている Web ページに移動します。
1. **[!UICONTROL Cookies]** セクションを展開し、「`https://experience.adobe.com`」をクリックします。
1. **[!UICONTROL mbox]** cookie を右クリックし、「**[!UICONTROL Delete]**」をクリックします。

## Microsoft Edgeからの [!DNL Target] Cookie の削除

バージョン 84.0.522.52

1. **[!UICONTROL Microsoft Edge]** メニュー/**[!UICONTROL Preferences]** をクリックします。
1. 「**[!UICONTROL Site Permissions]**」タブをクリックします。
1. **[!UICONTROL Cookies and site data]** をクリックします。
1. **[!UICONTROL See all cookies and site data]** をクリックします。
1. 「`adobe.com`」セクションを展開し、「**mbox** cookie を選択して、削除アイコン（X）をクリックします。

## Apple Safari からの [!DNL Target] Cookie の削除

バージョン 13.1.2

### `adobe.com` に関連付けられているすべての Cookie を削除する

1. **[!UICONTROL Safari]** メニュー/**[!UICONTROL Preferences]** をクリックします。
1. 「**[!UICONTROL Privacy]**」タブをクリックします。
1. **[!UICONTROL Manage Website Data]** をクリックします。
1. 削除する Cookie のサイトを選択し、「**[!UICONTROL Remove]**」をクリックします。

>[!WARNING]
>
>これにより、`adobe.com` サイトに関連付けられているすべての Cookie が削除されます。 サイトの個々の Cookie を削除する場合は、次の手順に従います。

### 個々の Cookie の削除（mbox）

1. **[!UICONTROL Safari]** メニュー/**[!UICONTROL Preferences]** をクリックします。
1. 「**[!UICONTROL Advanced]**」タブをクリックします。
1. **[!UICONTROL Show Develop menu in menu bar]** オプションを選択します。
1. 削除する Cookie が含まれている Web ページに移動します。
1. **[!UICONTROL Develop]** メニュー/**[!UICONTROL Show Web Inspector]** をクリックします。
1. 「**[!UICONTROL Storage]**」タブをクリックします。
1. **[!UICONTROL Cookies]** セクションを展開し、「`www.adobe.com`」をクリックします。
1. **mbox** cookie を右クリックし、「**[!UICONTROL Delete]**」をクリックします。
