---
title: Adobe Target Delivery APIの考慮事項と既知の制限事項
description: '[!UICONTROL Adobe Target Delivery API]を使用する際に考慮すべき考慮事項と既知の制限事項は何ですか？'
keywords: Delivery API
exl-id: 49fe13b0-efcb-4b1c-a4cb-03b64fbd9214
feature: APIs/SDKs
TQID: https://experienceleague.adobe.com/LCgGZONQpYfw6JxNCNc2Iu13Mft8Zfx-3Uxvm2EeUVk
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 158
ht-degree: 6%

---

# 考慮事項と既知の制限事項

次の情報は、[!DNL Adobe Target] [!DNL Delivery API]を使用する際の考慮事項と既知の制限事項を示しています。

* [!DNL Target]配信APIの認証がありません。
* この API は、Cookie またはリダイレクトの呼び出しを処理しません。
* HTTP/1.1とHTTP/2 ヘッダー名は両方とも大文字と小文字を区別しません。ただし、HTTP/2では小文字のヘッダー名が適用されます。 詳しくは、[&#x200B; ハイパーテキスト転送プロトコルバージョン 2 （HTTP/2）のドキュメント &#x200B;](https://www.rfc-editor.org/rfc/rfc7540#section-8.1.2){target=_blank}を参照してください。

  新しいロードバランサーインフラストラクチャを介して訪問者をルーティングするエンドポイントを使用する場合、その接続は自動的にHTTP/2にアップグレードされます。 このアップグレードプロセスでは、リクエストヘッダーを小文字のヘッダーに変換します。これにより、形式が正しくないと見なされることはありません。

  この問題は、ライブラリが大文字と小文字を区別しない（特に小文字を区別しない）リクエスト/応答ヘッダーを検索するように設定されている場合、顧客にとって問題になる可能性があります。
