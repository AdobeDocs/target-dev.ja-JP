---
title: Adobe Target Delivery API の考慮事項と既知の制限事項
description: を使用する際に考慮すべき考慮事項と既知の制限事項について [!UICONTROL Adobe Target Delivery API]?
keywords: 配信 api
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '185'
ht-degree: 7%

---

# 考慮事項と既知の制限事項

* 次に対する認証がありません： [!DNL Target] 配信 API。
* この API は、Cookie またはリダイレクトの呼び出しを処理しません。
* のサポート [!UICONTROL Automated Personalization] (AP) および [!UICONTROL Recommendations] アクティビティ：この API には、コンテンツを取得するための 2 つのモード（実行モードとプリフェッチモード）があります。 プリフェッチモードは、次の場合にのみ使用できます： [!UICONTROL AB テスト] および [!UICONTROL エクスペリエンスのターゲット設定] (XT) アクティビティ 次のプリフェッチモードを使用しない： [!UICONTROL Automated Personalization] (AP), [!UICONTROL 自動配分], [!UICONTROL 自動ターゲット]または [!UICONTROL Recommendations] アクティビティのタイプ。
* HTTP/1.1 と HTTP/2 のヘッダー名は、大文字と小文字が区別されません。ただし、HTTP/2 では小文字のヘッダー名が強制されます。 詳しくは、 [ハイパーテキスト転送プロトコルバージョン 2(HTTP/2) ドキュメント](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank}.

  新しいロードバランサーインフラストラクチャを通じて訪問者をルーティングするエンドポイントを使用する場合、その接続は自動的に HTTP/2 にアップグレードされます。 このアップグレードプロセスでは、リクエストヘッダーを小文字のヘッダーに変換して、正しくないと見なされないようにします。

  この問題は、（特に小文字ではなく）大文字と小文字を区別するリクエスト/応答ヘッダーを検索するようにライブラリが設定されている場合、お客様にとって問題になる可能性があります。
