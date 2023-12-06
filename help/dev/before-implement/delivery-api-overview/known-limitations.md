---
title: Adobe Target Delivery API の考慮事項と既知の制限事項
description: を使用する際に考慮すべき考慮事項と既知の制限事項について [!UICONTROL Adobe Target Delivery API]?
keywords: 配信 api
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: 49acf92bbe06dbcee36fef2b7394acd7ce37baad
workflow-type: tm+mt
source-wordcount: '138'
ht-degree: 6%

---

# 注意点と既知の制限事項

* 次に対する認証がありません： [!DNL Target] 配信 API。
* この API は、Cookie またはリダイレクトの呼び出しを処理しません。
* HTTP/1.1 と HTTP/2 のヘッダー名は、大文字と小文字が区別されません。ただし、HTTP/2 では小文字のヘッダー名が強制されます。 詳しくは、 [ハイパーテキスト転送プロトコルバージョン 2(HTTP/2) ドキュメント](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank}.

  新しいロードバランサーインフラストラクチャを通じて訪問者をルーティングするエンドポイントを使用する場合、その接続は自動的に HTTP/2 にアップグレードされます。 このアップグレードプロセスでは、リクエストヘッダーを小文字のヘッダーに変換して、正しくないと見なされないようにします。

  この問題は、（特に小文字ではなく）大文字と小文字を区別するリクエスト/応答ヘッダーを検索するようにライブラリが設定されている場合、お客様にとって問題になる可能性があります。
