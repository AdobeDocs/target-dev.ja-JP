---
keywords: qa，プレビュー，プレビューリンク，モバイル，モバイルプレビュー
description: モバイルプレビューリンクを使用して、モバイルアプリアクティビティに対してエンドツーエンドの QA を実行します。
title: モバイルプレビューリンクを [!DNL Adobe Target] 携帯？
feature: Implement Mobile
exl-id: c0c4237a-de1f-4231-b085-f8f1e96afc13
source-git-commit: 0bcfa16cb79644e7ce10e33daf6c8385104c197f
workflow-type: tm+mt
source-wordcount: '548'
ht-degree: 27%

---

# [!DNL Target] モバイルプレビュー

モバイルプレビューリンクを使用すると、モバイルアプリアクティビティに対してエンドツーエンドの簡単な QA を実行し、特別なテストデバイスを使用せずに、デバイスを使用して様々なエクスペリエンスに自分を登録できます。

モバイルプレビュー機能を使用すると、モバイルアプリアクティビティを実稼動環境に移行する前に完全にテストすることができます。

## 前提条件

1. **サポートされているバージョンの SDK を使用します。** モバイルプレビュー機能を使用するには、適切なバージョンの [!DNL Adobe Mobile SDK] を設定します。

   適切な SDK をダウンロードする手順については、 [現在の SDK のバージョン](https://developer.adobe.com/client-sdks/documentation/current-sdk-versions/){target=_blank} （内） *[!DNL Adobe Experience Platform Mobile SDK]* ドキュメント。

1. **URL スキームを設定する：**&#x200B;プレビューリンクでは、URL スキームを使用してアプリを開きます。プレビュー用の一意の URL スキームを指定します。

   詳しくは、 [ビジュアルプレビュー](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank} in *データ接続 UI での Target 拡張機能の設定* （内） *[!DNL Mobile SDK]* ドキュメント。

   詳細は次のリンクに記載されています。

   * **iOs**: iOSの URL スキームの設定について詳しくは、 [アプリのカスタム URL スキームの定義](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app){target=_blank} の *Apple Developer* web サイト。
   * **Android**:Android 用の URL スキームの設定について詳しくは、 [アプリコンテンツへのディープリンクの作成](https://developer.android.com/training/app-links/deep-linking){target=_blank} の *Android 開発者* web サイト。

1. **を設定します。 `collectLaunchInfo` API（i0S のみ）**

   詳しくは、 [ビジュアルプレビュー](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank} in *データ接続 UI での Target 拡張機能の設定* （内） *[!DNL Mobile SDK]* ドキュメント。

## プレビューリンクの生成

1. Adobe Analytics の [!DNL Target] UI で、 **[!UICONTROL その他のオプション]** アイコン（縦の省略記号）をクリックし、「 **[!UICONTROL モバイルプレビューリンクを作成]**.

   ![代替画像](assets/mobile-preview-create.png)

1. プレビューするアクティビティを選択し、「 **[!UICONTROL モバイルプレビューリンクを生成]**.

   >[!NOTE]
   >
   >選択できるのはフォームベースのみです [!UICONTROL A/B テスト] および [!UICONTROL エクスペリエンスのターゲット設定] (XT) アクティビティ

   ![代替画像](assets/mobile-preview-select-activities.png)

1. アプリの URL スキームを指定します。

   したがって、URL スキームは、iOSまたは Android アプリに存在するスキームと同じである必要があります。 必要に応じて、iOSと Android で、この手順を個別に繰り返します。

   ![代替画像](assets/mobile-preview-enter-url-scheme.png)

1. 「**[!UICONTROL モバイルプレビューリンクを生成」をクリック]** して、リンクをコピーします。

   ![代替画像](assets/mobile-preview-generate-and-copy.png)

## デバイスでのプレビュー

アプリをインストールしたデバイス上のモバイルブラウザーでリンクを開きます。このアプリは、 [!DNL Apple App Store] または [!DNL Google Play Store]. アプリは、特別なビルドである必要はありません。 アクティブなプレビューリンクがある場合は、デバイス上でエクスペリエンスを表示できます。

1. モバイルブラウザーでリンクを開きます。

   前の節でコピーしたリンクを [!DNL Target] テキスト、電子メール、 [!DNL Slack].

   |![ディープリンクのプレビュー1](assets/mobile-preview-open-deeplink.png)|![ディープリンクのプレビュー2](assets/mobile-preview-open-app.png)|

   アプリが開き、 [!DNL Target] [!UICONTROL モバイルプレビューモード].

1. 表示するエクスペリエンスの組み合わせを選択し、「エクスペリエンス **[!UICONTROL を開始」をクリック]** します。

   |![mobile preview1](assets/mobile-preview-experience-selection-1.png)|![モバイルプレビュー2](assets/mobile-preview-experience-result-1-france.png)|![モバイルプレビュー3](assets/mobile-preview-experience-result-1-shipfree.png)|
|![モバイルプレビュー4](assets/mobile-preview-experience-selection-2.png)|![モバイルプレビュー5](assets/mobile-preview-experience-result-2-aus.png)|![モバイルプレビュー6](assets/mobile-preview-experience-result-2-10off.png)|

## 制限事項

* 「**[!UICONTROL エクスペリエンスを開始]**」ボタンをクリックした後で新しいコンテンツを表示するには、ビューをもう一度読み込む必要があります。最も簡単な方法は、一旦別の画面に切り替えた後、変更を適用する画面に戻ることです。
* モバイルプレビューは、API-19（KitKat）より前の Android バージョンではサポートされません。
