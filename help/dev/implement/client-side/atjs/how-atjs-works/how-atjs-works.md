---
keywords: システム図，ちらつき， at.js，実装， javascript ライブラリ， js, atjs, $8
description: ページの読み込み時のワークフローを理解するのに役立つ  [!DNL Target]  at.js JavaScript ライブラリ関数（システム図を含む）について説明します。
title: at.js Javascript ライブラリはどのように機能しますか？
feature: at.js
exl-id: 9183797c-857b-4b7f-a573-6bb1d583f7b1
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1189'
ht-degree: 73%

---

# at.js の仕組み

クライアントサイドで [!DNL Adobe Target] を実装するには、at.js JavaScript ライブラリを使用する必要があります。

クライアント側での [!DNL Adobe Target] の実装では、[!DNL Target] アクティビティに関連付けられたエクスペリエンスをクライアントブラウザーに直接配信します。ブラウザーは、表示するエクスペリエンスを決定して表示します。クライアント側の実装では、WYSIWYG エディターの [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) （VEC）または非ビジュアルインターフェイスである[フォームベースの Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html) を使用して、テストエクスペリエンスとパーソナライゼーションエクスペリエンスを作成できます。

## at.js について

at.js ライブラリは、 [!DNL Adobe Target]. at.js ライブラリは、Web 実装のページ読み込み時間を改善し、シングルページアプリケーション向けのより優れた実装オプションを提供します。at.js は推奨される実装ライブラリであり、頻繁にアップデートされて新しい機能が追加されます。すべてのお客様に対して、[at.js の最新バージョン](/help/dev/implement/client-side/atjs/target-atjs-versions.md)を実装するか、最新バージョンに移行することをお勧めします。

詳細については、[Target JavaScript ライブラリ](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#libraries)を参照してください。

Adobe Analytics の [!DNL Target]以下の図に示す実装では、次のAdobe Experience Cloudソリューションが実装されています。 [!DNL Analytics]、Target および [!DNL Audience Manager]. さらに、次の [!DNL Experience Cloud] コアサービスは以下のように実装されています。 [!DNL Adobe Experience Platform], [!UICONTROL オーディエンス]、および [!UICONTROL 訪問者 ID サービス].

## at.js 1.*x* と at.js 2.x のワークフロー図の違いは何ですか？

2.0 と 1.x のバージョン間の違いについて詳しくは、「[at.js 1.x から at.js 2.x へのアップグレード](/help/dev/implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md)」を参照してください&#x200B;**。

抽象度の高い表示では、2 つのバージョン間にいくつかの違いがあります。

* at.js 2.x は、グローバル mbox リクエストの概念がなく、ページ読み込みリクエストを使用します。ページ読み込みリクエストは、Web サイトの最初のページ読み込み時に適用されるコンテンツを取得するリクエストとして表示できます。
* at.js 2.x は、 [!UICONTROL 件数]：シングルページアプリケーション (SPA) で使用されます。 at.js 1.*x* はこの概念に対応していません。

## at.js 2.x 図

次の図は、を使用した at.js 2.x のワークフローを理解するのに役立ちます。 [!UICONTROL 件数] これによってSPA統合がどのように強化されるかを説明します。 at.js 2.x で使用されている概念に関するより詳しい概要については、「[シングルページアプリケーションの実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)」を参照してください。

（全幅に拡大するには、画像をクリックします）。

![at.js 2.x での Target のフロー](/help/dev/implement/client-side/assets/system-diagram-atjs-20.png "at.js 2.x での Target のフロー"){zoomable=&quot;yes&quot;}

| 手順 | 詳細 |
| --- | --- |
| 1 | 呼び出しによって [!UICONTROL Experience CloudID] ユーザーが認証されると、別の呼び出しが顧客 ID を同期します。 |
| 2 | at.js ライブラリがドキュメント本文を同期的に読み込み、非表示にします。<br />at.js は、ページに実装されているオプションの非表示スニペットを使用して非同期で読み込むこともできます。 |
| 3 | すべての設定済みパラメーター（MCID、SDID および顧客 ID）を含む、ページ読み込みリクエストがおこなわれます。 |
| 4 | プロファイルスクリプトが実行されてから、 [!UICONTROL プロファイルストア]. 終わりから選定されたオーディエンスを再リクエストします [!UICONTROL オーディエンスライブラリ] ( 例： [!DNL Adobe Analytics], [!DNL Audience Manager]など )<br />[!UICONTROL 顧客属性がバッチ処理でプロファイルストアに送信されます。] |
| 5 | URL リクエストパラメーターとプロファイルデータに基づいて、[!DNL Target] が現在のページおよび将来のビューでどのアクティビティおよびエクスペリエンスを訪問者に返すかを決定します。 |
| 6 | ターゲットコンテンツが（オプションで、追加のパーソナライゼーションに関するプロファイル値を含めて）ページに送り返されます。<br />デフォルトコンテンツがちらつくことなく、可能な限り迅速に現在のページ上のターゲットコンテンツが表示されます。<br />SPA でのユーザーアクションの結果として表示されるビューのターゲットコンテンツは、ブラウザーにキャッシュされます。そのため、`triggerView()` を介してビューがトリガーされたときに追加のサーバー呼び出しをおこなわずに即座にターゲットコンテンツを適用できます。 |
| 7 | Analytics データの送信先： [!UICONTROL データ収集] サーバー。 |
| 8 | ターゲットデータは、SDID を使用して Analytics データに適合され、 [!DNL Analytics] レポートストレージ。<br />[!DNL Analytics]（A4T）レポートを使用して、[!DNL Analytics] データが [!DNL Target] と  の両方に表示できるようになります。 |

これで、`triggerView()` が SPA のどこに 実装されているかに関わらず、ビューとアクションはキャッシュから取得され、サーバー呼び出しなしでユーザーに表示されるようになります。`triggerView()` は、インプレッション数を増分して記録するために、[!DNL Target] バックエンド に通知リクエストもおこないます。ビューを使用した SPA での at.js について詳しくは、「[シングルページアプリケーションの実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)」を参照してください。

（全幅に拡大するには、画像をクリックします）。

![Target フローの at.js 2.x triggerView](/help/dev/implement/client-side/assets/atjs-20-triggerview.png "Target フローの at.js 2.x triggerView"){zoomable=&quot;yes&quot;}

| 手順 | 詳細 |
| --- | --- |
| 1 | `triggerView()`[!UICONTROL  は SPA で呼び出され、ビューをレンダリングし、ビジュアル要素を変更ためのアクションを適用します。] |
| 2 | ビューのターゲットコンテンツがキャッシュから読み取られます。 |
| 3 | デフォルトコンテンツがちらつくことなく、可能な限り迅速にターゲットコンテンツが表示されます。 |
| 4 | 通知リクエストが [!DNL Target] プロファイルストア に送信され、アクティビティで訪問者がカウントされ、指標が増分されます。 |
| 5 | [!DNL Analytics] に送信されたデータ [!UICONTROL データ収集サーバー]. |
| 6 | [!DNL Target] データは、SDID を使用して [!DNL Analytics] データに適合され、[!DNL Analytics] レポートストレージへと処理されます。[!DNL Analytics] その後、データは [!DNL Analytics] および [!DNL Target] A4T レポートを使用する。 |

### ビデオ： at.js 2.x のアーキテクチャ図

at.js 2.x は、Adobe Target の SAP のサポートを強化し、Adobe Target と他の Experience Cloud を統合します。このビデオでは、すべてがどのように結び付いているかを説明します。

>[!VIDEO](https://video.tv.adobe.com/v/26250/?quality=12)

詳しくは、[at.js 2.x の仕組みについて](https://experienceleague.adobe.com/docs/target-learn/tutorials/implementation/understanding-how-atjs-20-works.html)を参照してください。

## at.js 1.x の図

次の図は、at.js 1.x のワークフローを理解するのに役立ちます。

（全幅に拡大するには、画像をクリックします）。

![Target フローの at.js 1.x](/help/dev/implement/client-side/assets/target-flow.png "Target フローの at.js 1.x"){zoomable=&quot;yes&quot;}

| 手順 | 説明 | 呼び出し | 説明 |
|--- |--- |--- |--- |
| 1 | ユーザーが認証されると、呼び出しがExperience CloudID(MCID) を返し、別の呼び出しが顧客 ID を同期します。 | 2 | at.js ライブラリがドキュメント本文を同期的に読み込み、非表示にします。 |
| 3 | すべての設定済みパラメーター、MCID、SDID および顧客 ID（オプション）を含む、グローバル mbox リクエストがおこなわれます。 | 4 | プロファイルスクリプトが実行されてから、プロファイルストアにフィードされます。ストアは、オーディエンスライブラリから正規のオーディエンスをリクエストします ( 例えば、Adobe Analytics、Audience Managerなどから共有されたオーディエンス )。<br />顧客属性がバッチ処理でプロファイルストアに送信されます。 |
| 5 | URL、mbox パラメーターおよびプロファイルデータに基づいて、[!DNL Target] がどのアクティビティおよびエクスペリエンスを訪問者に返すかを決定します。 | 6 | ターゲットとなるコンテンツが（オプションで、追加のパーソナライゼーションに関するプロファイル値を含めて）ページに送り返されます。<br />デフォルトコンテンツがちらつくことなく、可能な限り迅速にエクスペリエンスが表示されます。 |
| 7 | Analytics データがデータ収集サーバーに送信されます。 | 8 | Target データは、SDID を使用して Analytics データに適合され、Analytics レポートストレージへと処理されます。<br />Analytics for [!DNL Target]（A4T）レポートを使用して、[!UICONTROL Analytics データが Analytics および ]Target に表示できるようになります。 |

### ビデオ — Office hours：at.js のヒントと概要（2019年6月26日（PT））

このビデオは、「Office Hours」の録画です。 [!UICONTROL Adobeカスタマーケア] チーム。

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
