---
keywords: 実装，javascript ライブラリ，js, atjs, オンデバイス判定，オンデバイス判定，at.js, オンデバイス，オンデバイス，トラブルシューティング，トラブルシューティング，実装2
description: at.js ライブラリを使用した[!UICONTROL &#x200B; オンデバイス決定]のトラブルシューティング方法について説明します。
title: at.js JavaScript ライブラリを使用したオンデバイス判定のトラブルシューティング方法を教えてください。
feature: at.js
exl-id: b9530cc7-5e83-4fdf-bde9-b2492e0861ff
TQID: https://experienceleague.adobe.com/Ji3jAHC0Ek7FrVnabEEMm-KCtxJLJ5rSz4uyi6sWpiE
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
  - id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 4d0e7f9f2887db71229061fa64b2633a84c6d054
workflow-type: tm+mt
source-wordcount: 281
ht-degree: 0%

---

# at.jsの[!UICONTROL &#x200B; オンデバイス決定]のトラブルシューティング

at.js JavaScript ライブラリを使用して[!UICONTROL Adobe Target]の[!UICONTROL &#x200B; オンデバイス決定]をトラブルシューティングするには、次の手順を実行します。

## 手順1:at.jsのコンソールログを有効にする

URL パラメーター`mboxDebug=1`を追加すると、at.jsはブラウザーのコンソールにメッセージを印刷できるようになります。

すべてのメッセージには、便利な概要のために「AT:」というプレフィックスが含まれています。 アーティファクトが正常に読み込まれることを確認するには、コンソールログに次のようなメッセージを含める必要があります。

```
AT: LD.ArtifactProvider fetching artifact - https://assets.adobetarget.com/your-client-cide/production/v1/rules.json
AT: LD.ArtifactProvider artifact received - status=200
```

次の図は、コンソールログ内のこれらのメッセージを示しています。

（画像をクリックして全幅に拡大します）。

![&#x200B; アーティファクトメッセージを含むコンソールログ &#x200B;](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/browser-console.png " アーティファクトメッセージを含むコンソールログ "){zoomable="yes"}

## 手順2：ブラウザーの「ネットワーク」タブでルールアーティファクトのダウンロードを確認する

ブラウザーの「ネットワーク」タブを開きます。

例えば、Google Chromeで開発ツールを開くには、次の手順を実行します。

1. Ctrl + Shift + J キー（Windows）またはCommand + Option + J キー（Mac）を押します。
1. 「ネットワーク」タブに移動します。
1. キーワード「rules.json」で呼び出しをフィルタリングして、アーティファクトルールファイルのみが表示されるようにします。

   さらに、「/delivery|rules.json/」でフィルタリングして、すべてのTarget呼び出しとアーティファクトのrules.jsonを表示できます。

   ![Google Chromeの「ネットワーク」タブ &#x200B;](assets/rule-json.png)

## 手順3:at.js カスタムイベントを使用したルールアーティファクトのダウンロードの確認

at.js ライブラリは、2つの新しいカスタムイベントをディスパッチして、[!UICONTROL &#x200B; オンデバイス決定]をサポートします。

* `adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED`
* `adobe.target.event.ARTIFACT_DOWNLOAD_FAILED`

アーティファクトルールファイルのダウンロードが成功または失敗した場合に、アプリケーションでこれらのカスタムイベントをリッスンしてアクションを実行できます。

次の例は、アーティファクトダウンロードの成功イベントと失敗イベントをリッスンするコードの例を示しています。

```javascript {line-numbers="true"}
document.addEventListener(adobe.target.event.ARTIFACT_DOWNLOAD_SUCCEEDED, function(e) { 
  console.log("Artifact successfully downloaded", e.detail);
}, false);

document.addEventListener(adobe.target.event.ARTIFACT_DOWNLOAD_FAILED, function(e) { 
  console.log("Artifact failed to download", e.detail);
}, false);
```

