---
keywords: ブラウザー、前提条件、要件、internet explorer、chrome、firefox、safari、android、surface、Browsers0
description: インターフェイスとコンテンツ配信でサポートされているインターネット ブラウザー [!DNL Adobe Target] について説明します。
title: ' [!DNL Target] はどのブラウザーをサポートしていますか？'
feature: Implementation
exl-id: 1d778e14-26b0-477b-ac28-d304db70a133
TQID: https://experienceleague.adobe.com/xYilaZkJzo4zLIJ4uvIxkuRkhl5E1D52OFmf1eZNtDs
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: d095671a-1355-40aa-8b5f-06c33c68080b
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 443
ht-degree: 20%

---

# サポートされているブラウザー

[!DNL Adobe Target] アプリケーションとコンテンツ配信は様々なブラウザーとデバイスでテストされています。

TLSに関する重要な情報については、[TLS （Transport Layer Security）暗号化の変更](tls-transport-layer-security-encryption.md)を参照してください。

## [!DNL Target] Standard/Premium インターフェイス

[!DNL Target] インターフェイスは、次のブラウザーとデバイスをサポートしています。

>[!NOTE]
>
>Targetは、リストされている各ブラウザーの最新バージョンと、最新バージョンから1を引いたバージョンをサポートしています。


| デバイスタイプ | ブラウザーのバージョン |
|--- |--- |
| [!DNL Windows] | <ul><li>[!DNL Microsoft Edge]</li><li>[!DNL Google Chrome]</li><li>[!DNL Mozilla Firefox]</li></ul> |
| [!DNL Mac] | <ul><li>[!DNL Microsoft Edge]</li><li>[!DNL Google Chrome]</li><li>[!DNL Mozilla Firefox]</li></ul> |

## ビジュアル編集の要件

[!UICONTROL Visual Experience Composer] （VEC）でweb ページを確実に開き、作成し、プレビューするには、[Adobe Experience Cloud Visual Editing Helper ブラウザー拡張機能](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank}をweb ブラウザーにインストールするか、[!UICONTROL Enhanced Experience Composer （EEC） ]を使用する必要があります。

>[!NOTE]
>
>[!DNL Google Chrome]と[!DNL Microsoft Edge]は現在、[!DNL Adobe Target]のweb ページのビジュアル編集をサポートする唯一のブラウザーです。


## コンテンツ配信

コンテンツ配信は、次のブラウザーおよびデバイスでテストされています。

| デバイスタイプ | ブラウザーのバージョン |
|--- |--- |
| Windows | <ul><li>Microsoft Internet Explorer 9および10。 エミュレーションモードでテスト済み。 **注意**: at.js 1.3.0 （およびそれ以降）では、IE 9でのコンテンツ配信はサポートされなくなりました。 IE 10、11、およびすべての古いバージョンでのコンテンツ配信は、at.js 2.5.0 （およびそれ以降）ではサポートされなくなりました。</li><li>Internet Explorer 11。 **注意**: at.js 2.5.0以降では、IE 10、11およびすべての古いバージョンのコンテンツ配信はサポートされなくなりました。</li><li>Microsoft Edge</li><li>Chrome（最新、最新マイナス 1）</li><li>Firefox （最新、最新マイナス 1）</li></ul> |
| Mac | <ul><li>Apple Safari （最新）。 **メモ**: Safariがファーストパーティ Cookieとサードパーティ Cookieをどのように処理するかについて詳しくは、[Target Cookie](../implement/client-side/atjs/atjs-cookies.md)を参照してください。</li><li>Firefox （最新、最新マイナス 1）</li><li>Chrome（最新、最新マイナス 1）</li></ul> |
| モバイル／タブレット | <ul><li>Apple iOS（最新）</li><li>Android デバイスおよびタブレット（Android 4 以降）</li><li>Microsoft Surface（Windows 8.1）</li></ul> |

以下のことに注意してください。

* [!DNL Adobe Experience Platform Web SDK]は、[!DNL Google Chrome]、[!DNL Safari]、[!DNL Firefox]、[!DNL Microsoft Edge Chromium]の最新バージョンで最適に動作するように設計されています。 これらのブラウザーの古いバージョンまたは非推奨のブラウザー（[!DNL Internet Explorer]など）で特定の機能を使用すると、問題が発生する可能性があります。
* at.js実装の場合、[!DNL Target]はInternet Explorerの以前のバージョン、および上記のブラウザーの以前のバージョンでデフォルトコンテンツを表示します。
* Internet Explorerは、すべての不明な要素（カスタム要素など）を同じ要素タイプとして扱います。 その結果、配信はカスタム要素で機能しません。
* [!DNL Target]は、上記に記載されていないブラウザーと[ クイックモード ](https://en.wikipedia.org/wiki/Quirks_mode)を使用しているブラウザーにデフォルトコンテンツを表示します。 at.js には、`<!DOCTYPE html>` など、標準モードでレンダリングを行う doctype が必要です。
* 2018年9月12日以降、TLS 1.0 デバイスとブラウザーをサポートしないように[!DNL Adobe]配信インフラストラクチャを保護しています。 「[TLS（トランスポート層セキュリティ）暗号化の変更](../before-implement/tls-transport-layer-security-encryption.md)」を参照して、この変更による全体への影響について理解してください。
