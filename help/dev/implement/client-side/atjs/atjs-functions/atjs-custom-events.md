---
keywords: カスタムイベント，at.js, リクエストに失敗しました，リクエストに成功しました，コンテンツレンダリングに失敗しました，コンテンツレンダリングに成功しました，ライブラリが読み込まれました，リクエスト開始，コンテンツレンダリング開始，オファーなし，コンテンツレンダリングリダイレクト，カスタムイベント 2
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリのカスタムイベントを使用すると、mbox リクエストまたはオファーが失敗または成功したときに通知されます。'
title: at.js カスタムイベントを使用するにはどうすればよいですか？
feature: at.js
exl-id: a4baed9a-9eb8-4343-9834-709b03e44ca2
TQID: https://experienceleague.adobe.com/gCjLJ-XBlMe-GE-gaTJ2wHkn1jmal6Ftbs1GDb0zURA
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c2be0313-b3ae-45e0-b454-d20bf54b23f2
source-git-commit: 4d0e7f9f2887db71229061fa64b2633a84c6d054
workflow-type: tm+mt
source-wordcount: 661
ht-degree: 70%

---

# at.js カスタムイベント

mbox リクエストまたはオファーの失敗や成功を把握するための `at.js custom events` について説明します。

従来、mbox.js （現在は非推奨）では、ページ上で動作する他のJavaScriptコードに、バックグラウンドで何が起こるかを知らせることはできませんでした。 at.js の改良により、この問題を独自の方法で解決することができました。

当社のお客様によれば、次のような場合に通知が必要になります。

* タイムアウト、ステータスコードの誤り、JSON 解析エラーなどの理由で mbox リクエストが失敗した。
* mbox リクエストが成功した。
* ラッピング mbox 要素が欠落している、セレクターがないなどの理由でオファーのレンダリングが失敗した。
* オファーのレンダリングが成功した。 DOM の変更が適用されました。

事前定義されたイベントの構造は、イベントのタイプに基づいて必要なデータを抽出できるようになっています。

イベントを様々なシナリオで使用できるようにするため、カスタムイベントにはペイロードオブジェクトがあります。このオブジェクトは（ハンドラーに渡される）イベントオブジェクトの詳細プロパティに割り当てられます。 また、文字列をイベント名として渡さないようにするため、イベントは `adobe.target.event` 名前空間を使用して定数として公開されます。

## 構造

| キー | タイプ | 説明 |
|--- |--- |--- |
| type | 文字列 | at.js とのインタラクションの追跡、デバッグ、カスタマイズに役立つ通知を受け取るシナリオには、様々なものが考えられます。<p>以下の各カスタムイベントには、「定数」と「文字列値」の 2 つの形式があります。<ul><li>**定数**： 先頭に `adobe.target.event.` が追加されます。すべて大文字で記述され、アンダースコア文字が含まれます。 at.js の読み込みの&#x200B;*後*&#x200B;から、mbox の応答の&#x200B;*前*&#x200B;までの間にカスタムイベントをサブスクライブする場合は、定数を使用します。</li><li>**文字列値**： 小文字で記述され、ダッシュが含まれます。 at.js の読み込みの&#x200B;*前*&#x200B;にカスタムイベントをサブスクライブする場合は、文字列値を使用します。</li></ul>**Request Failed**<p>定数：`adobe.target.event.REQUEST_FAILED`<p>文字列値：`at-request-failed`<p>説明：タイムアウト、ステータスコードの誤り、JSON 解析エラーなどの理由で mbox リクエストが失敗しました。<p>**Request Succeeded**<p>定数：`adobe.target.event.REQUEST_SUCCEEDED`<p>文字列値：`at-request-succeeded`<p>説明：mbox リクエストが成功しました。<p>**Content Rendering Failed**<p>定数：`adobe.target.event.CONTENT_RENDERING_FAILED`<p>文字列値：`at-content-rendering-failed`<p>説明：ラッピング mbox 要素が欠落している、セレクターがないなどの理由でオファーのレンダリングが失敗しました。<p>**Content Rendering Succeeded**<p>定数：`adobe.target.event.CONTENT_RENDERING_SUCCEEDED`<p>文字列値：`at-content-rendering-succeeded`<p>説明：オファーのレンダリングが成功した。 DOM の変更が適用されました。<p>**Library Loaded**<p>定数：`adobe.target.event.LIBRARY_LOADED`<p>文字列値：`at-library-loaded`<p>説明：このイベントは、at.js が完全に読み込まれたタイミングを追跡する場合に最適です。 このイベントを使用してグローバル mbox の実行をカスタマイズできます。 グローバル mbox を無効にしてから、このイベントをリッスンして後でグローバル mbox を呼び出すこともできます。<p>**Request Start**<p>定数：`adobe.target.event.REQUEST_START`<p>文字列値：`at-request-start`<p>説明：このイベントは、HTTP リクエストの実行前に発生します。 このイベントは、Resource Timing API によるパフォーマンス計測に使用できます。<p>**Content Rendering Start**<p>定数：`adobe.target.event.CONTENT_RENDERING_START`<p>文字列値：`at-content-rendering-start`<p>説明：このイベントは、セレクターポーリングが開始され、コンテンツがページにレンダリングされる前に発生します。 このイベントを使用して、コンテンツのレンダリングの進捗状況を追跡できます。<p>**Content Rendering No Offers**<p>定数：`adobe.target.event.CONTENT_RENDERING_NO_OFFERS`<p>文字列値：`at-content-rendering-no-offers`<p>説明：このイベントは、オファーが返されなかったときに発生します。<p>**Content Rendering Redirect**<p>定数：`adobe.target.event.CONTENT_RENDERING_REDIRECT`<p>文字列値：`at-content-rendering-redirect`<p>説明：このイベントは、オファーがリダイレクトされ、[!DNL Target]が別のURLにリダイレクトされる場合に発生します。 |
| mbox | 文字列 | mbox 名 |
| message | 文字列 | 人間が解読可能な説明（何が起こったか、エラーメッセージの内容など）。 |
| トラッキング | オブジェクト | `sessionId` と `deviceId` が格納されます。 場合によっては、`deviceId` が [!DNL Target] を取得できないため、この値が欠落することがあります。 |
| type | 文字列 | **オンデバイス決定アーティファクトが成功しました**<p>定数：<p>`adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED`<p>文字列値：`artifactDownloadSucceeded`<p>説明：オンデバイス判定アーティファクトが正常にダウンロードされたときに呼び出されます。<p>**オンデバイス決定アーティファクトが失敗しました**<p>定数：`adobe.target.event.ARTIFACT_DOWNLOAD_FAILED`<p>文字列値：`artifactDownloadFailed`<p>説明：オンデバイス判定アーティファクトのダウンロードに失敗した場合に呼び出されます。 |

## 使用方法

```javascript {line-numbers="true"}
document.addEventListener(adobe.target.event.REQUEST_SUCCEEDED, function(event) { 
  console.log('Event', event); 
});
```

## トレーニングビデオ：応答トークンとat.js カスタムイベント ![&#x200B; チュートリアルバッジ &#x200B;](../../../assets/tutorial.png)

次のビデオでは、応答トークンとat.js カスタムイベントを使用して、[!DNL Target]からサードパーティシステムにプロファイル情報を共有する方法を説明します。

>[!VIDEO](https://video.tv.adobe.com/v/23253/?quality=12)

