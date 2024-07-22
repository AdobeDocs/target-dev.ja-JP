---
keywords: クラウドインスタンス、パブリックサフィックスリスト、パブリックサフィックス、cookie、ファーストパーティ cookie、ファーストパーティ cookie、azurewebsites.net、cloudapp.net、amazonaws.com、cloudfront.net、herokuapp.com、firebaseapp.com、targetGlobalSettings、cookieDomain、クラウドインスタンス 5、クラウドインスタンス 6、クラウドインスタンス 7、クラウドインスタンス 8、クラウドインスタンス 9、パブリックサフィックスリスト 0、パブリックサフィックスリスト 1、パブリックサフィックスリスト 2、パブリックサフィックスリスト 3、パブリックサフィックスリスト 4、パブリックサフィックスリスト 5
description: クラウドベースのインスタンスを使用してテストや概念実証を行う際に発生する問題（ソリューショ  [!DNL Adobe Target]  に関する問題）について説明します。
title: クラウドベ  [!DNL Target]  スのインスタンスでを使用できますか？
feature: at.js
exl-id: 4b24fdc0-6c74-4b29-bbf9-7a761d4564a2
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 45%

---

# [!DNL Target] でのクラウドベースのインスタンスの使用

クラウドベースのインスタンスを使用して [!DNL Adobe Target] をテストする際の問題に関する情報をまとめています。

[!DNL Target] のお客様は、[!DNL Target] でクラウドベースのインスタンスを使用してテストをおこなったり、簡単な概念実証に利用したりする場合があります。このインスタンスに含まれるドメインの例を次に示します。

`azurewebsites.net`、`cloudapp.net`、`amazonaws.com`、`cloudfront.net`、`herokuapp.com`、または `firebaseapp.com`

これらのドメインは、他の多くのドメインと同様に[パブリックサフィックスリスト](https://publicsuffix.org/list/public_suffix_list.dat)に含まれています。

**問題：**&#x200B;最新型のブラウザーでは、これらのドメインを使用していると Cookie が保存されません。

at.js JavaScript ライブラリでは、[!DNL [!DNL Target]] で常に一貫性のあるエクスペリエンスを提供できるように、cookie を使用してユーザーをトラッキングします。 [!DNL Target] JavaScript ライブラリで Cookie を保存できない場合、Target リクエストは無効になります。

**解決策：**&#x200B;パブリックサフィックスリストに含まれているドメインを使用してクラウドベースのインスタンスを利用する必要がある場合は、`cookieDomain` 設定をカスタマイズすることをお勧めします。詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。
