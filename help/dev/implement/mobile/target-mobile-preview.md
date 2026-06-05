---
keywords: qa、プレビュー、プレビューリンク、モバイル、モバイルプレビュー
description: モバイルプレビューリンクを使用して、モバイルアプリのアクティビティのエンドツーエンドのQAを実行します。
title: ' [!DNL Adobe Target] Mobileでモバイルプレビューリンクを使用するにはどうすればよいですか？'
feature: Implement Mobile
exl-id: c0c4237a-de1f-4231-b085-f8f1e96afc13
TQID: https://experienceleague.adobe.com/ISZJ4lc8hhsQc3a-Mwz07US4fuEHobuvzCciFhmxEJk
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 578
ht-degree: 24%

---

# [!DNL Target] モバイルプレビュー

モバイルプレビューリンクを使用して、モバイルアプリのアクティビティに関する簡単なエンドツーエンドのQAを実行し、特別なテストデバイスを使用せずに、デバイスを使用してさまざまなエクスペリエンスに登録できます。

モバイルプレビュー機能を使用すると、モバイルアプリのアクティビティを本番稼働前に完全にテストできます。

## 前提条件

1. **サポートされているバージョンのSDKを使用：** モバイルプレビュー機能を使用するには、対応するアプリに適切なバージョンの[!DNL Adobe Mobile SDK]をダウンロードしてインストールする必要があります。

   適切なSDKをダウンロードする手順については、*[!DNL Adobe Experience Platform Mobile SDK]* ドキュメントの[現在のSDK バージョン ](https://developer.adobe.com/client-sdks/documentation/current-sdk-versions/){target=_blank}を参照してください。

1. **URL スキームを設定する：**&#x200B;プレビューリンクでは、URL スキームを使用してアプリを開きます。 プレビュー用の一意のURL スキームを指定します。

   詳しくは、*[!DNL Mobile SDK]* ドキュメントの「*データ接続UI*」の「[ ビジュアルプレビュー](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank}」を参照してください。

   詳細については、次のリンクを参照してください。

   * **iOs**: iOSのURL スキームの設定について詳しくは、*Apple Developer* web サイトの[ アプリのカスタム URL スキームの定義](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app){target=_blank}を参照してください。
   * **Android**: AndroidのURL スキームの設定について詳しくは、*Android Developers* web サイトの[ アプリ コンテンツへのディープリンクの作成](https://developer.android.com/training/app-links/deep-linking){target=_blank}を参照してください。

1. **`collectLaunchInfo` APIの設定（i0Sのみ）**

   詳しくは、*[!DNL Mobile SDK]* ドキュメントの「*データ接続UI*」の「[ ビジュアルプレビュー](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank}」を参照してください。

## プレビューリンクの生成

1. [!DNL Target] UIで、**[!UICONTROL 詳細オプション]** アイコン （垂直省略記号）をクリックし、**[!UICONTROL モバイルプレビューリンクの作成]**&#x200B;を選択します。

   ![alt画像](assets/mobile-preview-create.png)

1. プレビューするアクティビティを選択し、**[!UICONTROL モバイルプレビューリンクの生成]**&#x200B;をクリックします。

   >[!NOTE]
   >
   >選択できるのは、フォームベースの[!UICONTROL A/B テスト ]および[!UICONTROL  エクスペリエンスのターゲット設定] （XT）アクティビティのみです。

   ![alt画像](assets/mobile-preview-select-activities.png)

1. アプリの URL スキームを指定します。

   URL スキームは、iOSまたはAndroid アプリに存在するものと同じである必要があります。 必要に応じて、iOSとAndroidでこのプロセスを個別に繰り返します。

   ![alt画像](assets/mobile-preview-enter-url-scheme.png)

1. 「**[!UICONTROL モバイルプレビューリンクを生成]**」をクリックし、リンクをコピーします。

   ![alt画像](assets/mobile-preview-generate-and-copy.png)

## デバイスでのプレビュー

アプリをインストールしたデバイス上のモバイルブラウザーでリンクを開きます。 このアプリは、[!DNL Apple App Store]または[!DNL Google Play Store]からダウンロードした実稼動アプリです。 アプリは特別なビルドである必要はありません。 アクティブなプレビューリンクがある場合は、デバイス上のエクスペリエンスを表示できます。

1. モバイルブラウザーでリンクを開きます。

   前のセクションでコピーしたリンクを[!DNL Target] UIからモバイルデバイスに、テキスト、電子メール、または[!DNL Slack]を使用するなどの便利な方法で共有します。

   |![ディープリンクのプレビュー1](assets/mobile-preview-open-deeplink.png)|![ディープリンクのプレビュー2](assets/mobile-preview-open-app.png)|

   アプリが開き、[!DNL Target] [!UICONTROL  モバイルプレビューモード ]が開始されます。

1. 表示するエクスペリエンスの組み合わせを選択し、「**[!UICONTROL エクスペリエンスを開始]**」をクリックします。

   |![ モバイルプレビュー1](assets/mobile-preview-experience-selection-1.png)|![ モバイルプレビュー2](assets/mobile-preview-experience-result-1-france.png)|![ モバイルプレビュー3](assets/mobile-preview-experience-result-1-shipfree.png)|
|![ モバイルプレビュー4](assets/mobile-preview-experience-selection-2.png)|![ モバイルプレビュー5](assets/mobile-preview-experience-result-2-aus.png)|![ モバイルプレビュー6](assets/mobile-preview-experience-result-2-10off.png)|

## 制限事項

* 「**[!UICONTROL エクスペリエンスを開始]**」ボタンをクリックした後で新しいコンテンツを表示するには、ビューをもう一度読み込む必要があります。 最も簡単な方法は、一旦別の画面に切り替えた後、変更を適用する画面に戻ることです。
* モバイルプレビューは、API-19（KitKat）より前の Android バージョンではサポートされません。
