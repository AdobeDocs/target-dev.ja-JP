---
title: Adobe Target配信 API の考慮事項と既知の制限事項
description: '[!UICONTROL Adobe Target Delivery API] を使用する際に考慮すべき考慮事項と既知の制限事項は何ですか？'
keywords: 配信 api
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: 413b16ed0b098de6914558fa29b9ca59aaba958e
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 3%

---

# 考慮事項と既知の制限事項

次の情報では、[!DNL Adobe Target] [!DNL Delivery API] を使用する際の考慮事項と既知の制限事項について説明します。

* [!DNL Target] Delivery API の認証はありません。
* この API は、Cookie またはリダイレクトの呼び出しを処理しません。
* HTTP/1.1 と HTTP/2 の両方のヘッダー名では大文字と小文字が区別されませんが、HTTP/2 では小文字のヘッダー名が使用されます。 詳しくは、[ ハイパーテキスト転送プロトコルバージョン 2 （HTTP/2）ドキュメント ](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank} を参照してください。

  新しいロードバランサーインフラストラクチャを通じて訪問者をルーティングするエンドポイントを使用すると、訪問者の接続は自動的に HTTP/2 にアップグレードされます。 このアップグレードプロセスは、リクエストヘッダーを小文字のヘッダーに変換するので、不正な形式と見なされません。

  この問題は、ライブラリが大文字と小文字を区別する（特に小文字にしない）リクエスト/応答ヘッダーを検索するように設定されている場合、顧客にとって問題となる可能性があります。

* [!DNL Recommendations] から [!UICONTROL Catalog] [!DNL Delivery API] を更新する場合は注意が必要です。 [!DNL Delivery API] は公開されているので、Recommendations カタログのクリック可能な項目への入力には使用しないでください。 無効なコンテンツを追加すると、カタログが汚染される可能性があります。

  **ベストプラクティス**:

  [!DNL Delivery API] は、次のようなカタログ属性の更新にのみ使用します。
   * 頻繁に変更する（価格、株価など）。
   * Web サイトで簡単に検証できる事前定義済みの形式に従います。
   * クリック可能な項目やその他の未検証のコンテンツの追加や変更には使用しないでください。
   * 必要に応じて、配信 API を使用してカタログの更新を無効にするようカスタマーサポートにリクエストできます。

  詳しくは、[[!UICONTROL Adobe Target Delivery API]](https://developer.adobe.com/target/implement/delivery-api/){target=_blank} ドキュメントを参照してください。
