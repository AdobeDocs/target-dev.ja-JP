---
keywords: モバイルアプリ，aep sdk，ネイティブアプリ，Web ビュー，ネイティブ；swift,adobe experience platform モバイル sdk，モバイル sdk，ネイティブコード
description: の実装方法を学ぶ [!DNL Adobe Target] と [!DNL AEP Mobile SDK] は、web ビューを含むネイティブアプリで使用されます。
title: 実装方法 [!DNL Adobe Target] web ビューを持つネイティブコードを使用するモバイルアプリの
feature: Implement Mobile
role: Developer
source-git-commit: c0fda36cb5472d71438c47b8b484716003da4214
workflow-type: tm+mt
source-wordcount: '600'
ht-degree: 0%

---


# 実装方法 [!DNL Target] と [!DNL AEP Mobile SDK] web ビューを使用するネイティブアプリ内

この記事では、の実装に関するベストプラクティスを紹介します [!DNL Adobe Target] を使用して、web ビューでネイティブコードを使用するモバイルアプリの [!DNL Adobe Experience Platform Mobile SDK].

この記事では、 [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/getting-started/){target=_blank} and a [!DNL Target] integration written in [Swift from the GitHub repository](https://github.com/adobe/aep-sdk-app/){target=_blank}.

実際の環境では、エンタープライズアプリはモバイルアプリで Web ビューを使用している可能性が高くなります。 Web ビューは、URL を使用して Web ページを読み込むコンテナです。 コンテナは、コントロールのないブラウザーウィンドウに似ています。 iOSでは、Web ビューコンテナは、Web ページを処理する際に Safari ブラウザーとして機能します。

## 前提条件

を使い始めるには、以下を実行します。 [!DNL Adobe Experience Platform Mobile SDK]を使用する場合は、いくつかの前提条件のタスクを実行する必要があります。

詳しくは、 [Adobe Target](https://developer.adobe.com/client-sdks/documentation/adobe-target/){target=_blank} in the [[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/){target=_blank} ドキュメント。

## ネイティブコードを Web ビューと同期

導入時の課題 [!DNL Target] web ビューを持つネイティブアプリでは、 [!DNL Adobe Experience Platform Mobile SDK] には、次に必要なすべての識別子が既に生成されています： [!DNL Adobe] ソリューションをシームレスに動作させます。 ただし、これらの識別子はネイティブプラットフォーム環境上にないので、Web ビューにはまだ識別情報が表示されません。 したがって、訪問者 ID が Web 環境内で保持されるように、Web ビューに一部の SDK 識別子を渡すブリッジを作成する必要があります。 この処理が適切におこなわれないと、訪問が重複してレポートに影響を与えます。

幸いにも、 [!DNL Adobe Experience Platform Mobile SDK] 生成する便利な方法を提供します。 [!DNL Adobe] web ビューが同じ訪問者に対して使用および保持するために必要なパラメーターを次のサンプルコードに示します。

```swift
Identity.appendTo(url: URL(string: url), completion: {appendedURL, error in
  print("appendedURL \(String(describing: appendedURL))")
  // load the url with ECID on the main thread
  DispatchQueue.main.async {
    let request = NSMutableURLRequest(url: appendedURL!)
    self.webView.load(request as URLRequest)
  }
});
```

詳しくは、 `Identity.appendTo` メソッドを参照し、メソッドの使用方法の例については、 [Swift /例](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/tabs/api-reference/){target=_blank} （内） *Mobile SDK ドキュメント*.

使用 `Identity.appendTo`、この URL:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2
```

変換先：

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg
```

ご覧の通り、 `adobe_mc` パラメーターを追加しました。 このパラメーターには、次のエンコードされた値が含まれます。

* TS=1660667205：現在のタイムスタンプ。 このタイムスタンプにより、Web ビューは期限切れの値を受け取らなくなります。
* MCMID=69624092487065093697422606480535692677: [!UICONTROL Experience CloudID] (ECID) を使用します。 MID とも呼ばれます。 [!UICONTROL Marketing CloudID] 次に必要： [!DNL Adobe] クロスソリューション訪問者特定。
* MCORGID=EB9CAE8B56E003697F000101@AdobeOrg: [!UICONTROL Adobe組織 ID].

The `Identity.getUrlVariables` は別の方法です [!DNL Adobe Experience Platform Mobile SDK] メソッドを使用して、適切に形式が設定された文字列 ( [!DNL Experience Cloud Identity Service] URL 変数。 詳しくは、 [getUrlVariables](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/api-reference/#geturlvariables){target=_blank} （内） *ID API リファレンス*.

## パス [!DNL Target] 同じセッションエクスペリエンスのセッション ID

を作成するには、さらに 1 つの手順が必要です。 [!DNL Target] ユーザージャーニーは、ネイティブビューと web ビューをシームレスにまたいで機能します。 この手順には、 [!DNL Target] からのセッション ID [!DNL Adobe Experience Platform Mobile SDK] をモバイルアプリの web ビューに追加します。

The `Target.getSessionId` は、Web ビューの URL に渡すことのできるセッション ID を `mboxSession` パラメーター：

```swift
Target.getSessionId { (id, err) in
    // read Target sessionId
}
```

## Web ビューでのテスト

Web プレビューリンクが [!UICONTROL アクティビティの詳細] ページを開くには、 [[!UICONTROL AdobeQA] リンク](/help/dev/implement/mobile/target-mobile-preview.md) 次のようなポップアップを表示して、各エクスペリエンスのプレビューリンクをコピーします。

```
?at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```

Web プレビューリンクには、追加の `at_preview_index` および `at_preview_listed_activities_only` パラメーター。 これらのパラメーターをコピーして、Web リンクパラメーターを使用してモバイルに適したプレビューリンクを作成します。

次に例を示します。

```
com.adobe.targetmobile://?at_preview_token=mhFIzJSF7JWb-RsnakpBqhBwj-TiIlZsRTx_1QQuiXLIJFdpSLeEZwKGPUyy57O_&at_preview_index=1_1&at_preview_listed_activities_only=true
```

iOS Safari ブラウザーでリンクを開くと、アプリは `AppDelegate` 次の例のようなクラス：

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
  print("url= \(String(describing: url.absoluteString))")
  //...
```

これで、アプリケーションで必要なすべてのパラメーターを取り込み、必要に応じて Web に渡すことができます。

```swift
Identity.appendTo(url: URL(string: url), completion: {appendedURL, error in
  let urlWithWebPreviewLink = appendedURL + "&" + myPreviewLinkFromAppDelegate
```

Web ビューリンクの最終的な出力は、次のようになります。

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg&at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```
