---
title: Adobe Target単一プロファイル更新 API
description: ' [!DNL Adobe Target] [!UICONTROL Single Profile Update API] を使用して、1 人の訪問者のプロファイルデータを  [!DNL Target] に送信する方法を説明します。'
feature: APIs/SDKs
contributors: https://github.com/icaraps
exl-id: 4e022db3-215f-461b-9222-38ce2f2dbc28
source-git-commit: e2462d12cf58ab5a588c13a96df5e6abafb9d675
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 3%

---

# [!DNL Adobe Target Single Profile Update API]

[!DNL Adobe Target] [!UICONTROL Single Profile Update API] を使用すると、1 人のユーザーのプロファイルの更新を送信できます。 [!UICONTROL Single Profile Update API] は [!UICONTROL Bulk Profile Update API] とほぼ同じですが、.cvs ファイルではなく API 呼び出しを使用してインラインで一度に 1 つの訪問者プロファイルが更新されます。

[!UICONTROL Single Profile Update API] とは、通常、[!DNL Target] を実装していないチャネルで発生したトランザクションに関連して更新を行う必要がある場合に使用されます。 例えば、何らかのオフラインアクションを実行する 1 人の訪問者のプロファイルを更新するとします。 コールセンターへのアクセス、ローンの支払い、店舗でのロイヤルティカードの使用、キオスクへのアクセスなどがアクションの例になります。

[!UICONTROL Single Profile Update API] の利点は次のとおりです。

* プロファイル属性の数に上限がありません。
* サイトを介して送信されたプロファイル属性は、API を使用して更新できます。その逆も可能です。

## 注意事項

* [!UICONTROL Single Profile Update API] は、24 時間のローリングで 100 万回の更新を実行するように制限されています。
* 更新は通常 1 時間以内に行われますが、反映されるまでに 24 時間かかる場合があります。

  さらに多くの更新を送信する必要がある場合、または更新を短時間で処理する必要がある場合は、クライアントサイドの更新（推奨）または [!DNL Adobe Target] サーバーサイド [ 配信 API](/help/dev/implement/delivery-api/overview.md) を使用して、トランザクションプロファイルの更新を送信することを検討します。

* [!UICONTROL Single Profile Update API] はサーバー間 API であり、web ページ内で動作するように設計されていません。 Web ページ内から訪問者プロファイルを更新するには、[trackEvent （） ](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) 関数または [ 配信 API](/help/dev/implement/delivery-api/overview.md) を使用します。

## 形式

形式 `profile.paramName=value` でプロファイルパラメーターを指定します。

`pcId` ーザーのプロファイルを更新するには、次を使用します。

``````
https://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr=0&profile.attr2=1...
``````

ア `mbox3rdPartyId` ットのプロファイルを更新するには、次を使用します。

``````
shell http://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mbox3rdPartyId=123456&profile.attr=0&profile.attr2=1...
``````

[!UICONTROL Single Profile Update API] は更新専用です。 何も見つからない場合、プロファイルは作成されません。

## メモ

* パラメーターと値は、UTF-8 を使用して URL エンコードする必要があります。
* パラメーターの形式は `profile.paramName` です。
* すべての pcIds と mbox3rdPartyIds に対して、すべてのパラメーター値が存在する必要があるわけではありません。
* パラメーターと値は、大文字と小文字を区別します。
* GETとPOSTの両方がサポートされています。
* 制限の現在のサイズ制限は、GETで 8 KB、POSTで 60 KB です。

## 応答

上記のリクエストに対する応答のサンプルは、次のようになります。

`trueRequest successfully submitted`

この応答は、応答が送信され、まもなく処理されることを示しています。
