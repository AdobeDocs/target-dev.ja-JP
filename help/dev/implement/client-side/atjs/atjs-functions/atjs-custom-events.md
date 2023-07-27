---
keywords: カスタムイベント， at.js，リクエストが失敗しました，リクエストが成功しました，コンテンツのレンダリングが失敗しました，コンテンツのレンダリングが成功しました，ライブラリの読み込み，リクエスト開始，コンテンツのレンダリング開始，コンテンツのレンダリングなし，コンテンツのレンダリングリダイレクト，カスタムイベント 2
description: カスタムイベントを [!DNL Adobe Target] at.js JavaScript ライブラリ：mbox リクエストまたはオファーが失敗または成功した場合に通知を受け取ります。
title: at.js カスタムイベントの使用方法
feature: at.js
exl-id: a4baed9a-9eb8-4343-9834-709b03e44ca2
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '655'
ht-degree: 74%

---

# at.js カスタムイベント

mbox リクエストまたはオファーの失敗や成功を把握するための `at.js custom events` について説明します。

以前は、mbox.js（現在は非推奨）を使用している場合、バックグラウンドでの処理内容を、ページ上で実行される他の JavaScript コードが把握することはできませんでした。 at.js の改良により、この問題を独自の方法で解決することができました。

当社のお客様によれば、次のような場合に通知が必要になります。

* タイムアウト、ステータスコードの誤り、JSON 解析エラーなどの理由で mbox リクエストが失敗した。
* mbox リクエストが成功した。
* ラッピング mbox 要素が欠落している、セレクターがないなどの理由でオファーのレンダリングが失敗した。
* オファーのレンダリングが成功した。DOM の変更が適用されました。

事前定義されたイベントの構造は、イベントのタイプに基づいて必要なデータを抽出できるようになっています。

イベントを様々なシナリオで使用できるようにするため、カスタムイベントにはペイロードオブジェクトがあります。このオブジェクトは（ハンドラーに渡される）イベントオブジェクトの詳細プロパティに割り当てられます。また、文字列をイベント名として渡さないようにするため、イベントは `adobe.target.event` 名前空間を使用して定数として公開されます。

## 構造

| キー | タイプ | 説明 |
|--- |--- |--- |
| type | 文字列 | at.js とのインタラクションの追跡、デバッグ、カスタマイズに役立つ通知を受け取るシナリオには、様々なものが考えられます。<p>以下の各カスタムイベントには、「定数」と「文字列値」の 2 つの形式があります。<ul><li>**定数**： 先頭に `adobe.target.event.` が追加されます。すべて大文字で記述され、アンダースコア文字が含まれます。at.js の読み込みの&#x200B;*後*&#x200B;から、mbox の応答の&#x200B;*前*&#x200B;までの間にカスタムイベントをサブスクライブする場合は、定数を使用します。</li><li>**文字列値**： 小文字で記述され、ダッシュが含まれます。at.js の読み込みの&#x200B;*前*&#x200B;にカスタムイベントをサブスクライブする場合は、文字列値を使用します。</li></ul>**Request Failed**<p>定数: `adobe.target.event.REQUEST_FAILED`<p>文字列値: `at-request-failed`<p>説明： タイムアウト、ステータスコードの誤り、JSON 解析エラーなどの理由で mbox リクエストが失敗しました。<p>**Request Succeeded**<p>定数: `adobe.target.event.REQUEST_SUCCEEDED`<p>文字列値： `at-request-succeeded`<p>説明： mbox リクエストが成功しました。<p>**Content Rendering Failed**<p>定数: `adobe.target.event.CONTENT_RENDERING_FAILED`<p>文字列値： `at-content-rendering-failed`<p>説明： ラッピング mbox 要素が欠落している、セレクターがないなどの理由で、オファーのレンダリングが失敗しました。<p>**Content Rendering Succeeded**<p>定数: `adobe.target.event.CONTENT_RENDERING_SUCCEEDED`<p>文字列値： `at-content-rendering-succeeded`<p>説明： オファーのレンダリングが成功しました。DOM の変更が適用されました。<p>**Library Loaded**<p>定数: `adobe.target.event.LIBRARY_LOADED`<p>文字列値： `at-library-loaded`<p>説明： このイベントは、at.js が完全に読み込まれたタイミングを追跡する場合に最適です。このイベントを使用してグローバル mbox の実行をカスタマイズできます。グローバル mbox を無効にしてから、このイベントをリッスンして後でグローバル mbox を呼び出すこともできます。<p>**Request Start**<p>定数: `adobe.target.event.REQUEST_START`<p>文字列値： `at-request-start`<p>説明： このイベントは、HTTP リクエストの実行前に発生します。このイベントは、Resource Timing API によるパフォーマンス計測に使用できます。<p>**Content Rendering Start**<p>定数: `adobe.target.event.CONTENT_RENDERING_START`<p>文字列値： `at-content-rendering-start`<p>説明： このイベントは、セレクターポーリングが開始され、コンテンツがページにレンダリングされる前に発生します。このイベントを使用して、コンテンツのレンダリングの進捗状況を追跡できます。<p>**Content Rendering No Offers**<p>定数: `adobe.target.event.CONTENT_RENDERING_NO_OFFERS`<p>文字列値： `at-content-rendering-no-offers`<p>説明： このイベントは、オファーが返されなかったときに発生します。<p>**Content Rendering Redirect**<p>定数: `adobe.target.event.CONTENT_RENDERING_REDIRECT`<p>文字列値： `at-content-rendering-redirect`<p>説明：このイベントは、オファーがリダイレクトであり、 [!DNL Target] は別の URL にリダイレクトされます。 |
| mbox | 文字列 | mbox 名 |
| message | 文字列 | 人間が解読可能な説明（何が起こったか、エラーメッセージの内容など）。 |
| トラッキング | オブジェクト | `sessionId` と `deviceId` が格納されます。場合によっては、`deviceId` が [!DNL Target] を取得できないため、この値が欠落することがあります。 |
| type | 文字列 | **オンデバイス判定アーティファクトが成功しました**<p>定数:<p>`adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED`<p>文字列値: `artifactDownloadSucceeded`<p>説明：オンデバイス判定アーティファクトが正常にダウンロードされたときに呼び出されます。<p>**オンデバイス判定アーティファクトが失敗しました**<p>定数: `adobe.target.event.ARTIFACT_DOWNLOAD_FAILED`<p>文字列値： `artifactDownloadFailed`<p>説明：オンデバイス判定アーティファクトのダウンロードに失敗した場合に呼び出されます。 |

## 使用方法

```javascript {line-numbers="true"}
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(event) { 
  console.log('Event', event); 
});
```

## トレーニングビデオ：レスポンストークンと at.js カスタムイベント ![チュートリアルバッジ](../../../assets/tutorial.png)

次のビデオでは、レスポンストークンと at.js カスタムイベントを使用してからプロファイル情報を共有する方法について説明します。 [!DNL Target] をサードパーティ製システムに追加する必要があります。

>[!VIDEO](https://video.tv.adobe.com/v/23253/?quality=12)
