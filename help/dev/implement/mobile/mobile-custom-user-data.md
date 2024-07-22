---
keywords: モバイルアプリ, モバイルアプリ送信データ, target モバイルアプリ, モバイルのカスタムユーザーデータ, モバイルアプリのカスタムデータ
description: カスタムオーディエンスの構築に役立つ、場所またはユーザーに関する追加情報を名前  [!DNL Adobe Target]  値のペアとして送信する方法を説明します。
title: iOS アプリでカスタムユーザーデータを送信するにはどうすればよいですか？
feature: Implement Mobile
exl-id: 9cf8e8fd-1898-43b1-b339-d7a21cb35d57
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '407'
ht-degree: 55%

---

# iOS - カスタムユーザーデータの送信

場所やユーザーに関する追加情報を送信し、名前と値のペアとして [!DNL Target] すことができます。

>[!IMPORTANT]
>
>[!DNL Adobe Mobile] バージョン 4 のサポート。*x* SDK は 2021 年 8 月 31 日（PT）をもって終了し、[!DNL Adobe Target] モバイルユーザーには推奨されなくなりました。
>
>[ モバイルアプリ用Adobe Experience Platform SDK](https://developer.adobe.com/client-sdks/documentation/){target=_blank} は、モバイルアプリで [!DNL Adobe Experience Cloud] のソリューションおよびサービスを強化するための推奨ソリューションです。

この情報は、カスタムオーディエンス（例えば、25,000 マイルを超えたユーザー）の構築とレポートで使用できます。

[!DNL Target] 呼び出しで送信できるパラメーターには、次の 2 種類があります。

* **mbox パラメーター**:mbox パラメーターは、セッションをまたいで永続的ではありません。
* **プロファイルパラメーター**：プロファイルパラメーターは訪問者プロファイルストアに格納され、セッションをまたいで保持されます。 mbox パラメーターが保持されない。 一部のキーは予約されていますが、プロファイルと mbox の両方のパラメーターはカスタムのキーと値のペアとすることができます。

予約済みのキーがありますが、プロファイルと mbox の両方のパラメーターにはカスタムのキーと値のペアを含めることができます。

1. 辞書を作成します。

   まず、[!DNL Target] に渡す値を使用して辞書を作成します。 利便性のため、これを `welcomeMessageCampaign` メソッドの内部に追加すると、範囲について考慮する必要がなくなります。

   次にサンプルの辞書を示します。コピー `(void)welcomeMessageCampaign` することができます。`userLevel` や `userMiles` のようなキーの値は、この例ではハードコードされています。通常は、対応する変数を使用して渡します。

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

   * プレフィックスプロファイルのあるキー（例えば、`profile.persona`）は、ユーザーのプロファイルに保存されます。

     これらのプロファイル属性は、様々なアクティビティおよびチャネルで使用できます。

   * プレフィックスのないキー（例えば、`userMiles`）は mbox パラメーターです。

     これらのパラメーターは、セッション中にのみ利用できます。

   * プレフィックスエンティティのあるキー（例えば、`entity.category.id`）は製品のレコメンデーションで使用されます。

1. データを検証します。
   1. アプリケーション `didFinishLaunchingWithOptions` で、コメントを解除または追加 `[ADBMobile setDebugLogging:YES];` します。

      これにより、詳細なデバッグログが出力されます。
   1. アプリを構築します。
   1. Target の呼び出しにパラメーターが渡されていることを確認します。

      デバッグコンソールで Target の場所の名前を検索します。先ほど渡したすべてのパラメーターとともに、`YOUR-CLIENT-CODE.tt.omtrdc.net` への呼び出しが表示されます。

      （全幅に拡大するには、画像をクリックします）。

      ![ デバッグコンソールのターゲットの場所 ](/help/dev/implement/mobile/assets/mobile-debug.png " デバッグコンソールのターゲットの場所 "){zoomable="yes"}

   これらのパラメーターを [!DNL Target] 用して、オーディエンスを作成し、コンテンツの表示を制限またはターゲットにすることができます。
