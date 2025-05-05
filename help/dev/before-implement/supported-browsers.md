---
keywords: ブラウザー、前提条件、要件、internet explorer、chrome、firefox、safari、android、surface、Browsers0
description: インターフェイスとコンテンツ配信  [!DNL Adobe Target]  どのインターネットブラウザーがサポートされているかについて説明します。
title: はどのブラウザ  [!DNL Target]  をサポートしていますか？
feature: Implementation
exl-id: 1d778e14-26b0-477b-ac28-d304db70a133
source-git-commit: 1b6dcb24d677b758ed1daf85dc0a7e9e5b42680d
workflow-type: tm+mt
source-wordcount: '421'
ht-degree: 21%

---

# サポートされているブラウザー

[!DNL Adobe Target] アプリケーションとコンテンツ配信は様々なブラウザーとデバイスでテストされています。

TLS について詳しくは、「[TLS （Transport Layer Security）暗号化の変更 ](tls-transport-layer-security-encryption.md)」を参照してください。

## [!DNL Target] Standard/Premium インターフェイス

[!DNL Target] インターフェイスは、次のブラウザーとデバイスをサポートしています。

>[!NOTE]
>
>Target では、リストされている各ブラウザーの最新バージョンと、最新バージョンから 1 を引いたバージョンがサポートされています。


| デバイスタイプ | ブラウザーのバージョン |
|--- |--- |
| [!DNL Windows] | <ul><li>[!DNL Microsoft Edge]</li><li>[!DNL Google Chrome]</li><li>[!DNL Mozilla Firefox]</li></ul> |
| [!DNL Mac] | <ul><li>[!DNL Microsoft Edge]</li><li>[!DNL Google Chrome]</li><li>[!DNL Mozilla Firefox]</li></ul> |

## ビジュアル編集の要件

[!UICONTROL Visual Experience Composer] （VEC）で web ページを確実に開いて作成、プレビューするには、[Adobe Experience Cloud Visual Editing Helper ブラウザー拡張機能が web ブラウザーにインストールされているか ](https://experienceleague.adobe.com/ja/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank} 使用している必要 [!UICONTROL Enhanced Experience Composer (EEC)] あります。

>[!NOTE]
>
>現在、[!DNL Adobe Target] での web ページのビジュアル編集をサポートしているブラウザーは、[!DNL Google Chrome] と [!DNL Microsoft Edge] のみです。


## コンテンツ配信

コンテンツ配信は、次のブラウザーおよびデバイスでテストされています。

| デバイスタイプ | ブラウザーのバージョン |
|--- |--- |
| Windows | <ul><li>Microsoft Internet Explorer 9 および 10。 エミュレーションモードでテスト済み。**注意**:IE 9 でのコンテンツ配信は、at.js 1.3.0 （およびそれ以降）ではサポートされなくなりました。 IE 10、11、およびすべての旧バージョンでのコンテンツ配信は、at.js 2.5.0 （およびそれ以降）ではサポートされなくなりました。</li><li>Internet Explorer 11。 **注意**:IE 10、11 およびすべての旧バージョンでのコンテンツ配信は、at.js 2.5.0 （およびそれ以降）ではサポートされなくなりました。</li><li>Microsoft Edge</li><li>Chrome（最新、最新–1）</li><li>Firefox （最新、最新–1）</li></ul> |
| Mac | <ul><li>Apple Safari （最新） **注意**:Safari でのファーストパーティおよびサードパーティ Cookie の処理方法について詳しくは、[Target Cookie](../implement/client-side/atjs/atjs-cookies.md) を参照してください。</li><li>Firefox （最新、最新–1）</li><li>Chrome（最新、最新–1）</li></ul> |
| モバイル／タブレット | <ul><li>Apple iOS（最新）</li><li>Android デバイスおよびタブレット（Android 4 以降）</li><li>Microsoft Surface（Windows 8.1）</li></ul> |

以下のことに注意してください。

* [!DNL Adobe Experience Platform Web SDK] は、[!DNL Google Chrome]、[!DNL Safari]、[!DNL Firefox] および [!DNL Microsoft Edge Chromium] の最新バージョンで最適に動作するように設計されています。 これらのブラウザーの古いバージョンや、[!DNL Internet Explorer] などの非推奨ブラウザーでは、特定の機能を使用する際に問題が発生する可能性があります。
* at.js 実装の場合、[!DNL Target] では、以前のバージョンの Internet Explorer と、場合によっては上記のブラウザーの以前のバージョンで、デフォルトコンテンツが表示されます。
* Internet Explorer では、すべての不明な要素（カスタム要素など）は同じ要素タイプとして処理されます。 その結果、配信はカスタム要素では機能しません。
* [!DNL Target] は、上記に示されていないブラウザーおよび [ 互換モード ](https://en.wikipedia.org/wiki/Quirks_mode) を使用しているブラウザーで、デフォルトコンテンツを表示します。 at.js には、`<!DOCTYPE html>` など、標準モードでレンダリングを行う doctype が必要です。
* 配信インフラストラクチャ [!DNL Adobe]、2018 年 9 月 12 日（PT）以降、TLS 1.0 デバイスおよびブラウザーをサポートしないように保護されています。 「[TLS（トランスポート層セキュリティ）暗号化の変更](../before-implement/tls-transport-layer-security-encryption.md)」を参照して、この変更による全体への影響について理解してください。
