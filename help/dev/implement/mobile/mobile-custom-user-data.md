---
keywords: モバイルアプリ, モバイルアプリ送信データ, target モバイルアプリ, モバイルのカスタムユーザーデータ, モバイルアプリのカスタムデータ
description: 場所またはユーザーに関する追加情報を名前と値のペアとして [!DNL Adobe Target] に送信して、カスタムオーディエンスを作成する方法を説明します。
title: IOS アプリでカスタムユーザーデータを送信するにはどうすればよいですか？
feature: Implement Mobile
exl-id: 9cf8e8fd-1898-43b1-b339-d7a21cb35d57
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '417'
ht-degree: 55%

---

# iOS - カスタムユーザーデータの送信

場所またはユーザーに関する追加情報を名前と値のペアとして[!DNL Target]に送信できます。

>[!IMPORTANT]
>
>[!DNL Adobe Mobile] バージョン 4.*x* SDKのサポートは2021年8月31日（PT）をもって終了し、[!DNL Adobe Target] モバイルユーザーには推奨されなくなりました。
>
>[Adobe Experience Platform SDK モバイル版アプリ ](https://developer.adobe.com/client-sdks/documentation/){target=_blank}は、モバイル アプリで[!DNL Adobe Experience Cloud]のソリューションとサービスを強化するために推奨されるソリューションです。

この情報は、カスタムオーディエンス（例えば、25,000 マイルを超えたユーザー）の構築とレポートで使用できます。

[!DNL Target]呼び出しで送信できるパラメーターには、次の2種類があります。

* **mbox パラメーター**: Mbox パラメーターは、セッション間で永続的ではありません。
* **プロファイルパラメーター**: プロファイルパラメーターは訪問者プロファイルストアに保存され、セッション間で永続的です。 mbox パラメーターが保持されません。 一部のキーは予約されていますが、プロファイルと mbox の両方のパラメーターはカスタムのキーと値のペアとすることができます。

予約済みのキーがありますが、プロファイルと mbox の両方のパラメーターにはカスタムのキーと値のペアを含めることができます。

1. 辞書を作成します。

   まず、値を持つ辞書を作成して[!DNL Target]に渡します。 利便性のため、これを `welcomeMessageCampaign` メソッドの内部に追加すると、範囲について考慮する必要がなくなります。

   次にサンプルの辞書を示します。 コピー `(void)welcomeMessageCampaign` することができます。 `userLevel` や `userMiles` のようなキーの値は、この例ではハードコードされています。 通常は、対応する変数を使用して渡します。

   ```
   NSDictionary *targetParams = [[NSDictionary alloc] initWithObjectsAndKeys: 
                                 @"platinum",@"userLevel", 
                                 @26500,@"userMiles", 
                                 @"1067007",@"entity.id", 
                                 @"dealsapp.qa", @"host", 
                                 @"fashion",@"entity.categoryId", 
                                 @"millenial", @"profile.persona", 
                                 @"cohort_5", @"profile.cohort", 
                                 nil];
   ```

   * 接頭辞プロファイルのあるキー（例えば、`profile.persona`）は、ユーザーのプロファイルに保存されます。

     これらのプロファイル属性は、様々なアクティビティおよびチャネルで使用できます。

   * 接頭辞のないキー（例えば、`userMiles`）は mbox パラメーターです。

     これらのパラメーターは、セッション中にのみ利用できます。

   * 接頭辞エンティティのあるキー（例えば、`entity.category.id`）は製品のレコメンデーションで使用されます。

1. データを検証します。
   1. アプリケーション `didFinishLaunchingWithOptions` で、コメントを解除または追加 `[ADBMobile setDebugLogging:YES];` します。

      これにより、詳細なデバッグログが出力されます。
   1. アプリを構築します。
   1. Target の呼び出しにパラメーターが渡されていることを確認します。

      デバッグコンソールで Target の場所の名前を検索します。 先ほど渡したすべてのパラメーターとともに、`YOUR-CLIENT-CODE.tt.omtrdc.net` への呼び出しが表示されます。

      （画像をクリックして全幅に拡大します）。

      ![ デバッグコンソールのターゲットの場所](/help/dev/implement/mobile/assets/mobile-debug.png " デバッグコンソールのターゲットの場所"){zoomable="yes"}

   オーディエンスを作成し、[!DNL Target]でこれらのパラメーターを使用してコンテンツの表示を制限またはターゲットにすることができます。
