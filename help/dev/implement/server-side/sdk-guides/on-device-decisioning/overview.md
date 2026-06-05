---
keywords: サーバーサイド，サーバーサイド，sdk, sdk, オンデバイス，意思決定，オンデバイス，オンデバイス，オンデバイス，ゼロレイテンシ，レイテンシ，ニアゼロ，node.js, サーバーサイド 3
description: '[!UICONTROL [!UICONTROL オンデバイス決定]]を使用してサーバー上で [!DNL Target] A/BおよびMVT アクティビティをキャッシュし、ほぼゼロの遅延でメモリ内の決定を実行する方法について説明します。'
title: オンデバイス判定とは何ですか？
feature: Implement Server-side
exl-id: 22ed3072-56f0-4075-9d1a-d642afe3b649
TQID: https://experienceleague.adobe.com/-HHGn3lG5fOh2GLXQ6jOLRQmX7H24lN-2fseOg4y5H4
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: bcc5edb5-84c3-4940-9f84-ed88b6c16274
  - id: bce87dde-a4ab-44c9-8a18-ad66e4ddb377
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1349
ht-degree: 8%

---

# オンデバイス判定の概要

次世代の[!DNL Adobe Target] SDKでは、[!UICONTROL &#x200B; オンデバイス判定]が提供されるようになりました。これにより、[!DNL Adobe Target]のEdge Networkへのネットワークリクエストをブロックすることなく、A/Bおよびエクスペリエンスのターゲット設定（XT）キャンペーンをサーバーにキャッシュし、ほぼゼロの遅延でメモリ内の判定を実行できます。

また、[!DNL Adobe Target]では、ライブサーバーコールを介して、実験やML主導のパーソナライゼーション施策から、最も関連性の高い最新のエクスペリエンスを柔軟に提供できます。 つまり、パフォーマンスが最も重要な場合は、[!UICONTROL &#x200B; オンデバイス判定]を利用できますが、最も関連性の高い最新のエクスペリエンスが必要な場合は、代わりにサーバーコールを実行できます。 オンデバイスとエッジ決定を使用するタイミング [を参照して、どちらか一方を使用する必要があるユースケースについて確認してください](../../sdk-guides/on-device-decisioning/supported-features.md)。

>[!NOTE]
>
>オンデバイス判定は、クライアントサイドとサーバーサイドの両方の実装で使用できます。 この記事では、サーバーサイドの[!UICONTROL &#x200B; オンデバイス決定]について説明します。 クライアントサイドの[!UICONTROL &#x200B; オンデバイス決定]について詳しくは、クライアントサイド実装ドキュメント [こちら](../../../client-side/atjs/on-device-decisioning/on-device-decisioning.md)を参照してください。

## どのように機能しますか？

[!UICONTROL on-device decisioning]が有効になっている[!DNL Adobe Target] SDKをインストールして初期化すると、*ルールアーティファクト*&#x200B;がダウンロードされ、サーバーに最も近いAkamai CDNからサーバー上にローカルにキャッシュされます。 サーバーサイドアプリケーション内で[!DNL Adobe Target] エクスペリエンスを取得するリクエストが行われると、キャッシュされたルールアーティファクトにエンコードされたメタデータに基づいて、返すコンテンツに関する決定がメモリ内で行われます。このメタデータは、[!UICONTROL &#x200B; オンデバイス決定]のすべてのA/BおよびXT アクティビティを定義します。

次の図は、[!UICONTROL &#x200B; オンデバイス決定] アーキテクチャを示しています。 クリックして画像を展開します。

（画像をクリックして全幅に拡大します）。

![&#x200B; オンデバイス判定アーキテクチャ図](/help/dev/implement/server-side/sdk-guides/on-device-decisioning/assets/asset-sdk-local-decisioning-architecture-diagram.png " オンデバイス判定アーキテクチャ図"){zoomable="yes"}

## どのようなメリットがあるでしょうか？

* **遅延がほぼゼロに近い決定を行います。** バケット化と決定は、ネットワーク要求のブロックを避けるために、メモリ内およびデバイス上で実行されます。
* **アプリケーションのパフォーマンスを強化します。** エンドユーザーのエクスペリエンスを損なうことなく、実験を行い、顧客とユーザーにパーソナライズされたエクスペリエンスを提供します。
* **Google サイトの品質スコアを向上させます。** オンラインストアとサーバーサイドで行われる意思決定により、オンラインビジネスのGoogle Site Quality スコアを向上させ、消費者が見つけやすくします。
* **リアルタイム分析から学習します。** [!DNL Adobe Target]またはA4T レポートを介してリアルタイムでアクティビティのパフォーマンスからインサイトを得て、重要な瞬間に戦略を方向転換できるようにします。

## サポートされる機能

### アクティビティ

オンデバイス決定では、[&#x200B; フォームベースのExperience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)によって作成された次のアクティビティタイプをサポートしています。

* [!UICONTROL A/B テスト]
* [!UICONTROL エクスペリエンスのターゲット設定]（XT）

### 配分方法

オンデバイス決定では、次の割り当て方法をサポートしています。

* 手動

### Audience Targeting

オンデバイス決定では、次のオーディエンスルールをサポートしています。

| オーディエンスルール | オンデバイス判定 |
| --- | --- |
| [地域](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/geo.html) | ○<P>オンデバイス決定を使用する場合、次の位置情報の属性がサポートされます。<ul><li>国 / 地域</li><li>市区町村</li><li>緯度</li><li>経度</li></ul> |
| [ネットワーク](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/network.html) | × |
| [モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html) | × |
| [&#x200B; カスタムパラメーター](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/custom-parameters.html) | ○ |
| [オペレーティングシステム](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/operating-system.html) | ○ |
| [サイトのページ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/site-pages.html) | ○ |
| [ブラウザー](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/browser.html) | ○ |
| [訪問者プロファイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) | × |
| [トラフィックソース](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/traffic-sources.html) | × |
| [時間枠](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/time-frame.html) | ○ |
| [Experience Cloud Audiences](https://experienceleague.adobe.com/docs/target/using/integrate/mmp.html) （Adobe Audience Manager、Adobe Analytics、Adobe Experience Managerのオーディエンス） | × |

## [!UICONTROL &#x200B; オンデバイス判定]を使用するようにクライアントをプロビジョニングするにはどうすればよいですか？

オンデバイス判定は、[!DNL Adobe Target] サーバーサイド SDKを使用するすべての[!DNL Adobe Target]のお客様が利用できます。 この機能を有効にするには、[!DNL Adobe Target] UIで&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 実装]** > **[!UICONTROL アカウントの詳細]**&#x200B;に移動し、**[!UICONTROL オンデバイス決定]** トグルを有効にします。

>[!NOTE]
>
>[!UICONTROL &#x200B; オンデバイス決定] トグルを有効または無効にするには、管理者または承認者&#x200B;*ユーザーの役割*&#x200B;が必要です。

![alt画像](assets/asset-odd-toggle.png)

オンデバイス決定トグルを有効にすると、[!DNL Adobe Target]は、クライアントの&#x200B;*ルールアーティファクト*&#x200B;の生成と伝播を開始します。

>[!NOTE]
>
>[!DNL Adobe Target] SDKを初期化して[!UICONTROL &#x200B; オンデバイス判定]を使用する前に、切り替えスイッチを有効にしてください。 [!UICONTROL &#x200B; オンデバイス決定]を機能させるには、まずルールアーティファクトを生成してAkamai CDNに反映する必要があります。

### 既存のすべての[!UICONTROL &#x200B; オンデバイス決定]の適格アクティビティをアーティファクトトグルに含めます

[!UICONTROL &#x200B; オンデバイス決定]に適格なすべてのライブ [!DNL Target] アクティビティをアーティファクトに自動的に含める場合は、この&#x200B;**オン**&#x200B;を切り替えます。

この切り替えスイッチ **off**&#x200B;を離すと、生成されたルール アーティファクトに含めるには、[!UICONTROL &#x200B; デバイス上の決定] アクティビティを再作成してアクティブ化する必要があります。

## アクティビティが[!UICONTROL &#x200B; オンデバイス判定]対応であることがわかるにはどうすればよいですか？

アクティビティを作成すると、**[!UICONTROL Decisioning Method]**&#x200B;というラベルがアクティビティの詳細ページに表示され、アクティビティが[!UICONTROL &#x200B; オンデバイス判定]対応かどうかを示します。

![alt画像](assets/asset-odd9.png)

**[!UICONTROL アクティビティ]** ページで[!UICONTROL &#x200B; オンデバイス決定]可能なすべてのアクティビティを表示するには、列&#x200B;**[!UICONTROL 決定方法]**&#x200B;をアクティビティのリストに追加します。

![alt画像](assets/asset-odd7.png)

>[!NOTE]
>
>[!UICONTROL &#x200B; オンデバイス判定]対応のアクティビティを作成してアクティブ化した後、生成されAkamai CDN PoPに反映されるルールアーティファクトに含まれるまで、最大20分かかる場合があります。

## [!UICONTROL &#x200B; オンデバイス決定] アクティビティが[!DNL Adobe Target]のサーバーサイド SDKを介して正常に配信されるようにするために実行する必要がある手順の概要を教えてください。

1. [!DNL Adobe Target] UIにアクセスし、**[!UICONTROL 管理]** > **[!UICONTROL 実装]** > **[!UICONTROL アカウントの詳細]**&#x200B;に移動して、**[!UICONTROL オンデバイス決定]**&#x200B;切り替えを有効にします。
1. **[!UICONTROL 既存のすべての[!UICONTROL &#x200B; オンデバイス決定]の適格なアクティビティをアーティファクト]** トグルに含める機能を有効にします。
1. [!UICONTROL &#x200B; オンデバイス決定]でサポートされているアクティビティタイプを作成してアクティブ化し、そのアクティビティの&#x200B;**[!UICONTROL 決定方法]**&#x200B;が&#x200B;**[!UICONTROL オンデバイス決定]**&#x200B;であることを確認します。
1. `decisioningMethod = on-device`で[Node.js](../../node-js/overview.md)または[Java](../../java/overview.md) SDKをインストールして初期化します。
1. コードに`getOffers()`または`getAttributes()`を実装して、デバイス上のエクスペリエンスを取得します。
1. コードをデプロイします。

上記の手順1 ～ 3を開始する方法を示す例については、「[はじめに](../getting-started/getting-started.md)」の節を参照してください。


## その他のリソース

### ウェビナー：[!DNL Adobe Target] のオンデバイス決定を使用して、待ち時間ゼロでパーソナライズとテストを行う

これまで以上に、マーケターや、製品所有者、開発者は、サイトやアプリなど、顧客とつながるあらゆる場所での顧客エクスペリエンス全体を最適化しようと取り組んでいます。 データが分断され、複雑な導入が必要な複数のツールでは不十分です。

この録画されたウェビナーでは、[!DNL Adobe Target]人の製品エキスパートが、デバイス上で重要なエクスペリエンス最適化の決定をほぼゼロの遅延でローカルに実行する方法について説明します。これにより、顧客のサイトパフォーマンスを向上させながら、新たなユースケースを生み出すことができます。

>[!VIDEO](https://video.tv.adobe.com/v/328148/?quality=12)


### チュートリアル：オンデバイス判定

[!DNL Adobe Target] [!UICONTROL &#x200B; オンデバイス決定]により、ほぼゼロの遅延でコンテンツ配信が可能になります。

この7分間のビデオ：

* [!UICONTROL &#x200B; オンデバイス決定]について説明します。これには、[!DNL Target]実装の他の方法との比較も含まれます
* Targetで[!UICONTROL &#x200B; オンデバイス決定]を有効にする方法を示します
* JSON コンテンツで設定されたフォームベースのコンポーザーアクティビティのサンプルを調べます
* [!UICONTROL &#x200B; オンデバイス決定]に必要なキー設定を含むサンプル Node.JS SDK コードを示します
* ブラウザーでの結果を示します

>[!VIDEO](https://video.tv.adobe.com/v/329032/?quality=12)

詳細なビデオとチュートリアルについては、[[!DNL Adobe Target]  チュートリアル &#x200B;](https://experienceleague.adobe.com/docs/target-learn/tutorials/overview.html?lang=ja)を参照してください。

### Adobe技術ブログ – パート 1: エッジプラットフォームでの実験とパーソナライゼーションのために[!DNL Adobe Target] NodeJS SDKを実行する（Akamai Edge Workers）

[ここをクリックしてブログ記事にアクセスしてください](https://medium.com/adobetech/part-1-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-4d8660964ed9)。

### Adobe Tech Blog - パート 2：Edge Platform で実験とパーソナライゼーションを行うために [!DNL Adobe Target] NodeJS SDK を実行する（AWS Lambda@Edge）

[ここをクリックしてブログ記事にアクセスしてください](https://medium.com/adobetech/part-2-run-adobe-target-nodejs-sdk-for-experimentation-and-personalization-on-edge-platforms-aws-4d6bdac24563)。
