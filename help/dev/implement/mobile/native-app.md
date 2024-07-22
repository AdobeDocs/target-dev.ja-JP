---
keywords: モバイルアプリ、aep sdk、ネイティブアプリ、web ビュー、ネイティブ；swift、adobe experience platform mobile sdk、mobile sdk、ネイティブコード
description: Web ビューを使用してネイティブアプリに  [!DNL AEP Mobile SDK]  を実装する方法を説明します  [!DNL Adobe Target]
title: Web ビュー  [!DNL Adobe Target]  ネイティブコードを使用するモバイルアプリに実装
feature: Implement Mobile
role: Developer
exl-id: 3dd2e1d7-c744-4ba8-aaa4-6c2fe64d01fa
source-git-commit: 50ee7e66e30c0f8367763a63b6fde5977d30cfe7
workflow-type: tm+mt
source-wordcount: '561'
ht-degree: 0%

---

# Web ビューを使用したネイティブアプリでの [!DNL AEP Mobile SDK] を使用した [!DNL Target] の実装

この記事では、[!DNL Adobe Experience Platform Mobile SDK] を使用して、ネイティブコードと web ビューを使用するモバイルアプリで [!DNL Adobe Target] を実装するためのベストプラクティスを共有します。

この記事では、[[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/getting-started/){target=_blank} を使用したサンプル iOS アプリと、[GitHub リポジトリから Swift で記述された [!DNL Target] 統合を使用 ](https://github.com/adobe/aep-sdk-app/){target=_blank} ます。

実際の世界では、エンタープライズアプリはモバイルアプリで web ビューを使用している可能性が高くなります。 Web ビューは、URL を使用して web ページを読み込むコンテナです。 コンテナは、コントロールのないブラウザーウィンドウに似ています。 iOSでは、web ビューコンテナは、web ページの処理時に Safari ブラウザーとして機能します。

## 前提条件

[!DNL Adobe Experience Platform Mobile SDK] の使用を開始するには、いくつかの前提条件タスクを実行する必要があります。

詳しくは、[[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/){target=_blank} ドキュメントの [Adobe Target](https://developer.adobe.com/client-sdks/documentation/adobe-target/){target=_blank} を参照してください。

## ネイティブコードを web ビューと同期

Web 表示を使用するネイティブアプリで [!DNL Target] を実装する際の課題は、[!DNL Adobe Experience Platform Mobile SDK] がソリューションをシームレスに動作させるために必要なすべての識別子を既に生成してい [!DNL Adobe] ことです。 ただし、これらの識別子はネイティブのプラットフォーム環境にはないので、web ビューではまだ識別子が表示されません。 したがって、訪問者 ID が web 環境に保持されるように、一部の SDK 識別子を web ビューに渡すブリッジを作成する必要があります。 これを適切に行わないと、訪問が重複し、レポートに影響を与えます。

幸いなことに、[!DNL Adobe Experience Platform Mobile SDK] は、次のサンプルコードに示すように、web ビューが同じ訪問者に対して使用および保持されるために必要な [!DNL Adobe] パラメーターを生成する便利な方法を提供します。

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

`Identity.appendTo` メソッドの詳細と、メソッドの使用方法の例については、*Mobile SDK ドキュメント* の [Swift/例 ](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/tabs/api-reference/){target=_blank} を参照してください。

この URL は、`Identity.appendTo` を使用します。

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2
```

変換先：

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg
```

このように、URL にパラメーター `adobe_mc` 追加されています。 このパラメーターには、次のエンコードされた値が含まれています。

* TS=1660667205：現在のタイムスタンプ。 このタイムスタンプにより、web ビューに有効期限切れの値が届かなくなります。
* MCMID=69624092487065093697422606480535692677:[!UICONTROL Experience Cloud ID] （ECID）。 クロスソリューション訪問者の特定に必要な MID または [!UICONTROL Marketing Cloud ID] とも呼ば [!DNL Adobe] ます。
* MCORGID=EB9CAE8B56E003697F000101@AdobeOrg: [!UICONTROL Adobe Organization ID]。

`Identity.getUrlVariables` は、[!DNL Experience Cloud Identity Service] の URL 変数を含んだ適切な形式の文字列を返す代替 [!DNL Adobe Experience Platform Mobile SDK] メソッドです。 詳しくは、[Identity API リファレンス ](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/api-reference/#geturlvariables){target=_blank} の *getUrlVariables* を参照してください。

## 同じセッションエクスペリエンスの [!DNL Target] セッション ID を渡す

[!DNL Target] ユーザージャーニーをネイティブビューと web ビューをまたいでシームレスに機能させるには、さらに手順が必要です。 この手順には、[!DNL Target] セッション ID の抽出と、[!DNL Adobe Experience Platform Mobile SDK] からモバイルアプリの web ビューへの渡しが含まれます。

`Target.getSessionId` では、web ビュー URL に `mboxSession` パラメーターとして渡すことができるセッション ID を抽出します。

```swift
Target.getSessionId { (id, err) in
    // read Target sessionId
}
```

## Web ビューでのテスト

Web プレビューリンクは、[[!UICONTROL Adobe QA] リンクをクリックすると [!UICONTROL Activity detail] ページに生成され ](/help/dev/implement/mobile/target-mobile-preview.md) 次のように、各エクスペリエンスプレビューリンクをコピーするポップアップを表示します。

```
?at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```

Web プレビューリンクには、追加の `at_preview_index` パラメーターと `at_preview_listed_activities_only` パラメーターが含まれています。 これらのパラメーターをコピーして、web リンクパラメーターを含む、モバイルに適したプレビューリンクを作成します。

次に例を示します。

```
com.adobe.targetmobile://?at_preview_token=mhFIzJSF7JWb-RsnakpBqhBwj-TiIlZsRTx_1QQuiXLIJFdpSLeEZwKGPUyy57O_&at_preview_index=1_1&at_preview_listed_activities_only=true
```

iOS Safari ブラウザーでリンクを開くと、アプリは次の例のように、`AppDelegate` クラスの URL をキャプチャします。

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
  print("url= \(String(describing: url.absoluteString))")
  //...
```

必要なパラメーターをすべてアプリに取り込んだので、必要に応じて web に渡すことができます。

```swift
Identity.appendTo(url: URL(string: url), completion: {appendedURL, error in
  let urlWithWebPreviewLink = appendedURL + "&" + myPreviewLinkFromAppDelegate
```

Web 表示リンクの最終的な出力は、次のようになります。

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg&at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```
