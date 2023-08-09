---
keywords: qa，プレビュー，プレビューリンク，モバイル，モバイルプレビュー
description: モバイルプレビューリンクを使用して、モバイルアプリアクティビティに対してエンドツーエンドの QA を実行します。 特別なテストデバイスを使用せずに、自分を異なるエクスペリエンスに登録できます。
title: モバイルプレビューリンクを [!DNL Target] 携帯？
feature: Implement Mobile
exl-id: c0c4237a-de1f-4231-b085-f8f1e96afc13
source-git-commit: 97c96e63f9121793a83b445ad3dc33c5d094509a
workflow-type: tm+mt
source-wordcount: '555'
ht-degree: 51%

---

# [!DNL Target] モバイルプレビュー

モバイルのプレビューリンクを使用して、モバイルアプリケーションアクティビティの簡易的なエンドツーエンドの QA を実行できます。特別なテスト用のデバイスがなくても、ご利用のデバイス上で様々なエクスペリエンスを確認できます。

## 概要

モバイルプレビュー機能を使用して、モバイルアプリアクティビティを実稼動環境に投入する前に完全にテストすることができます。

## 前提条件

1. **サポートされているバージョンの SDK を使用します。** モバイルプレビュー機能を使用するには、適切なバージョンのAdobeMobile SDK を対応するアプリにダウンロードしてインストールする必要があります。

   適切な SDK をダウンロードする手順については、 [現在の SDK のバージョン](https://developer.adobe.com/client-sdks/documentation/current-sdk-versions/){target=_blank} （内） *[!DNL Adobe Experience Platform Mobile SDK]* ドキュメント。

1. **URL スキームを設定する：**&#x200B;プレビューリンクでは、URL スキームを使用してアプリを開きます。プレビュー用に一意の URL スキームを指定する必要があります。

   詳しくは、 [ビジュアルプレビュー](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank} in *Adobe Target* （内） *[!DNL Adobe Experience Platform Mobile SDK]* ドキュメント。

   詳細は次のリンクに記載されています。

   * **iOs**: iOSの URL スキームの設定について詳しくは、 [アプリのカスタム URL スキームの定義](https://developer.apple.com/documentation/xcode/defining-a-custom-url-scheme-for-your-app){target=_blank} をApple Developer Web サイトに追加します。
   * **Android**:Android 用の URL スキームの設定について詳しくは、 [アプリコンテンツへのディープリンクの作成](https://developer.android.com/training/app-links/deep-linking){target=_blank} （ Android Developers の Web サイト）を参照してください。

1. **設定 `collectLaunchInfo` API（i0S のみ）**

   詳しくは、 [ビジュアルプレビュー](https://developer.adobe.com/client-sdks/documentation/adobe-target/#visual-preview){target=_blank} in *Adobe Target* （内） *[!DNL Adobe Experience Platform Mobile SDK]* ドキュメント。

## プレビューリンクの生成

1. Adobe Analytics の [!DNL Target] UI で、 **[!UICONTROL その他のオプション]** アイコン（縦の省略記号）をクリックし、「 **[!UICONTROL モバイルプレビューを作成]**.

   ![代替画像](assets/mobile-preview-create.png)

1. プレビューするアクティビティを選択し、「 **[!UICONTROL モバイルプレビューリンクを生成]**.

   >[!NOTE]
   >
   >フォームベースの AB および XT アクティビティのみを選択できます。

   ![代替画像](assets/mobile-preview-select-activities.png)

1. アプリの URL スキームを指定します。

   これは、iOS または Android アプリに存在する URL スキームと同じである必要があります。必要に応じて、iOS と Android でこのプロセスを別々に繰り返します。

   ![代替画像](assets/mobile-preview-enter-url-scheme.png)

1. 「**[!UICONTROL モバイルプレビューリンクを生成」をクリック]** して、リンクをコピーします。

   ![代替画像](assets/mobile-preview-generate-and-copy.png)

## デバイスでのプレビュー

アプリをインストールしたデバイス上のモバイルブラウザーでリンクを開きます。Apple App Store または Google Play ストアからダウンロードした製品アプリを使用できます。特殊なビルドである必要はありません。アクティブなプレビューリンクがある場合は、デバイス上でエクスペリエンスを表示できます。

1. モバイルブラウザーでリンクを開きます。

   前の手順でコピーしたリンクを [!DNL Target] テキスト、電子メール、Slackなどを使用する便利な方法でモバイルデバイスに UI を追加できます。

   |![ディープリンクのプレビュー1](assets/mobile-preview-open-deeplink.png)|![ディープリンクのプレビュー2](assets/mobile-preview-open-app.png)|

   アプリが開き、 [!DNL Target] モバイルプレビューモード。

1. 表示するエクスペリエンスの組み合わせを選択し、「エクスペリエンス **[!UICONTROL を開始」をクリック]** します。

   |![mobile preview1](assets/mobile-preview-experience-selection-1.png)|![モバイルプレビュー2](assets/mobile-preview-experience-result-1-france.png)|![モバイルプレビュー3](assets/mobile-preview-experience-result-1-shipfree.png)|
|![モバイルプレビュー4](assets/mobile-preview-experience-selection-2.png)|![モバイルプレビュー5](assets/mobile-preview-experience-result-2-aus.png)|![モバイルプレビュー6](assets/mobile-preview-experience-result-2-10off.png)|

## 制限事項

* 「**[!UICONTROL エクスペリエンスを開始]**」ボタンをクリックした後で新しいコンテンツを表示するには、ビューをもう一度読み込む必要があります。最も簡単な方法は、一旦別の画面に切り替えた後、変更を適用する画面に戻ることです。
* モバイルプレビューは、API-19（KitKat）より前の Android バージョンではサポートされません。
