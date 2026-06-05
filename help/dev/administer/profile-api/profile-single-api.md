---
title: Adobe Target Single Profile Update API
description: ' [!DNL Adobe Target] [!UICONTROL 単一プロファイル更新API]を使用して、1人の訪問者のプロファイルデータを [!DNL Target]に送信する方法について説明します。'
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 4e022db3-215f-461b-9222-38ce2f2dbc28
TQID: https://experienceleague.adobe.com/HEjGkrgixufe9wQvaPAljSlZRSaF-idgwKYWs3cuoJ0
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 6fac79420aef0a73c109b2c19f363266c1f8027a
workflow-type: tm+mt
source-wordcount: 396
ht-degree: 4%

---

# [!DNL Adobe Target Single Profile Update API]

[!DNL Adobe Target] [!UICONTROL 単一プロファイル更新API]を使用すると、単一ユーザーのプロファイル更新を送信できます。 [!UICONTROL 単一プロファイル更新API]は、[!UICONTROL 一括プロファイル更新API]とほぼ同じですが、1人の訪問者プロファイルが一度に更新され、.cvs ファイルではなくAPI呼び出しでインライン化されます。

[!UICONTROL 単一プロファイル更新API]は、[!DNL Target]を実装していないチャネルで発生しているトランザクションに関連して更新を行う必要がある場合に一般的に使用されます。 例えば、オフラインアクションを実行する1人の訪問者のプロファイルを更新するとします。 アクションには、コールセンターへの連絡、ローンの資金調達、店舗でのロイヤルティカードの利用、キオスクへのアクセスなどが含まれます。

[!UICONTROL 単一プロファイル更新API]の利点は次のとおりです。

* プロファイル属性の数に上限がありません。
* サイト経由で送信されたプロファイル属性は、APIやその逆の方法で更新できます。

## 注意事項

* [!UICONTROL 単一プロファイル更新API]は、任意のローリング 24時間で100万件の更新を実行することに制限されています。
* 更新は通常1時間以内に行われますが、反映されるまでに24時間かかる場合があります。

  さらに更新を送信する必要がある場合、または更新を短い時間枠で処理する必要がある場合は、クライアントサイドの更新（推奨）または[!DNL Adobe Target] サーバーサイド [配信API](/help/dev/implement/delivery-api/overview.md)を介してトランザクションプロファイルの更新を送信することを検討してください。

* [!UICONTROL 単一プロファイル更新API]はサーバー間APIであり、Web ページ内で動作するように設計されていません。 Web ページ内から訪問者プロファイルを更新するには、[trackEvent （） &#x200B;](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)関数または[配信API](/help/dev/implement/delivery-api/overview.md)を使用できます。

## 形式

形式`profile.paramName=value`でプロファイル パラメーターを指定します。

`pcId`のプロファイルを更新するには、次を使用します。

```
https://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr=0&profile.attr2=1...
```

`mbox3rdPartyId`のプロファイルを更新するには、次を使用します。

```
shell http://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mbox3rdPartyId=123456&profile.attr=0&profile.attr2=1...
```

[!UICONTROL 単一プロファイル更新API]は更新用です。 何も見つからない場合は、プロファイルは作成されません。

## メモ

* パラメーターと値は、UTF-8を使用してURL エンコードする必要があります。
* パラメーター形式は`profile.paramName`です。
* すべてのpcIdおよびmbox3rdPartyIdに対して、すべてのパラメーター値が存在する必要はありません。
* パラメーターと値は、大文字と小文字を区別します。
* GETとPOSTの両方がサポートされています。
* 制限の現在のサイズ制限は、GETの場合は8 KB、POSTの場合は60 KBです。

## 応答

上記のリクエストのサンプル応答は次のようになります。

`trueRequest successfully submitted`

この応答は、応答が送信され、まもなく処理されることを示します。
