---
keywords: サーバー側，サーバー側， sdk, sdk，オンデバイス，決定，デバイス上，デバイス上， ondevice，遅延なし，待ち時間，ニアゼロ， node.js，サーバー側 3
description: 使用方法を学ぶ [!UICONTROL [!UICONTROL on-device decisioning]] をキャッシュします。 [!DNL Target] サーバー上で A/B および MVT アクティビティを使用して、ほぼゼロの遅延でメモリ内判定を実行する。
title: On-Device Decisioning とは
feature: Implement Server-side
exl-id: 22ed3072-56f0-4075-9d1a-d642afe3b649
source-git-commit: ff0becf3fe3a6fd6694e13243b6a93b910316434
workflow-type: tm+mt
source-wordcount: '1022'
ht-degree: 9%

---

# オンデバイス判定の概要

次世代 [!DNL Adobe Target] SDK でオファーが開始されました [!UICONTROL on-device decisioning]：サーバー上で A/B およびエクスペリエンスターゲット設定 (XT) キャンペーンをキャッシュし、に対するネットワークリクエストをブロックすることなく、ほぼゼロの待ち時間でメモリ内判定を実行できます。 [!DNL Adobe Target]の Edge ネットワーク。

[!DNL Adobe Target] また、は、実験や ML 駆動のパーソナライゼーションキャンペーンから、ライブサーバー呼び出しを介して、最も関連性の高い最新のエクスペリエンスを提供する柔軟性も提供します。 つまり、パフォーマンスが最も重要な場合、 [!UICONTROL on-device decisioning]ですが、最も関連性の高い最新のエクスペリエンスが必要な場合は、代わりにサーバー呼び出しをおこなうことができます。 詳しくは、 [オンデバイスとエッジ判定を使用するタイミング](../../sdk-guides/on-device-decisioning/supported-features.md) を使用して適切な使用例を確認する必要があります。

>[!NOTE]
>
>オンデバイス判定は、クライアント側とサーバー側の両方の実装で使用できます。 この記事では、 [!UICONTROL on-device decisioning] （サーバー側）。 に関する情報 [!UICONTROL on-device decisioning] クライアント側の場合は、クライアント側実装のドキュメントを参照してください。 [ここ](../../../client-side/atjs/on-device-decisioning/on-device-decisioning.md).

## どのように機能しますか？

をインストールして初期化すると、 [!DNL Adobe Target] を使用した SDK [!UICONTROL on-device decisioning] 有効、 *ルールアーティファクト* は、サーバーに最も近い Akamai CDN からダウンロードされ、サーバー上にローカルにキャッシュされます。 次を取得するリクエストが発生したとき： [!DNL Adobe Target] エクスペリエンスは、サーバーサイドアプリケーション内でおこなわれ、キャッシュされたルールアーティファクトでエンコードされたメタデータ ( [!UICONTROL on-device decisioning] A/B および XT アクティビティ。

次の図に、 [!UICONTROL on-device decisioning] アーキテクチャ。 をクリックして、画像を展開します。

（全幅に拡大するには、画像をクリックします）。

![オンデバイス判定アーキテクチャの図](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/assets/asset-sdk-local-decisioning-architecture-diagram.png "オンデバイス判定アーキテクチャの図"){zoomable=&quot;yes&quot;}

## 利点は何ですか？

* **遅延がほぼゼロの決定を下します。** グループ化と判定は、ネットワークリクエストのブロックを避けるために、メモリ内およびデバイス上で実行されます。
* **アプリケーションのパフォーマンスを向上させます。** エンドユーザーエクスペリエンスを損なうことなく、実験を実行し、パーソナライゼーションを顧客およびユーザーに提供します。
* **Googleサイトの品質スコアを改善します。** メモリ内およびサーバー側で判定がおこなわれるので、オンラインビジネスのGoogleサイトの品質スコアが向上し、消費者がより見つけやすいものになります。
* **リアルタイム分析から学ぶ。** を通じて、アクティビティのパフォーマンスからリアルタイムでインサイトを得る [!DNL Adobe Target] A4T レポートを使用すると、重要な時点で戦略をピボットできます。

## サポートされる機能

### アクティビティ

オンデバイス判定では、 [フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html):

* [!UICONTROL A/B Test]
* [!UICONTROL Experience Targeting] (XT)

### 配分方法

オンデバイス判定では、次の割り当て方法がサポートされます。

* 手動

### オーディエンスのターゲティング

オンデバイス判定では、次のオーディエンスルールがサポートされます。

| オーディエンスルール | オンデバイス判定 |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | ○<P>オンデバイス判定を使用する場合、次の地域属性がサポートされます。<ul><li>国 / 地域</li><li>市区町村</li><li>緯度</li><li>経度</li></ul> |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | × |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | × |
| [カスタムパラメーター](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | × |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | × |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | ○ |
| [Experience Cloudオーディエンス](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) (Adobe Audience Manager、Adobe Analytics、Adobe Experience Managerのオーディエンス | × |

## クライアントを使用するようにプロビジョニングする方法 [!UICONTROL on-device decisioning]?

オンデバイス判定は、すべての [!DNL Adobe Target] を使用する顧客 [!DNL Adobe Target] サーバーサイド SDK この機能を有効にするには、次に移動します。 **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** （内） [!DNL Adobe Target] UI を開き、 **[!UICONTROL On-Device Decisioning]** 切り替え

>[!NOTE]
>
>管理者または承認者が必要です *ユーザーロール* 有効または無効にするには [!UICONTROL On-Device Decisioning] 切り替え

![代替画像](assets/asset-odd-toggle.png)

「オンデバイス判定」切り替えを有効にした後、 [!DNL Adobe Target] 生成を開始し、 *ルールアーティファクト* を設定します。

>[!NOTE]
>
>トグルを有効にしてから、 [!DNL Adobe Target] 使用する SDK [!UICONTROL on-device decisioning]. ルールアーティファクトは、まず、次の目的で Akamai CDN を生成して伝播する必要があります。 [!UICONTROL on-device decisioning] を有効にします。

### 既存のすべてを含める [!UICONTROL on-device decisioning] アーティファクトの切り替えでの適合したアクティビティ

切り替え **オン** 生きていきたい時 [!DNL Target] 条件を満たすアクティビティ [!UICONTROL on-device decisioning] を自動的にアーティファクトに含めます。

この切り替えを終了 **オフ** は、 [!UICONTROL on-device decisioning] アクティビティを作成し、生成されたルールアーティファクトに含める必要があります。

## アクティビティの確認方法 [!UICONTROL on-device decisioning] 可能？

アクティビティを作成した後、 **[!UICONTROL Decisioning Method]**( アクティビティの詳細ページに表示され、アクティビティが [!UICONTROL on-device decisioning] 可能

![代替画像](assets/asset-odd9.png)

また、 [!UICONTROL on-device decisioning] ～に対して有効である **[!UICONTROL Activities]** 列を追加してページを表示 **[!UICONTROL Decisioning Method]** をアクティビティのリストに追加します。

![代替画像](assets/asset-odd7.png)

>[!NOTE]
>
>次のアクティビティを作成してアクティブ化した後： [!UICONTROL on-device decisioning] 可能です。生成され Akamai CDN PoPs に反映されるルールアーティファクトに含められるまでに、最大 20 分かかる場合があります。

## 次の手順の概要を説明します。 [!UICONTROL on-device decisioning] 次を介してアクティビティが正常に配信されました： [!DNL Adobe Target]のサーバー側 SDK

1. 次にアクセス： [!DNL Adobe Target] UI を開き、に移動します。 **[!UICONTROL Administration]** > **[!UICONTROL Implementation]** > **[!UICONTROL Account details]** 有効にする **[!UICONTROL On-Device Decisioning]** 切り替え
1. を有効にします。 **[!UICONTROL Include all existing [!UICONTROL on-device decisioning] qualified activities in the artifact]** 切り替え
1. でサポートされているアクティビティタイプの作成とアクティブ化 [!UICONTROL on-device decisioning]、を確認し、 **[!UICONTROL Decisioning Method]** 次に該当 **[!UICONTROL On-Device Decisioning]** 」をクリックします。
1. をインストールして初期化します。 [Node.js](../../node-js/overview.md) または [Java](../../java/overview.md) を使用した SDK `decisioningMethod = on-device`.
1. 実装方法 `getOffers()` または `getAttributes()` を使用して、デバイス上のエクスペリエンスを取得します。
1. コードをデプロイします。

上記の手順 1～3 を開始する方法を示す例については、 [はじめに](../getting-started/getting-started.md) 」セクションに入力します。


## その他のリソース

### ウェビナー：[!DNL Adobe Target] のオンデバイス決定を使用して、待ち時間ゼロでパーソナライズとテストを行う

これまで以上に、マーケターや、製品所有者、開発者は、サイトやアプリなど、顧客とつながるあらゆる場所での顧客エクスペリエンス全体を最適化しようと取り組んでいます。データのサイロや複雑な実装を備えた複数のツールが不十分です。

この記録済みウェビナーでは、 [!DNL Adobe Target] 製品エキスパートは、ほぼゼロの遅延で、デバイス上での重要なエクスペリエンス最適化の決定をローカルで実行する方法を説明し、新しい使用例に扉を開き、お客様のサイトのパフォーマンスを向上させます。

>[!VIDEO](https://video.tv.adobe.com/v/328148/?quality=12)


### チュートリアル：オンデバイス判定

[!DNL Adobe Target] [!UICONTROL on-device decisioning] 遅延がほぼゼロのコンテンツ配信を有効にします。

この 7 分間のビデオ：

* 説明 [!UICONTROL on-device decisioning]他の方法と比較する方法も含む [!DNL Target] 実装
* を有効にする方法を示します。 [!UICONTROL on-device decisioning] Target 内
* JSON コンテンツを使用して設定されたサンプルのフォームベースのコンポーザーアクティビティを調べます。
* に必要なキー設定を含む Node.JS SDK コードのサンプルを示します。 [!UICONTROL on-device decisioning]
* ブラウザーでの結果を示します。

>[!VIDEO](https://video.tv.adobe.com/v/329032/?quality=12)

その他のビデオおよびチュートリアルについては、 [[!DNL Adobe Target] Tutorials](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html?lang=ja).

### Adobeテクニカルブログ — 第 1 部：実行 [!DNL Adobe Target] エッジプラットフォームでの実験とパーソナライゼーションのための NodeJS SDK(Akamai Edge Workers)

[ブログ投稿にアクセスするには、ここをクリックしてください](https://medium.com/adobetech/part-1-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-4d8660964ed9).

### Adobe Tech Blog - パート 2：Edge Platform で実験とパーソナライゼーションを行うために [!DNL Adobe Target] NodeJS SDK を実行する（AWS Lambda@Edge）

[ブログ投稿にアクセスするには、ここをクリックしてください](https://medium.com/adobetech/part-2-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-aws-4d6bdac24563).
