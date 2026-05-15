---
keywords: system diagram, flicker, at.js, implementation, javascript library, js, atjs, $8
description: ページの読み込み時のワークフローを理解するのに役立つ  [!DNL Target]  at.js JavaScript ライブラリ関数（システム図を含む）について説明します。
title: at.js Javascript ライブラリはどのように機能しますか？
feature: at.js
exl-id: 9183797c-857b-4b7f-a573-6bb1d583f7b1
TQID: https://experienceleague.adobe.com/ZyfwRiSeZDL-gFA-3MehXoNO5XhdANPaAmHqDxVeQ-g
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d3cdead0-685a-4489-9250-4bb709942f66
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1190
ht-degree: 56%

---

# at.js の仕組み

クライアントサイドで [!DNL Adobe Target] を実装するには、at.js JavaScript ライブラリを使用する必要があります。

クライアント側での [!DNL Adobe Target] の実装では、[!DNL Target] アクティビティに関連付けられたエクスペリエンスをクライアントブラウザーに直接配信します。 ブラウザーは、表示するエクスペリエンスを決定して表示します。 クライアント側の実装では、WYSIWYG エディターの [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) （VEC）または非ビジュアルインターフェイスである[フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html) を使用して、テストエクスペリエンスとパーソナライゼーションエクスペリエンスを作成できます。

## at.js について

at.js ライブラリは、[!DNL Adobe Target]のクライアントサイド実装のための実装ライブラリです。 at.js ライブラリは、Web 実装のページ読み込み時間を改善し、シングルページアプリケーション向けのより優れた実装オプションを提供します。 at.js は推奨される実装ライブラリであり、頻繁にアップデートされて新しい機能が追加されます。 すべてのユーザーが[最新バージョンのat.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md)を実装または移行することをお勧めします。

詳細については、[Target JavaScript ライブラリ](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#libraries)を参照してください。

以下に示す[!DNL Target]実装では、次のAdobe Experience Cloud ソリューションが実装されています：[!DNL Analytics]、Target、および[!DNL Audience Manager]。 さらに、次の[!DNL Experience Cloud] コアサービスが実装されています：[!DNL Adobe Experience Platform]、[!UICONTROL Audiences]、および[!UICONTROL Visitor ID Service]。

## at.js 1.*x*&#x200B;とat.js 2.x ワークフロー図の違いは何ですか？

2.Oで1.*x*&#x200B;から導入された相違点について詳しくは、[at.js 1.xからat.js 2.x](/help/dev/implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md)へのアップグレードを参照してください。

抽象度の高い表示では、2 つのバージョン間にいくつかの違いがあります。

* at.js 2.x は、グローバル mbox リクエストの概念がなく、ページ読み込みリクエストを使用します。 ページ読み込みリクエストは、Web サイトの最初のページ読み込み時に適用されるコンテンツを取得するリクエストとして表示できます。
* at.js 2.xは、[!UICONTROL Views]という概念を管理します。これは、シングルページアプリケーション（SPA）に使用されます。 at.js 1.*x*&#x200B;はこの概念を認識していません。

## at.js 2.x 図

次の図は、[!UICONTROL Views]を使用したat.js 2.xのワークフローと、これがSPA統合をどのように強化するかを理解するのに役立ちます。 at.js 2.x で使用されている概念に関するより詳しい概要については、「[シングルページアプリケーションの実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)」を参照してください。

（画像をクリックして全幅に拡大します）。

![at.js 2.x](/help/dev/implement/client-side/assets/system-diagram-atjs-20.png "at.js 2.x"){zoomable="yes"}のTarget フロー

| 手順 | 詳細 |
| --- | --- |
| 1 | 呼び出しユーザーが認証されると、呼び出しが [!UICONTROL Experience Cloud ID]を返し、別の呼び出しが顧客 ID を同期します。 |
| 2 | at.js ライブラリは、ドキュメント本文を同期して読み込み、非表示にします。<br />at.jsは、オプションの事前非表示スニペットをページに実装して、非同期で読み込むこともできます。 |
| 3 | すべての設定済みパラメーター（MCID、SDID および顧客 ID）を含む、ページ読み込みリクエストがおこなわれます。 |
| 4 | プロファイルスクリプトが実行され、[!UICONTROL Profile Store]に入力されます。 Storeは、[!UICONTROL Audience Library]から適格なオーディエンスをリクエストします（例えば、[!DNL Adobe Analytics]、[!DNL Audience Manager]などから共有されたオーディエンス）。<br />顧客属性は、バッチプロセスで[!UICONTROL Profile Store]に送信されます。 |
| 5 | URL リクエストパラメーターとプロファイルデータに基づいて、[!DNL Target] が現在のページおよび将来のビューでどのアクティビティおよびエクスペリエンスを訪問者に返すかを決定します。 |
| 6 | ターゲットコンテンツが（オプションで、追加のパーソナライゼーションに関するプロファイル値を含めて）ページに送り返されます。<br />デフォルトコンテンツがちらつくことなく、可能な限り迅速に現在のページ上のターゲットコンテンツが表示されます。<br />SPA でのユーザーアクションの結果として表示されるビューのターゲットコンテンツは、ブラウザーにキャッシュされます。そのため、`triggerView()` を介してビューがトリガーされたときに追加のサーバー呼び出しをおこなわずに即座にターゲットコンテンツを適用できます。 |
| 7 | 分析データは[!UICONTROL Data Collection] サーバーに送信されます。 |
| 8 | ターゲットデータは、SDIDを介してAnalytics データと照合され、[!DNL Analytics] レポートストレージに処理されます。<br />[!DNL Analytics] その後、（A4T）レポートを使用して、[!DNL Analytics]と[!DNL Target]の両方でデータを表示できます。 |

現在、`triggerView()`がSPAに実装されている場所では、[!UICONTROL Views]とアクションがキャッシュから取得され、サーバーコールなしでユーザーに表示されます。 `triggerView()` は、インプレッション数を増分して記録するために、[!DNL Target] バックエンド に通知リクエストもおこないます。 ビューを使用した SPA での at.js について詳しくは、「[シングルページアプリケーションの実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)」を参照してください。

（画像をクリックして全幅に拡大します）。

![Target flow at.js 2.x triggerView](/help/dev/implement/client-side/assets/atjs-20-triggerview.png "Target flow at.js 2.x triggerView"){zoomable="yes"}

| 手順 | 詳細 |
| --- | --- |
| 1 | `triggerView()`がSPAで呼び出されて[!UICONTROL View]がレンダリングされ、アクションを適用してビジュアル要素を変更します。 |
| 2 | ビューのターゲットコンテンツがキャッシュから読み取られます。 |
| 3 | デフォルトコンテンツがちらつくことなく、可能な限り迅速にターゲットコンテンツが表示されます。 |
| 4 | アクティビティおよび増分指標で訪問者をカウントするために、通知リクエストが[!DNL Target] [!UICONTROL Profile Store]に送信されます。 |
| 5 | [!DNL Analytics] データを[!UICONTROL Data Collection Servers]に送信しました。 |
| 6 | [!DNL Target] データはSDIDを介して[!DNL Analytics] データと照合され、[!DNL Analytics] レポートストレージに処理されます。 [!DNL Analytics]個のデータは、A4T レポートを使用して[!DNL Analytics]と[!DNL Target]の両方で表示できます。 |

### ビデオ： at.js 2.x のアーキテクチャ図

at.js 2.x は、Adobe Target の SAP のサポートを強化し、Adobe Target と他の Experience Cloud を統合します。 このビデオでは、すべてがどのように結び付いているかを説明します。

>[!VIDEO](https://video.tv.adobe.com/v/26250/?quality=12)

詳しくは、[at.js 2.x の仕組みについて](https://experienceleague.adobe.com/docs/target-learn/tutorials/implementation/understanding-how-atjs-20-works.html)を参照してください。

## at.js 1.x の図

次の図は、at.js 1.xのワークフローを理解するのに役立ちます。

（画像をクリックして全幅に拡大します）。

![Target flow at.js 1.x](/help/dev/implement/client-side/assets/target-flow.png "Target flow at.js 1.x"){zoomable="yes"}

| 手順 | 説明 | 呼び出し | 説明 |
|--- |--- |--- |--- |
| 1 | コールは、ユーザーが認証された場合にExperience Cloud ID （MCID）を返します。別のコールは、お客様IDを同期します。 | 2 | at.js ライブラリがドキュメント本文を同期的に読み込み、非表示にします。 |
| 3 | すべての設定済みパラメーター、MCID、SDID および顧客 ID（オプション）を含む、グローバル mbox リクエストがおこなわれます。 | 4 | プロファイルスクリプトが実行されてから、プロファイルストアにフィードされます。 ストアは、オーディエンスライブラリから適格なオーディエンスをリクエストします（例えば、Adobe Analytics、Audience Managerなどから共有されたオーディエンス）。<br />お客様の属性は、バッチプロセスでプロファイルストアに送信されます。 |
| 5 | URL、mbox パラメーターおよびプロファイルデータに基づいて、[!DNL Target] がどのアクティビティおよびエクスペリエンスを訪問者に返すかを決定します。 | 6 | ターゲットとなるコンテンツが（オプションで、追加のパーソナライゼーションに関するプロファイル値を含めて）ページに送り返されます。<br />デフォルトコンテンツがちらつくことなく、可能な限り迅速にエクスペリエンスが表示されます。 |
| 7 | Analytics データがデータ収集サーバーに送信されます。 | 8 | ターゲットデータは、SDIDを介してAnalytics データと照合され、Analytics レポートストレージに処理されます。<br />Analytics データは、[!UICONTROL Analytics for Target] （A4T）レポートを介してAnalyticsと[!DNL Target]の両方で表示できます。 |

### ビデオ — Office hours：at.js のヒントと概要（2019年6月26日（PT））

このビデオは、[!UICONTROL Adobe Customer Care] チームが主導するイニシアチブである「Office Hours」の録画です。

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
