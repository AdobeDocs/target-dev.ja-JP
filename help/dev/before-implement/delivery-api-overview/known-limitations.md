---
title: Adobe Target配信 API の考慮事項と既知の制限事項
description: '[!UICONTROL Adobe Target Delivery API] を使用する際に考慮すべき考慮事項と既知の制限事項は何ですか？'
keywords: 配信 api
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: 49acf92bbe06dbcee36fef2b7394acd7ce37baad
workflow-type: tm+mt
source-wordcount: '134'
ht-degree: 6%

---

# 考慮事項と既知の制限事項

* [!DNL Target] Delivery API の認証はありません。
* この API は、Cookie またはリダイレクトの呼び出しを処理しません。
* HTTP/1.1 と HTTP/2 の両方のヘッダー名では大文字と小文字が区別されませんが、HTTP/2 では小文字のヘッダー名が使用されます。 詳しくは、[ ハイパーテキスト転送プロトコルバージョン 2 （HTTP/2）ドキュメント ](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank} を参照してください。

  新しいロードバランサーインフラストラクチャを通じて訪問者をルーティングするエンドポイントを使用すると、訪問者の接続は自動的に HTTP/2 にアップグレードされます。 このアップグレードプロセスは、リクエストヘッダーを小文字のヘッダーに変換するので、不正な形式と見なされません。

  この問題は、ライブラリが大文字と小文字を区別する（特に小文字にしない）リクエスト/応答ヘッダーを検索するように設定されている場合、顧客にとって問題となる可能性があります。
