---
keywords: 実装， javascript ライブラリ， js, atjs，オンデバイス判定，デバイス判定， at.js，オンデバイス，デバイス上，トラブルシューティング，トラブルシューティング，実装 2
description: トラブルシューティング方法を説明します [!UICONTROL オンデバイス判定] を at.js ライブラリと共に使用します。
title: at.js JavaScript ライブラリを使用したオンデバイス判定のトラブルシューティング方法を教えてください。
feature: at.js
exl-id: b9530cc7-5e83-4fdf-bde9-b2492e0861ff
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '274'
ht-degree: 0%

---

# トラブルシューティング [!UICONTROL オンデバイス判定] （at.js 用）

次の手順を実行してトラブルシューティングします。 [!UICONTROL オンデバイス判定] in [!UICONTROL Adobe Target] at.js JavaScript ライブラリを使用する場合：

## 手順 1:at.js のコンソールログの有効化

URL パラメーターの追加 `mboxDebug=1` at.js がブラウザーのコンソールにメッセージを印刷できるようにします。

すべてのメッセージには、わかりやすい概要を示すために、プレフィックス「AT:」が付きます。 アーティファクトが正常に読み込まれるようにするには、コンソールログに次のようなメッセージが記録されます。

```
AT: LD.ArtifactProvider fetching artifact - https://assets.adobetarget.com/your-client-cide/production/v1/rules.json
AT: LD.ArtifactProvider artifact received - status=200
```

次の図は、これらのメッセージをコンソールログに表示しています。

（全幅に拡大するには、画像をクリックします）。

![アーティファクトメッセージを含むコンソールログ](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/browser-console.png "アーティファクトメッセージを含むコンソールログ"){zoomable=&quot;yes&quot;}

## 手順 2：ブラウザーの「ネットワーク」タブでルールアーティファクトのダウンロードを確認する

ブラウザーの「ネットワーク」タブを開きます。

例えば、Google Chrome で DevTools を開くには、次のようにします。

1. Ctrl + Shift + J キー (Windows) または Command + Option + J キー (Mac) を押します。
1. 「ネットワーク」タブに移動します。
1. キーワード「rules.json」で呼び出しをフィルタリングして、アーティファクトルールファイルのみが表示されるようにします。

   また、「/delivery|rules.json/」でフィルタリングして、すべての Target 呼び出しとアーティファクト rules.json を表示できます。

   ![Google Chrome の「ネットワーク」タブ](assets/rule-json.png)

## 手順 3:at.js カスタムイベントを使用したルールアーティファクトのダウンロードの検証

at.js ライブラリは、をサポートするために 2 つの新しいカスタムイベントをディスパッチします。 [!UICONTROL オンデバイス判定].

* `adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED`
* `adobe.target.event.ARTIFACT_DOWNLOAD_FAILED`

アーティファクトルールファイルのダウンロードが成功または失敗した場合のアクションに対して、アプリケーションでこれらのカスタムイベントをリッスンするよう登録できます。

アーティファクトのダウンロードの成功イベントと失敗イベントをリッスンするコード例を次に示します。

```javascript {line-numbers="true"}
document.addEventListener(adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED, function(e) { 
  console.log("Artifact successfully downloaded", e.detail);
}, false);

document.addEventListener(adobe.target.event.ARTIFACT_DOWNLOAD_FAILED, function(e) { 
  console.log("Artifact failed to download", e.detail);
}, false);
```
