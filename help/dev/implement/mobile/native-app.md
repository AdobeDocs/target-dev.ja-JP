---
keywords: モバイルアプリ，aep sdk，ネイティブアプリ，web ビュー，ネイティブ；swift,adobe experience platform モバイル sdk，モバイル sdk，ネイティブコード
description: Web ビューを備えたネイティブアプリで [!DNL AEP Mobile SDK] を使用して [!DNL Adobe Target] を実装する方法について説明します。
title: Web ビューでネイティブ コードを使用するモバイル アプリで [!DNL Adobe Target] を実装する
feature: Implement Mobile
role: Developer
exl-id: 3dd2e1d7-c744-4ba8-aaa4-6c2fe64d01fa
TQID: https://experienceleague.adobe.com/JrbjPpq3ds0sl4rkMnuzF9SYk2PI4r676hHqN-Pvn78
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: d051910f-2bda-47ea-a969-6ade9fcd71f1
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 624
ht-degree: 0%

---

# [!DNL Target]を[!DNL AEP Mobile SDK]で実装し、web ビューを備えたネイティブアプリで実装する

この記事では、[!DNL Adobe Experience Platform Mobile SDK]を使用してweb ビューでネイティブコードを使用するモバイルアプリに[!DNL Adobe Target]を実装するためのベストプラクティスを紹介します。

この記事では、[[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/getting-started/){target=_blank}と[Swiftで記述された[!DNL Target]統合をGitHub リポジトリ ](https://github.com/adobe/aep-sdk-app/){target=_blank}から使用したiOS アプリのサンプルを使用します。

実際には、エンタープライズアプリはモバイルアプリでweb ビューを使用している可能性があります。 Web ビューは、URLを使用してweb ページを読み込むコンテナです。 コンテナは、コントロールを持たないブラウザーウィンドウに似ています。 IOSでは、web ページを処理する際に、Web ビューコンテナがSafari ブラウザーとして機能します。

## 前提条件

[!DNL Adobe Experience Platform Mobile SDK]を開始するには、いくつかの前提条件タスクを実行する必要があります。

詳しくは、[[!DNL Adobe Experience Platform Mobile SDK]](https://developer.adobe.com/client-sdks/documentation/){target=_blank} ドキュメントの[Adobe Target](https://developer.adobe.com/client-sdks/documentation/adobe-target/){target=_blank}を参照してください。

## ネイティブコードとweb ビューの同期

Web ビューを使用してネイティブアプリに[!DNL Target]を実装する際の課題は、[!DNL Adobe Experience Platform Mobile SDK]が[!DNL Adobe]個のソリューションがシームレスに動作するために必要なすべての識別子を既に生成していることです。 ただし、これらの識別子はネイティブプラットフォーム環境にはないため、web ビューにはまだ表示されません。 そのため、訪問者IDがweb環境に保持されるように、一部のSDK IDをweb ビューに渡すためにブリッジを作成する必要があります。 これを適切に行わないと、訪問が重複し、レポートに影響を与えます。

幸いなことに、[!DNL Adobe Experience Platform Mobile SDK]は、次のサンプルコードに示すように、web ビューが同じ訪問者を使用して永続化するために必要な[!DNL Adobe] パラメーターを生成する便利な方法を提供します。

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

`Identity.appendTo` メソッドの詳細と、メソッドの使用方法の例については、*モバイル SDK ドキュメント*&#x200B;の[Swift > Example](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/tabs/api-reference/){target=_blank}を参照してください。

`Identity.appendTo`を使用して、このURL:

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2
```

変形：

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg
```

ご覧のとおり、URLに`adobe_mc`個のパラメーターが追加されています。 このパラメーターには、次のエンコードされた値が含まれます。

* TS=1660667205：現在のタイムスタンプ。 このタイムスタンプは、Web ビューが期限切れの値を受け取らないようにします。
* MCMID=69624092487065093697422606480535692677: [!UICONTROL Experience Cloud ID] （ECID）。 [!DNL Adobe]個のクロスソリューション訪問者を識別するには、MIDまたは[!UICONTROL Marketing Cloud ID]とも呼ばれます。
* MCORGID=EB9CAE8B56E003697F000101@AdobeOrg: [!UICONTROL Adobe Organization ID]。

`Identity.getUrlVariables`は、[!DNL Experience Cloud Identity Service] URL変数を含む適切に形成された文字列を返す代替[!DNL Adobe Experience Platform Mobile SDK] メソッドです。 詳しくは、*ID API リファレンス*&#x200B;の[getUrlVariables](https://developer.adobe.com/client-sdks/documentation/mobile-core/identity/api-reference/#geturlvariables){target=_blank}を参照してください。

## 同一セッションのエクスペリエンスに[!DNL Target] セッション IDを渡します

ネイティブビューとweb ビューをまたいで[!DNL Target] ユーザージャーニーをシームレスに機能させるには、さらに1つの手順が必要です。 この手順では、[!DNL Adobe Experience Platform Mobile SDK]から[!DNL Target] セッション IDを抽出し、モバイルアプリのweb ビューに渡します。

`Target.getSessionId`は、Web ビューのURLに`mboxSession` パラメーターとして渡すことができるセッション IDを抽出します。

```swift
Target.getSessionId { (id, err) in
    // read Target sessionId
}
```

## web ビューでのテスト

Web プレビューリンクは、[[!UICONTROL Adobe QA] リンク ](/help/dev/implement/mobile/target-mobile-preview.md)をクリックしてポップアップを表示し、次のような各エクスペリエンス プレビューリンクをコピーすることで、[!UICONTROL  アクティビティの詳細] ページで生成されます。

```
?at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```

Web プレビューリンクには、追加の`at_preview_index`および`at_preview_listed_activities_only`個のパラメーターが含まれています。 これらのパラメーターをコピーして、モバイルフレンドリーなプレビューリンクとweb リンクパラメーターを作成します。

次に例を示します。

```
com.adobe.targetmobile://?at_preview_token=mhFIzJSF7JWb-RsnakpBqhBwj-TiIlZsRTx_1QQuiXLIJFdpSLeEZwKGPUyy57O_&at_preview_index=1_1&at_preview_listed_activities_only=true
```

IOS Safari ブラウザーでリンクを開くと、アプリは次の例のように`AppDelegate` クラスのURLをキャプチャします。

```swift
func application(_ app: UIApplication, open url: URL, options: [UIApplicationOpenURLOptionsKey : Any] = [:]) -> Bool {
  print("url= \(String(describing: url.absoluteString))")
  //...
```

これで、アプリ内で必要なパラメーターをすべてキャプチャできたので、必要に応じてwebに渡すことができます。

```swift
Identity.appendTo(url: URL(string: url), completion: {appendedURL, error in
  let urlWithWebPreviewLink = appendedURL + "&" + myPreviewLinkFromAppDelegate
```

Web ビューリンクの最終的な出力は次のようになります。

```
https://vadymus.github.io/ateng/at-order-confirmation/index.html?a=1&b=2&adobe_mc=TS%3D1660667205%7CMCMID%3D69624092487065093697422606480535692677%7CMCORGID%3DEB9CAE8B56E003697F000101%40AdobeOrg&at_preview_token=mhFIzJSF7JWb-RsnakpBqi_s83Sl64hZp928VWpkwvI&at_preview_index=1_1&at_preview_listed_activities_only=true
```
