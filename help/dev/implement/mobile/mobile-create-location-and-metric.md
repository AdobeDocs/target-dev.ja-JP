---
keywords: モバイルアプリ, モバイルアプリ位置, Target モバイルアプリ, モバイルターゲット位置, モバイルアプリ成功指標
description: iOS アプリで場所と成功指標を作成する方法を学ぶのに役立つサンプルコードを表示します。これにより、を使用してアプリをパーソナラ  [!DNL Adobe Target]  ズおよび最適化できます。
title: iOS アプリで場所  [!DNL Target]  成功指標を作成するにはどうすればよいですか？
feature: Implement Mobile
exl-id: 755c8b26-5c60-48fc-9e7e-5e97a25edb78
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '442'
ht-degree: 66%

---

# iOS -[!DNL Target] の場所と成功指標の作成

モバイルアプリで [!DNL Target] を使用するには、場所と成功指標を作成します。

>[!IMPORTANT]
>
>[!DNL Adobe Mobile] バージョン 4 のサポート。*x* SDK は 2021 年 8 月 31 日（PT）をもって終了し、[!DNL Adobe Target] モバイルユーザーには推奨されなくなりました。
>
>[ モバイルアプリ用Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/){target=_blank} は、モバイルアプリで [!DNL Adobe Experience Cloud] のソリューションおよびサービスを強化するための推奨ソリューションです。

この節では、アプリのテンプレートとして使用できるサンプルコードを紹介します。この節のサンプルには、iOS 用のコードがあります。Android でも同じパターンを使用します。Android固有の構文については、[Android SDK 4.x for Experience Cloudソリューション ](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/target-main.html) ガイドを参照してください。

>[!NOTE]
>
>使用可能なすべての [!DNL Target] メソッドのリストについては、[ モバイルドキュメント ](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-target-methods.html) を参照してください。

アプリに [!DNL Target] しい場所を作成してリクエストするには、主に次の 2 つの方法があります。

* `targetCreateRequestWithName`
* `targetLoadRequest`

1. [!DNL Target] の場所を作成します。

   リクエストを作成するためのサンプル呼び出しを以下に示します。

   ```
   // make your request 
   ADBTargetLocationRequest *myRequest = [ADBMobile targetCreateRequestWithName:@"heroBanner" 
                                                    defaultContent:@"default.png" 
                                                    parameters:nil];
   ```

   | パラメーター | 説明 |
   |---|---|
   | `ADBTargetLocationRequest *myRequest` | `myRequest` をアプリの `targetLocation` の名前に置き換えます。 |
   | `targetCreateRequestWithName:@"heroBanner"` | `heroBanner` を Target の `targetLocation` の名前に置き換えます。これは mbox 名と同じです。このヒーローバナーは Target インターフェイスに表示されます。 |
   | `defaultContent:@"default.png"` | 「`default.png`」を Target が応答しない場合にアプリが使用する値に置き換えます。 |
   | `parameters:nil` | プロファイルまたは mbox パラメーターを指定します。詳しくは「カスタムデータの引き渡し」を参照してください。 |

   リクエストを読み込むサンプルコードを以下に示します。

   ```
   // load your request 
   [ADBMobile targetLoadRequest:myRequest callback:^(NSString *content) { 
                                        // do something with content 
                                        heroImage.image = [UIImage imageNamed:content]; 
   }];
   ```

   | パラメーター | 説明 |
   |---|---|
   | `targetLoadRequest:myRequest` | `myRequest` をアプリの `targetLocation` の名前に置き換えます。 |
   | `NSString *content` | 「content」を Adobe から返される実際のコンテンツに置き換えます。文字列は XML、JSON、プレーン文字列のいずれかにできます。コードのこの部分を使用して、変数の定義、画像パスの設定、コントローラーフローやトランザクションポイントの表示などの任意の作業ができます。Target は UI で入力されたコンテンツを完全に同じ形式で返します。 |
   | `heroImage.image = [UIImage imageNamed:content];` | 例えば、コンテンツを取得して、ヒーローイメージのパスを設定します。 |

1. 成功指標を作成します。

   `targetCreateOrderConfirmRequestWithName` メソッドを使用して、アプリのコンバージョンや成功指標を追跡することができます。

   ```
   ADBTargetLocationRequest *req = [ADBMobile targetCreateOrderConfirmRequestWithName: "orderConfirm" 
                                              orderId: orderId 
                                              orderTotal: @"39.95" 
                                              productPurchasedId: _galleryItem.title 
                                              parameters: nil]; 
   [ADBMobile targetLoadRequest: req callback: nil];
   ```

   | パラメーター | 説明 |
   |---|---|
   | `orderId` | 一意の注文 ID を表す動的変数に置き換えます。 |
   | `@"39.95"` | 一意の注文合計額を表す動的変数に置き換えます。 |
   | `_galleryItem.title` | 購入商品のコンマ区切りのリストを表す動的変数に置き換えます。 |
   | `parameters: nil` | 追加パラメーターのオプションの辞書。 |

1. アプリを構築します。

   ステップ結果ターゲット場所の作成と成功指標のタグ付けが完了したら、A/Bテストを作成します。このアクティビティはフォームベースの Experience Composer を使用して作成できます。
