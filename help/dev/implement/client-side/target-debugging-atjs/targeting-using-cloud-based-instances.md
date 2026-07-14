---
keywords: cloud instances, public suffix list, public suffix list, cookie, first-party cookie, 1st-party cookie, azurewebsites.net, cloudapp.net, amazonaws.com, cloudfront.net, herokuapp.com, firebaseapp.com, targetGlobalSettings, cookieDomain, cloud instances5, cloud instances6, cloud instances7, cloud instances8, cloud instances9, public suffix list0, public suffix list1, public suffix list2, public suffix list3, public suffix list4, public suffix list5
description: クラウドベースのインスタンスを使用してテストを行う際に顧客が直面する問題（解決策）や、概念実証の目的で発生する問題を調査します。 [!DNL Adobe Target] または概念実証の目的で使用します。
title: クラウドベースのインスタンスで [!DNL Target] を使用できますか？
feature: at.js
exl-id: 4b24fdc0-6c74-4b29-bbf9-7a761d4564a2
TQID: https://experienceleague.adobe.com/df63sTQxukCfa4pYc1X6FRvxV3cY2UG-ixk5v9Fqh-c
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 235baadf4059d2c363368408012630d6619aef99
workflow-type: tm+mt
source-wordcount: 203
ht-degree: 46%

---

# [!DNL Target]でクラウドベースのインスタンスを使用

クラウドベースのインスタンスを使用して [!DNL Adobe Target] をテストする際の問題に関する情報をまとめています。

[!DNL Target] のお客様は、[!DNL Target] でクラウドベースのインスタンスを使用してテストをおこなったり、簡単な概念実証に利用したりする場合があります。 このインスタンスに含まれるドメインの例を次に示します。

`azurewebsites.net`、`cloudapp.net`、`amazonaws.com`、`cloudfront.net`、`herokuapp.com`、または `firebaseapp.com`

これらのドメインは、他の多くのドメインと同様に[パブリックサフィックスリスト](https://publicsuffix.org/list/public_suffix_list.dat)に含まれています。

**問題：**&#x200B;最新型のブラウザーでは、これらのドメインを使用していると Cookie が保存されません。

at.js JavaScript ライブラリでは、Cookieを使用してユーザーを追跡し、[!DNL [!DNL Target]]が常に一貫したエクスペリエンスを提供できるようにします。 [!DNL Target] JavaScript ライブラリでCookieを保存できない場合、Target リクエストは無効になります。

**解決策：**&#x200B;パブリックサフィックスリストに含まれているドメインを使用してクラウドベースのインスタンスを利用する必要がある場合は、`cookieDomain` 設定をカスタマイズすることをお勧めします。 詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。




