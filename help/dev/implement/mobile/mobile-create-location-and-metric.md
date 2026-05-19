---
keywords: モバイルアプリ, モバイルアプリ位置, Target モバイルアプリ, モバイルターゲット位置, モバイルアプリ成功指標
description: サンプルコードを表示して、iOS アプリで場所と成功指標を作成する方法を学び、 [!DNL Adobe Target] を使用してアプリをパーソナライズおよび最適化できるようにします。
title: IOS アプリで [!DNL Target] 場所と成功指標を作成するにはどうすればよいですか？
feature: Implement Mobile
exl-id: 755c8b26-5c60-48fc-9e7e-5e97a25edb78
TQID: https://experienceleague.adobe.com/frolzqCgdL0iz5Z3E8OaJmP6yiVq7jEYiWn6LD4bocA
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
level_v2:
  - id: e8ccd51f-da0d-4e3b-939b-e30d5ebb1ea5
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 469
ht-degree: 63%

---

# iOS - [!DNL Target]の場所と成功指標の作成

モバイルアプリで[!DNL Target]を使用するには、場所と成功指標を作成します。

>[!IMPORTANT]
>
>[!DNL Adobe Mobile] バージョン 4.*x* SDKのサポートは2021年8月31日（PT）をもって終了し、[!DNL Adobe Target] モバイルユーザーには推奨されなくなりました。
>
>[Adobe Experience Platform SDK モバイル版アプリ &#x200B;](https://developer.adobe.com/client-sdks/documentation/){target=_blank}は、モバイル アプリで[!DNL Adobe Experience Cloud]のソリューションとサービスを強化するために推奨されるソリューションです。

この節では、アプリのテンプレートとして使用できるサンプルコードを紹介します。 この節のサンプルには、iOS 用のコードがあります。 Android でも同じパターンを使用します。 Android固有の構文については、[Android SDK 4.x for Experience Cloud Solutions](https://experienceleague.adobe.com/docs/mobile-services/android/target-android/target-main.html?lang=ja) ガイドを参照してください。

>[!NOTE]
>
>使用可能なすべての[!DNL Target] メソッドの一覧については、[&#x200B; モバイルドキュメント &#x200B;](https://experienceleague.adobe.com/docs/mobile-services/ios/target-ios/c-target-methods.html?lang=ja)を参照してください。

アプリで[!DNL Target]の場所を作成してリクエストを行うには、主に2つの方法があります。

* `targetCreateRequestWithName`
* `targetLoadRequest`

1. [!DNL Target]の場所を作成します。

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
   | `targetCreateRequestWithName:@"heroBanner"` | `heroBanner` を Target の `targetLocation` の名前に置き換えます。 これは mbox 名と同じです。 このヒーローバナーは Target インターフェイスに表示されます。 |
   | `defaultContent:@"default.png"` | 「`default.png`」を Target が応答しない場合にアプリが使用する値に置き換えます。 |
   | `parameters:nil` | プロファイルまたは mbox パラメーターを指定します。 詳しくは「カスタムデータの引き渡し」を参照してください。 |

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
   | `NSString *content` | 「content」を Adobe から返される実際のコンテンツに置き換えます。 文字列は XML、JSON、プレーン文字列のいずれかにできます。 コードのこの部分を使用して、変数の定義、画像パスの設定、コントローラーフローやトランザクションポイントの表示などの任意の作業ができます。 Target は UI で入力されたコンテンツを完全に同じ形式で返します。 |
   | `heroImage.image = [UIImage imageNamed:content];` | 例えば、コンテンツを取得して、ヒーロー画像のパスを設定します。 |

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

   ステップ結果ターゲット場所の作成と成功指標のタグ付けが完了したら、A/Bテストを作成します。 このアクティビティはフォームベースの Experience Composer を使用して作成できます。
