---
keywords: qa，プレビュー，プレビューリンク，モバイル，モバイルプレビュー
description: モバイルプレビューリンクを使用して、モバイルアプリアクティビティに対してエンドツーエンドの QA を実行します。
title: モバイルプレビューリンクを [!DNL Adobe Target] 携帯？
feature: Implement Mobile
exl-id: c0c4237a-de1f-4231-b085-f8f1e96afc13
source-git-commit: 15e42d0fb049f9243ff5468ff5f22a8e79c55c79
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 23%

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

1. Adobe Analytics の [!DNL Target] UI で、 **[!UICONTROL More Options]** アイコン（縦の省略記号）をクリックし、「 **[!UICONTROL Create Mobile Preview Link]**.

   ![代替画像](assets/mobile-preview-create.png)

1. プレビューするアクティビティを選択し、「 **[!UICONTROL Generate Mobile Preview Link]**.

   >[!NOTE]
   >
   >選択できるのはフォームベースのみです [!UICONTROL A/B Test] および [!UICONTROL Experience Targeting] (XT) アクティビティ

   ![代替画像](assets/mobile-preview-select-activities.png)

1. アプリの URL スキームを指定します。

   URL スキームは、iOSまたは Android アプリに存在するものと同じである必要があります。 必要に応じて、iOSと Android で、この手順を個別に繰り返します。

   ![代替画像](assets/mobile-preview-enter-url-scheme.png)

1. クリック **[!UICONTROL Generate Mobile Preview Link]**&#x200B;をクリックし、リンクをコピーします。

   ![代替画像](assets/mobile-preview-generate-and-copy.png)

## デバイスでのプレビュー

アプリをインストールしたデバイス上のモバイルブラウザーでリンクを開きます。このアプリは、 [!DNL Apple App Store] または [!DNL Google Play Store]. アプリは、特別なビルドである必要はありません。 アクティブなプレビューリンクがある場合は、デバイス上でエクスペリエンスを表示できます。

1. モバイルブラウザーでリンクを開きます。

   前の節でコピーしたリンクを [!DNL Target] テキスト、電子メール、 [!DNL Slack].

   |![ディープリンクのプレビュー1](assets/mobile-preview-open-deeplink.png)|![ディープリンクのプレビュー2](assets/mobile-preview-open-app.png)|

   アプリが開き、 [!DNL Target] [!UICONTROL Mobile Preview Mode].

1. 表示するエクスペリエンスの組み合わせを選択し、「 **[!UICONTROL Launch Experiences]**.

   |![mobile preview1](assets/mobile-preview-experience-selection-1.png)|![モバイルプレビュー2](assets/mobile-preview-experience-result-1-france.png)|![モバイルプレビュー3](assets/mobile-preview-experience-result-1-shipfree.png)|
|![モバイルプレビュー4](assets/mobile-preview-experience-selection-2.png)|![モバイルプレビュー5](assets/mobile-preview-experience-result-2-aus.png)|![モバイルプレビュー6](assets/mobile-preview-experience-result-2-10off.png)|

## 制限事項

* 新しいコンテンツを表示するには、ビューを再度読み込む必要があります。 **[!UICONTROL Launch Experiences]** 」ボタンがクリックされたときに表示されます。 最も簡単な方法は、一旦別の画面に切り替えた後、変更を適用する画面に戻ることです。
* モバイルプレビューは、API-19（KitKat）より前の Android バージョンではサポートされません。
