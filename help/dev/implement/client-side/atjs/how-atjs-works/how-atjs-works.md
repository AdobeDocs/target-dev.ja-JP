---
keywords: システム図，ちらつき，at.js，実装，javascript ライブラリ，js, atjs, $8
description: ページの読み込み時のワークフローを理解するのに役立つ  [!DNL Target]  at.js JavaScript ライブラリ関数（システム図を含む）について説明します。
title: at.js Javascript ライブラリはどのように機能しますか？
feature: at.js
exl-id: 9183797c-857b-4b7f-a573-6bb1d583f7b1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1127'
ht-degree: 66%

---

# at.js の仕組み

クライアントサイドで [!DNL Adobe Target] を実装するには、at.js JavaScript ライブラリを使用する必要があります。

クライアント側での [!DNL Adobe Target] の実装では、[!DNL Target] アクティビティに関連付けられたエクスペリエンスをクライアントブラウザーに直接配信します。ブラウザーは、表示するエクスペリエンスを決定して表示します。クライアント側の実装では、WYSIWYG エディターの [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) （VEC）または非ビジュアルインターフェイスである[フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html) を使用して、テストエクスペリエンスとパーソナライゼーションエクスペリエンスを作成できます。

## at.js について

at.js ライブラリは、[!DNL Adobe Target] のクライアントサイド実装用の実装ライブラリです。 at.js ライブラリは、Web 実装のページ読み込み時間を改善し、シングルページアプリケーション向けのより優れた実装オプションを提供します。at.js は推奨される実装ライブラリであり、頻繁にアップデートされて新しい機能が追加されます。すべてのお客様に、[at.js の最新バージョン ](/help/dev/implement/client-side/atjs/target-atjs-versions.md) を実装または移行することをお勧めします。

詳細については、[Target JavaScript ライブラリ](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#libraries)を参照してください。

以下に示す [!DNL Target] 実装では、[!DNL Analytics]、Target および [!DNL Audience Manager] のAdobe Experience Cloud ソリューションが実装されています。 さらに、[!DNL Adobe Experience Platform]、[!UICONTROL Audiences]、[!UICONTROL Visitor ID Service] の [!DNL Experience Cloud] コアサービスが実装されています。

## at.js 1.*x* と at.js 2.x のワークフロー図の違いは何ですか？

2.0 と 1.x のバージョン間の違いについて詳しくは、「[at.js 1.x から at.js 2.x へのアップグレード](/help/dev/implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md)」を参照してください&#x200B;**。

抽象度の高い表示では、2 つのバージョン間にいくつかの違いがあります。

* at.js 2.x は、グローバル mbox リクエストの概念がなく、ページ読み込みリクエストを使用します。ページ読み込みリクエストは、Web サイトの最初のページ読み込み時に適用されるコンテンツを取得するリクエストとして表示できます。
* at.js 2.x は、Single Page Applications （SPA）で使用される [!UICONTROL Views] と呼ばれる概念を管理します。 at.js 1.*x* はこの概念に対応していません。

## at.js 2.x 図

次の図は、at.js 2.x と [!UICONTROL Views] の連携のワークフローと、これによりSPA統合がどのように強化されるかを理解するのに役立ちます。 at.js 2.x で使用されている概念に関するより詳しい概要については、「[シングルページアプリケーションの実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)」を参照してください。

（全幅に拡大するには、画像をクリックします）。

![at.js 2.x での Target のフロー ](/help/dev/implement/client-side/assets/system-diagram-atjs-20.png "at.js 2.x での Target のフロー "){zoomable="yes"}

| 手順 | 詳細 |
| --- | --- |
| 1 | 呼び出しユーザーが認証されると、呼び出しが [!UICONTROL Experience Cloud ID]を返し、別の呼び出しが顧客 ID を同期します。 |
| 2 | at.js ライブラリがドキュメント本文を同期的に読み込み、非表示にします。<br />at.js は、ページに実装されているオプションの非表示スニペットを使用して非同期で読み込むこともできます。 |
| 3 | すべての設定済みパラメーター（MCID、SDID および顧客 ID）を含む、ページ読み込みリクエストがおこなわれます。 |
| 4 | プロファイルスクリプトは、を実行してから [!UICONTROL Profile Store] にフィードします。 Store は、[!UICONTROL Audience Library] から適格なオーディエンスをリクエストします（例えば、[!DNL Adobe Analytics] から共有されたオーディエンス、[!DNL Audience Manager] など）。<br />顧客属性がバッチ処理で [!UICONTROL Profile Store] に送信されます。 |
| 5 | URL リクエストパラメーターとプロファイルデータに基づいて、[!DNL Target] が現在のページおよび将来のビューでどのアクティビティおよびエクスペリエンスを訪問者に返すかを決定します。 |
| 6 | ターゲットコンテンツが（オプションで、追加のパーソナライゼーションに関するプロファイル値を含めて）ページに送り返されます。<br />デフォルトコンテンツがちらつくことなく、可能な限り迅速に現在のページ上のターゲットコンテンツが表示されます。<br />SPA でのユーザーアクションの結果として表示されるビューのターゲットコンテンツは、ブラウザーにキャッシュされます。そのため、`triggerView()` を介してビューがトリガーされたときに追加のサーバー呼び出しをおこなわずに即座にターゲットコンテンツを適用できます。 |
| 7 | Analytics データは [!UICONTROL Data Collection] サーバーに送信されます。 |
| 8 | ターゲットデータは、SDID を介して Analytics データと照合され、[!DNL Analytics] レポートストレージで処理されます。（A4T）レポートを使用して、<br />[!DNL Analytics] データが [!DNL Analytics] と [!DNL Target] の両方に表示できるようになります。 |

現在は、SPAで `triggerView()` が実装される場所にかかわらず、[!UICONTROL Views] ールとアクションがキャッシュから取得され、サーバーを呼び出すことなくユーザーに表示されます。 `triggerView()` は、インプレッション数を増分して記録するために、[!DNL Target] バックエンド に通知リクエストもおこないます。ビューを使用した SPA での at.js について詳しくは、「[シングルページアプリケーションの実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)」を参照してください。

（全幅に拡大するには、画像をクリックします）。

![target フロー at.js 2.x triggerView](/help/dev/implement/client-side/assets/atjs-20-triggerview.png "Target フロー at.js 2.x triggerView"){zoomable="yes"}

| 手順 | 詳細 |
| --- | --- |
| 1 | `triggerView()` をSPAで呼び出して [!UICONTROL View] をレンダリングし、アクションを適用して視覚的要素を変更します。 |
| 2 | ビューのターゲットコンテンツがキャッシュから読み取られます。 |
| 3 | デフォルトコンテンツがちらつくことなく、可能な限り迅速にターゲットコンテンツが表示されます。 |
| 4 | アクティビティ内の訪問者をカウントして指標を増分するための通知リクエストが [!DNL Target] [!UICONTROL Profile Store] ーザーに送信されます。 |
| 5 | [!DNL Analytics] データが [!UICONTROL Data Collection Servers] に送信されました。 |
| 6 | [!DNL Target] データは、SDID を介して [!DNL Analytics] データと照合され、[!DNL Analytics] レポートストレージに処理されます。 A4T レポートを使用して、[!DNL Analytics] データが [!DNL Analytics] と [!DNL Target] の両方に表示できるようになります。 |

### ビデオ： at.js 2.x のアーキテクチャ図

at.js 2.x は、Adobe Target の SAP のサポートを強化し、Adobe Target と他の Experience Cloud を統合します。このビデオでは、すべてがどのように結び付いているかを説明します。

>[!VIDEO](https://video.tv.adobe.com/v/26250/?quality=12)

詳しくは、[at.js 2.x の仕組みについて](https://experienceleague.adobe.com/docs/target-learn/tutorials/implementation/understanding-how-atjs-20-works.html)を参照してください。

## at.js 1.x の図

次の図は、at.js 1.x のワークフローを理解するのに役立ちます。

（全幅に拡大するには、画像をクリックします）。

![Target フロー at.js 1.x](/help/dev/implement/client-side/assets/target-flow.png "Target フロー at.js 1.x"){zoomable="yes"}

| 手順 | 説明 | 呼び出し | 説明 |
|--- |--- |--- |--- |
| 1 | ユーザーが認証されている場合、の呼び出しはExperience CloudID （MCID）を返し、別の呼び出しは顧客 ID を同期します。 | 2 | at.js ライブラリがドキュメント本文を同期的に読み込み、非表示にします。 |
| 3 | すべての設定済みパラメーター、MCID、SDID および顧客 ID（オプション）を含む、グローバル mbox リクエストがおこなわれます。 | 4 | プロファイルスクリプトが実行されてから、プロファイルストアにフィードされます。ストアは、オーディエンスライブラリから選定オーディエンス（Adobe AnalyticsやAudience Managerなどで共有されたオーディエンスなど）をリクエストします。<br />顧客属性がバッチ処理でプロファイルストアに送信されます。 |
| 5 | URL、mbox パラメーターおよびプロファイルデータに基づいて、[!DNL Target] がどのアクティビティおよびエクスペリエンスを訪問者に返すかを決定します。 | 6 | ターゲットとなるコンテンツが（オプションで、追加のパーソナライゼーションに関するプロファイル値を含めて）ページに送り返されます。<br />デフォルトコンテンツがちらつくことなく、可能な限り迅速にエクスペリエンスが表示されます。 |
| 7 | Analytics データがデータ収集サーバーに送信されます。 | 8 | ターゲットデータは、SDID を介して Analytics データと照合され、Analytics レポートストレージ内で処理されます。<br />[!UICONTROL Analytics for Target] （A4T）レポートを使用して、Analytics データが Analytics と [!DNL Target] の両方に表示できるようになります。 |

### ビデオ — Office hours：at.js のヒントと概要（2019年6月26日（PT））

この動画は、[!UICONTROL Adobe Customer Care] チーム主導で取り組んでいる「Office Hours」の録画です。

* at.js を使用する利点
* at.js の設定
* ちらつき処理
* at.js のデバッグ
* 既知の問題
* FAQ

>[!VIDEO](https://video.tv.adobe.com/v/27959/?quality=12)

## at.js による HTML コンテンツを使用したオファーのレンダリング方法

HTML コンテンツを使用してオファー をレンダリングする場合、at.js は次のアルゴリズムを適用します。

1. 画像がプリロードされます（HTML コンテンツに `<img>` タグがある場合）。

1. HTML コンテンツが DOM ノードに添付されます。

1. インラインスクリプトが実行されます（`<script>` タグで囲まれたコード）。

1. リモートスクリプトが非同期的に読み込まれ、実行されます（`src` 属性の `<script>` タグ）。

重要事項：

* at.js は、非同期的に読み込まれるので、リモートスクリプトの実行順序を保証しません。
* インラインスクリプトは、後で読み込まれて実行されるので、リモートスクリプトへの依存関係を持たせないようにします。
