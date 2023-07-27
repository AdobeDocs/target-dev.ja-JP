---
keywords: クラウドインスタンス，パブリックサフィックスリスト，パブリックサフィックス， cookie，ファーストパーティ cookie, azurewebsites.net, cloudapp.net, amazonaws.com, firebaseapp.com, targetGlobalSettings, cloudfront.net, herokuapp.com, cloudinstances5, cloud instances7, cloud instances8, cloud instances9，パブリックサフィックスリスト 0，パブリックサフィックスリスト 1，パブリックサフィックスリスト 3，パブリックサフィックスリスト 4，パブリックリスト 5
description: クラウドベースのインスタンスを使用してをテストする際に（ソリューションで）お客様が直面する問題を調査する [!DNL Adobe Target] または概念実証用にも取り込まれます。
title: 使用可能な機能 [!DNL Target] クラウドベースのインスタンスを使用する場合
feature: at.js
exl-id: 4b24fdc0-6c74-4b29-bbf9-7a761d4564a2
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '195'
ht-degree: 49%

---

#  でのクラウドベースのインスタンスの使用[!DNL Target]

クラウドベースのインスタンスを使用して [!DNL Adobe Target] をテストする際の問題に関する情報をまとめています。

[!DNL Target] のお客様は、[!DNL Target] でクラウドベースのインスタンスを使用してテストをおこなったり、簡単な概念実証に利用したりする場合があります。このインスタンスに含まれるドメインの例を次に示します。

`azurewebsites.net`、`cloudapp.net`、`amazonaws.com`、`cloudfront.net`、`herokuapp.com`、または `firebaseapp.com`

これらのドメインは、他の多くのドメインと同様に[パブリックサフィックスリスト](https://publicsuffix.org/list/public_suffix_list.dat)に含まれています。

**問題：**&#x200B;最新型のブラウザーでは、これらのドメインを使用していると Cookie が保存されません。

at.js JavaScript ライブラリでは、Cookie を使用してユーザーを追跡し、[!DNL [!DNL Target]] は常に一貫したエクスペリエンスを提供します。 次の場合、 [!DNL Target] JavaScript ライブラリは Cookie を保存できないので、Target リクエストが無効です。

**解決策：**&#x200B;パブリックサフィックスリストに含まれているドメインを使用してクラウドベースのインスタンスを利用する必要がある場合は、`cookieDomain` 設定をカスタマイズすることをお勧めします。詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。
