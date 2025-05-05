---
keywords: 実装，javascript ライブラリ，js, atjs, on-device decisioning, at.js, on-device, on device, implementation0
description: at.js ライブラリを使用して [!UICONTROL on-device decisioning] を実行する方法を説明します
title: オンデバイス判定は at.js JavaScript ライブラリでどのように機能しますか？
feature: at.js
exl-id: bd0e062f-c259-46f3-adba-e380af058ac8
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '3478'
ht-degree: 4%

---

# at.js の [!UICONTROL On-device decisioning]

バージョン 2.5.0 以降、at.js は [!UICONTROL on-device decisioning] を提供します。 [!UICONTROL On-device decisioning] を使用すると、ブラウザー上で [A/B テスト ](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html?lang=ja) および [ エクスペリエンスのターゲット設定 ](https://experienceleague.adobe.com/docs/target/using/activities/experience-targeting/experience-target.html?lang=ja) （XT）アクティビティをキャッシュして、[!DNL Adobe Target] Edge Networkへのネットワークリクエストをブロックすることなく、インメモリ判定を実行できます。

>[!NOTE]
>
>[!UICONTROL On-device decisioning] は、クライアントサイド実装とサーバーサイド実装の両方で使用できます。 この記事では、クライアントサイドの [!UICONTROL on-device decisioning] について説明します。 サーバーサイドの [!UICONTROL on-device decisioning] について詳しくは、サーバーサイド実装のドキュメント [ こちら ](../../../server-side/sdk-guides/on-device-decisioning/overview.md) を参照してください。

また、[!DNL Target] は、ライブサーバー呼び出しを通じて、実験や機械学習に基づく（ML 駆動の）パーソナライゼーションアクティビティから最も関連性が高い最新のエクスペリエンスを柔軟に提供します。 つまり、パフォーマンスが最も重要な場合は、[!UICONTROL on-device decisioning] の使用を選択できます。 ただし、最も関連性が高く、最新の ML ドリブン型のエクスペリエンスが必要な場合は、代わりにサーバーコールを行うことができます。

## [!UICONTROL on-device decisioning] の利点は何ですか？

[!UICONTROL on-device decisioning] の利点は次のとおりです。

* **驚くほど高速な意思決定とエクスペリエンスを実現します。** バケッティングと決定は、ネットワークリクエストがブロックされるのを回避するために、メモリ内およびブラウザーで実行されます。
* **アプリケーションのパフォーマンスの向上。** 実験を実行し、エンドユーザーのエクスペリエンスを損なうことなく、顧客とユーザーにパーソナライゼーションを提供します。
* **Google サイトの品質スコアが向上します。** インメモリで意思決定が行われるので、オンラインビジネスのGoogle サイト品質スコアを向上させて、消費者がより多くの情報を見つけられるようにします。
* **リアルタイム分析から説明します。** [Analytics for Target](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html?lang=ja) （A4T）レポートを使用して、アクティビティのパフォーマンスからリアルタイムでインサイトを得ます。 A4T を使用すると、重要な瞬間に戦略を転換できます。

## サポートされる機能

[!DNL Adobe Target] JS SDK を使用すると、意思決定のためのデータのパフォーマンスと鮮度を柔軟に選択できます。 つまり、最も関連性が高く、魅力的なパーソナライズされたコンテンツを機械学習を通じて配信することが最も重要な場合は、ライブサーバーを呼び出す必要があります。 ただし、パフォーマンスがより重要な場合は、オンデバイスおよびメモリ内の決定を行う必要があります。 動作する [!UICONTROL on-device decisioning] については、サポートされている機能のリストを参照してください。

* アクティビティのタイプ
* オーディエンスのターゲティング
* 配分方法

詳しくは、[[!UICONTROL on-device decisioning]](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md) でサポートされる機能」を参照してください。

## [!UICONTROL on-device decisioning] の仕組み

[!UICONTROL on-device decisioning] を有効にして at.js をデプロイおよび初期化すると、A/B および XT のアクティビティ、オーディエンスおよびアセット用の [!UICONTROL on-device decisioning] を含んだ [ ルールアーティファクト ](/help/dev/implement/client-side/atjs/on-device-decisioning/rule-artifact.md) が、最も近い Akamai CDN から訪問者にダウンロードされ、訪問者のブラウザーにローカルにキャッシュされます。 エクスペリエンスを取得するためのリクエストが at.js から送信されると、キャッシュされたルールアーティファクトにエンコードされたメタデータに基づいて、返すエクスペリエンスに関する決定がメモリ内で行われます。

## 判定方法

[!UICONTROL on-device decisioning] では、Decisioning Method[!DNL Target] いう新しい設定が導入されています。 判定方法の設定は、at.js でのエクスペリエンスの提供方法を指定します。 判定方法には次の 3 つの値があります。

* サーバー側のみ
* オンデバイスのみ
* ハイブリッド

### サーバー側のみ

at.js 2.5.0 以降が実装され web プロパティにデプロイされる場合に標準で設定されているデフォルトの判定方法はサーバー側のみです。

サーバーサイドのみをデフォルト設定として使用すると、すべての決定は [!DNL Target] エッジネットワーク上で行われます。これにはブロッキングサーバーコールが必要になります。 このアプローチでは、待ち時間が徐々に増えるおそれがありますが、[Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=ja)、[Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=ja) （AP）および [ 自動ターゲット ](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=ja) のアクティビティなど、[!DNL Target] の機械学習機能を適用する機能を提供するなど、大きなメリットもあります。

さらに、すべてのセッションやチャネルにわたって永続化された [!DNL Target] のユーザープロファイルを使用して、パーソナライズされたエクスペリエンスを強化することで、ビジネスに大きな成果をもたらすことができます。

最後に、サーバーサイドのみでは、Adobe Experience Cloudを使用して、Audience ManagerセグメントやAdobe Analytics セグメントを介してターゲティングできるオーディエンスを微調整できます。

次の図は、訪問者、ブラウザー、at.js 2.5.0 以降と [!DNL Adobe Target] Edge ネットワークの間のやり取りを示しています。 次のフロー図は、新規訪問者と再訪問者を取り込みます。

（全幅に拡大するには、画像をクリックします）。

![ サーバー側のみのフロー図 ](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/server-side-only.png " サーバー側のみのフロー図 "){zoomable="yes"}

次のリストは、図の数値に対応します。

| 手順 | 説明 |
| --- | --- |
| 1 | Experience Cloud訪問者 ID は、[Adobe Experience Cloud ID サービス ](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja&) から取得されます。 |
| 2 | at.js ライブラリがドキュメント本文を同期的に読み込み、非表示にします。<br />   at.js ライブラリは、ページに実装されたオプションの事前非表示スニペットを使用して非同期で読み込むこともできます。 |
| 3 | at.js ライブラリは、ちらつきを防ぐために本文を非表示にします。 |
| 4 | ページ読み込みリクエストは、すべての設定済みパラメーター（ECID、顧客 ID、カスタムパラメーター、ユーザープロファイルなど）を含めて行われます。 |
| 5 | プロファイルスクリプトは、を実行してからプロファイルストアにフィードします。<br /> プロファイルストアは、オーディエンスライブラリから選定オーディエンス（Adobe Analytics、Adobe Audience Managerなどから共有されたオーディエンスなど）をリクエストします。<br />顧客属性がバッチ処理でプロファイルストアに送信されます。 |
| 6 | プロファイルストアは、アクティビティをフィルタリングするためのオーディエンスの選定およびバケット化に使用されます。 |
| 7 | 結果として生成されるコンテンツは、エクスペリエンスがライブ [!DNL Target] アクティビティから決定された後に選択されます。 |
| 8 | at.js ライブラリは、レンダリングが必要なエクスペリエンスに関連付けられた、ページ上の対応する要素を非表示にします。 |
| 9 | at.js ライブラリは、ページの残りを読み込んで訪問者が表示できるように本文を表示します。 |
| 10 | at.js ライブラリは、DOM を操作して、[!DNL Target] Edge Networkーからエクスペリエンスをレンダリングします。 |
| 11 | エクスペリエンスが訪問者向けにレンダリングされます。 |
| 12 | Web ページ全体が読み込まれます。 |
| 13 | Analytics データがデータ収集サーバーに送信されます。 |
| 14 | ターゲットデータは、SDID を介して Analytics データと照合され、Analytics レポートストレージで処理されます。 [!UICONTROL Analytics for Target] （A4T）レポートを使用して、Analytics データが Analytics と [!DNL Target] の両方に表示できるようになります。 |

### オンデバイスのみ

オンデバイスのみは、web ページ全体でのみ使用する場合に at.js 2.5.0 以降で設定する必要 [!UICONTROL on-device decisioning] ある判定方法です。

決定 [!UICONTROL On-device decisioning]、エクスペリエンススコアに該当するすべてのアクティビティを含んだ、キャッシュされたルールアーティファクトから行われるので、エクスペリエンスとパーソナライゼーションアクティビティを超高速で配信で [!UICONTROL on-device decisioning] ます。

どのアクティビティが [!UICONTROL on-device decisioning] の対象となるかについて詳しくは、[[!UICONTROL on-device decisioning]](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md) でサポートされる機能」を参照してください。

この判定方法は、Target からの決定を必要とするすべてのページでパフォーマンスが非常に重要な場合にのみ使用してください。 さらに、この判定方法を選択した場合、[!UICONTROL on-device decisioning] の条件を満たさない [!DNL Target] アクティビティは配信も実行もされないことに注意してください。 at.js ライブラリ 2.5.0 以降は、キャッシュされたルールアーティファクトのみを探して決定を下すように設定されています。

次の図は、訪問者、ブラウザー、at.js 2.5.0 以降と Akamai CDN の間のやり取りを示しています。 Akamai CDN は、訪問者の初回訪問のルールアーティファクトをキャッシュします。 新規訪問者の最初のページ訪問では、JSON ルールアーティファクトを Akamai CDN からダウンロードして、訪問者のブラウザー上でローカルにキャッシュする必要があります。 JSON ルールアーティファクトがダウンロードされると、ネットワーク呼び出しをブロックすることなく、すぐに決定が行われます。 次のフロー図は、新規訪問者を取り込んでいます。

（全幅に拡大するには、画像をクリックします）。

![ オンデバイスのみのフロー図 ](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/on-device-only.png " オンデバイスのみのフロー図 "){zoomable="yes"}

次のリストは、図の数値に対応します。

>[!NOTE]
>
>管理サーバー [!DNL Adobe Target]、[!UICONTROL on-device decisioning] 用の対象となるすべてのアクティビティを選定し、JSON ルールアーティファクトを生成して、Akamai CDN に伝播します。 アクティビティは、Akamai CDN に伝播する新しい JSON ルールアーティファクトを出力するための更新について継続的に監視されます。

| 手順 | 説明 |
| --- | --- |
| 1 | Experience Cloud訪問者 ID は、[Adobe Experience Cloud ID サービス ](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) から取得されます。 |
| 2 | at.js ライブラリがドキュメント本文を同期的に読み込み、非表示にします。<br />at.js ライブラリは、ページに実装されたオプションの事前非表示スニペットを使用して非同期で読み込むこともできます。 |
| 3 | at.js ライブラリは、ちらつきを防ぐために本文を非表示にします。 |
| 4 | at.js ライブラリは、訪問者に最も近い Akamai CDN から JSON ルールアーティファクトを取得するリクエストを行います。 |
| 5 | Akamai CDN が JSON ルールアーティファクトで応答します。 |
| 6 | JSON ルールアーティファクトは、訪問者のブラウザーにローカルにキャッシュされます。 |
| 7 | at.js ライブラリは JSON ルールアーティファクトを解釈し、エクスペリエンスを取得するための決定を実行し、テストされた要素を非表示にします。 |
| 8 | at.js ライブラリは、ページの残りを読み込んで訪問者が表示できるように本文を表示します。 |
| 9 | at.js ライブラリは、DOM を操作して、キャッシュされた JSON ルールアーティファクトからエクスペリエンスをレンダリングします。 |
| 10 | エクスペリエンスが訪問者向けにレンダリングされます。 |
| 11 | Web ページ全体が読み込まれます。 |
| 12 | Analytics データはデータ収集サーバーに送信されます。 ターゲットデータは、SDID を介して Analytics データと照合され、Analytics レポートストレージで処理されます。 [!UICONTROL Analytics for Target] （A4T）レポートを使用して、Analytics データが Analytics と [!DNL Target] の両方に表示できるようになります。 |

次の図は、訪問者、ブラウザー、at.js 2.5.0 以降および訪問者の後続のページヒットまたは再訪問に対するキャッシュされた JSON ルールアーティファクトの間のインタラクションを示しています。 JSON ルールアーティファクトは既にキャッシュされており、ブラウザーで使用できるので、ネットワーク呼び出しをブロックすることなく、すぐに決定が行われます。 このフロー図は、その後のページナビゲーションまたは再訪問者を取り込みます。

（全幅に拡大するには、画像をクリックします）。

![ 後続のページナビゲーションおよび繰り返し訪問のオンデバイスのみのフロー図 ](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/on-device-only-subsequent.png " 後続のページナビゲーションおよび繰り返し訪問のオンデバイスのみのフロー図 "){zoomable="yes"}

次のリストは、図の数値に対応します。

>[!NOTE]
>
>管理サーバー [!DNL Adobe Target]、[!UICONTROL on-device decisioning] 用の対象となるすべてのアクティビティを選定し、JSON ルールアーティファクトを生成して、Akamai CDN に伝播します。 アクティビティは、Akamai CDN に伝播する新しい JSON ルールアーティファクトを出力するための更新について継続的に監視されます。

| 手順 | 説明 |
| --- | --- |
| 1 | Experience Cloud訪問者 ID は、[Adobe Experience Cloud ID サービス ](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) から取得されます。 |
| 2 | at.js ライブラリがドキュメント本文を同期的に読み込み、非表示にします。<br />at.js ライブラリは、ページに実装されたオプションの事前非表示スニペットを使用して非同期で読み込むこともできます。 |
| 3 | at.js ライブラリは、ちらつきを防ぐために本文を非表示にします。 |
| 4 | at.js ライブラリは、JSON ルールアーティファクトを解釈し、エクスペリエンスを取得するためにメモリ内の決定を実行します。 |
| 5 | テストされた要素は非表示です。 |
| 6 | at.js ライブラリは、ページの残りを読み込んで訪問者が表示できるように本文を表示します。 |
| 7 | at.js ライブラリは、DOM を操作して、キャッシュされた JSON ルールアーティファクトからエクスペリエンスをレンダリングします。 |
| 8 | エクスペリエンスが訪問者向けにレンダリングされます。 |
| 9 | Web ページ全体が読み込まれます。 |
| 10 | Analytics データはデータ収集サーバーに送信されます。 ターゲットデータは、SDID を介して Analytics データと照合され、Analytics レポートストレージで処理されます。 [!UICONTROL Analytics for Target] （A4T）レポートを使用して、Analytics データが Analytics と [!DNL Target] の両方に表示できるようになります。 |

### ハイブリッド

ハイブリッドは、[!UICONTROL on-device decisioning] アクティビティと、[!DNL Adobe Target] Edge ネットワークへのネットワーク呼び出しを必要とするアクティビティの両方を実行する必要がある場合に、at.js 2.5.0 以降で設定する必要がある判定方法です。

[!UICONTROL on-device decisioning] アクティビティとサーバーサイドアクティビティの両方を管理している場合は、ページにをデプロイしてプロビジョニングする方法を考える際に、少し複雑で面倒な場合 [!DNL Target] あります。 ハイブリッド判定方法では、サーバー側実行が必要 [!DNL Target] アクティビティについて [!DNL Adobe Target] Edge ネットワークに対してサーバーコールを行う必要がある場合と、オンデバイス判定のみを実行する必要がある場合を把握しています。

JSON ルールアーティファクトには、mbox がサーバー側アクティビティを実行しているか、[!UICONTROL on-device decisioning] アクティビティを実行しているかを at.js に通知するメタデータが含まれています。 この判定方法では、迅速に配信すべきアクティビティを [!UICONTROL on-device decisioning] を通じて行い、機械学習によるより強力なパーソナライゼーションを必要とするアクティビティについては、[!DNL Adobe Target] Edge ネットワークを介して行います。

次の図は、新しい訪問者が初めてページにアクセスする場合の、訪問者、ブラウザー、at.js 2.5.0 以降、Akamai CDN および [!DNL Adobe Target] Edge Network間のインタラクションを示しています。 この図から得られるメリットは、JSON ルールアーティファクトが非同期でダウンロードされる一方で、決定は [!DNL Adobe Target] Edge Network を介して行われることです。

このアプローチにより、多くのアクティビティを含む可能性のあるアーティファクトのサイズが、決定の待ち時間に悪影響を与えないようにします。 JSON ルールアーティファクトを同期的にダウンロードし、その後に決定を行うと、待ち時間に悪影響を及ぼす可能性があり、一貫性がなくなる場合があります。 したがって、ハイブリッド判定方法は、新しい訪問者の決定を常にサーバー側で呼び出すためのベストプラクティスの推奨事項です。JSON ルールのアーティファクトは並行してキャッシュされます。 以降のページ訪問および再来訪では、JSON ルールアーティファクトを通じてキャッシュとメモリ内から決定が行われます。

（全幅に拡大するには、画像をクリックします）。

![ 初回訪問者用ハイブリッドフロー図 ](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/hybrid-first-time-visitor.png " 初回訪問者用ハイブリッドフロー図 "){zoomable="yes"}

次のリストは、図の数値に対応します。

>[!NOTE]
>
>管理サーバー [!DNL Adobe Target]、[!UICONTROL on-device decisioning] 用の対象となるすべてのアクティビティを選定し、JSON ルールアーティファクトを生成して、Akamai CDN に伝播します。 アクティビティは、Akamai CDN に伝播する新しい JSON ルールアーティファクトを出力するための更新について継続的に監視されます。

| 手順 | 説明 |
| --- | --- |
| 1 | Experience Cloud訪問者 ID は、[Adobe Experience Cloud ID サービス ](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) から取得されます。 |
| 2 | at.js ライブラリがドキュメント本文を同期的に読み込み、非表示にします。<br />at.js ライブラリは、ページに実装されたオプションの事前非表示スニペットを使用して非同期で読み込むこともできます。 |
| 3 | at.js ライブラリは、ちらつきを防ぐために本文を非表示にします。 |
| 4 | （ECID、顧客 ID、カスタムパラメーター、ユーザープロファイルなどの）設定済みのすべてのパラメーターを含むページ読み込みリクエストが [!DNL Adobe Target] Edge Networkに対して実行されます。 |
| 5 | 同時に、at.js は、訪問者に最も近い Akamai CDN から JSON ルールアーティファクトを取得するリクエストを行います。 |
| 6 | （[!DNL Adobe Target] Edge Network）プロファイルスクリプトが実行され、プロファイルストアにフィードされます。 プロファイルストアは、オーディエンスライブラリから選定オーディエンス（Adobe Analytics、Adobe Audience Managerなどから共有されたオーディエンスなど）をリクエストします。 |
| 7 | Akamai CDN が JSON ルールアーティファクトで応答します。 |
| 8 | プロファイルストアは、アクティビティをフィルタリングするためのオーディエンスの選定およびバケット化に使用されます。 |
| 9 | 結果として生成されるコンテンツは、エクスペリエンスがライブ [!DNL Target] アクティビティから決定された後に選択されます。 |
| 10 | at.js ライブラリは、レンダリングが必要なエクスペリエンスに関連付けられた、ページ上の対応する要素を非表示にします。 |
| 11 | at.js ライブラリは、ページの残りを読み込んで訪問者が表示できるように本文を表示します。 |
| 12 | at.js ライブラリは、DOM を操作して、[!DNL Target] Edge Networkーからエクスペリエンスをレンダリングします。 |
| 13 | エクスペリエンスが訪問者向けにレンダリングされます。 |
| 14 | Web ページ全体が読み込まれます。 |
| 15 | Analytics データはデータ収集サーバーに送信されます。 ターゲットデータは、SDID を介して Analytics データと照合され、Analytics レポートストレージで処理されます。 [!UICONTROL Analytics for Target] （A4T）レポートを使用して、Analytics データが Analytics と [!DNL Target] の両方に表示できるようになります。 |

次の図は、訪問者、ブラウザー、at.js 2.5.0 以降および後続のページナビゲーションまたは再訪問のためのキャッシュされた JSON ルールアーティファクトの間のインタラクションを示しています。 この図では、後続のページナビゲーションや再来訪に対してデバイス上の決定が行われるユースケースにのみ焦点を当てています。 特定のページでライブになっているアクティビティに応じて、サーバーサイドの呼び出しを行ってサーバーサイドの決定を実行できます。

（全幅に拡大するには、画像をクリックします）。

![ 後続のページナビゲーションと繰り返し訪問のハイブリッドフロー図 ](/help/dev/implement/client-side/atjs/on-device-decisioning/assets/hybrid-subsequent.png " 後続のページナビゲーションと繰り返し訪問のハイブリッドフロー図 "){zoomable="yes"}

次のリストは、図の数値に対応します。

>[!NOTE]
>
>管理サーバー [!DNL Adobe Target]、[!UICONTROL on-device decisioning] 用の対象となるすべてのアクティビティを選定し、JSON ルールアーティファクトを生成して、Akamai CDN に伝播します。 アクティビティは、Akamai CDN に伝播する新しい JSON ルールアーティファクトを出力するための更新について継続的に監視されます。

| 手順 | 説明 |
| --- | --- |
| 1 | Experience Cloud訪問者 ID は、[Adobe Experience Cloud ID サービス ](https://experienceleague.adobe.com/docs/id-service/using/home.html?lang=ja) から取得されます。 |
| 2 | at.js ライブラリがドキュメント本文を同期的に読み込み、非表示にします。<br />at.js ライブラリは、ページに実装されたオプションの事前非表示スニペットを使用して非同期で読み込むこともできます。 |
| 3 | at.js ライブラリは、ちらつきを防ぐために本文を非表示にします。 |
| 4 | エクスペリエンスを取得するリクエストが作成されます。 |
| 5 | at.js ライブラリは、JSON ルールアーティファクトが既にキャッシュされていることを確認し、エクスペリエンスを取得するためにメモリ内の決定を実行します。 |
| 6 | テストされた要素は非表示です。 |
| 7 | at.js ライブラリは、ページの残りを読み込んで訪問者が表示できるように本文を表示します。 |
| 8 | at.js ライブラリは、DOM を操作して、キャッシュされた JSON ルールアーティファクトからエクスペリエンスをレンダリングします。 |
| 9 | エクスペリエンスが訪問者向けにレンダリングされます。 |
| 10 | Web ページ全体が読み込まれます。 |
| 11 | Analytics データはデータ収集サーバーに送信されます。 ターゲットデータは、SDID を介して Analytics データと照合され、Analytics レポートストレージで処理されます。 [!UICONTROL Analytics for Target] （A4T）レポートを使用して、Analytics データが Analytics と [!DNL Target] の両方に表示できるようになります。 |

## [!UICONTROL on-device decisioning] を有効にするにはどうすればよいですか？

[!UICONTROL On-device decisioning] は、At.js 2.5.0 以降を使用するすべての [!DNL Target] ユーザーが利用できます。

[!UICONTROL on-device decisioning] を有効にするには：

>[!NOTE]
>
>オンデバイス判定の切り替えを有効または無効にするには、管理者または承認者 [ ユーザーの役割 ](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html?lang=ja) が必要です。

1. **[!UICONTROL Administration]**/**[!UICONTROL Implementation]**/**[!UICONTROL Account details]** をクリックします。
1. **[!UICONTROL Account details]** の下で、**[!UICONTROL On-Device Decisioning]** トグルを「オン」の位置にスライドします。

   切 ![[!UICONTROL On-device decisioning] 替え ](assets/on-device-decisioning-toggle.png)

   [!UICONTROL on-device decisioning] を有効にすると、「既存のすべて [!UICONTROL on-device decisioning] 適格なアクティビティをアーティファクトに含める」オプションが表示されます。
1. （条件付き）オンの対象となるすべてのライブ [!DNL Target] アクティビティをアーティファクトに自動的に含める場合は、切り替えスイッチを「[!UICONTROL on-device decisioning]」の位置にスライドします。

   このトグルをオフのままにすると、生成されたルールアーティファクトに含めるには、[!UICONTROL on-device decisioning] のアクティビティを再作成してアクティブ化する必要があります。 つまり、「オンデバイス判定」切り替えスイッチをオンにする前にライブ状態になっていたアクティビティは、ルールアーティファクトに含まれません。

オンデバイス判定トグルを有効に [!DNL Target] ると、クライアントの [ ルールアーティファクト ](/help/dev/implement/client-side/atjs/on-device-decisioning/rule-artifact.md) の生成と伝播が開始されます。

>[!WARNING]
>
>[!UICONTROL on-device decisioning] を使用するように [!DNL Adobe Target] SDK を初期化する前に、必ず切り替えスイッチを有効にします。 ルールアーティファクトを生成して Akamai CDN に反映させると、[!UICONTROL on-device decisioning] が機能するようになります。 最初のルールアーティファクトが生成されて Akamai CDN に伝播されるまでに、伝播に 5 ～ 10 分かかる場合があります。

## at.js 2.5.0 以降で [!UICONTROL on-device decisioning] を使用するように設定するにはどうすればよいですか？

1. **[!UICONTROL Administration]**/**[!UICONTROL Implementation]**/**[!UICONTROL Account details]** をクリックします。
1. **[!UICONTROL Implementation Methods]**/**[!UICONTROL Main Implementation Method]** で、at.js バージョンの横にある「**[!UICONTROL Edit]**」をクリックします（at.js 2.5.0 以降である必要があります）。

   ![ メインの実装方法の設定の編集 ](assets/main-implementation-method.png)

   >[!WARNING]
   >
   >これらのデフォルト設定を変更する前に、現在の実装に影響を与えないよう、Client Care にお問い合わせください。

1. 目的の判定方法を選択します。

   * サーバー側のみ
   * オンデバイスのみ
   * ハイブリッド

   ![at.js 設定パネルを編集 ](assets/global-settings.png)

### グローバル設定

すべての決定に対してデフォルトの判定方法 [!DNL Target] 設定できます。 様々な判定方法として、サーバー側のみ、オンデバイスのみ、ハイブリッドがあります。 [!DNL Target] UI で選択された判定方法は、`decisioningMethod` フィールドの下の `window.targetGlobalSettings` で設定されます。 `decisioningMethod` について詳しくは、[targetGlobalSettings （） ](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#decisioningmethod) を参照してください。

```javascript {line-numbers="true"}
<head> 
    <script type="text/javascript">

        window.targetGlobalSettings = { 
            clientCode: "yourClientCodeHere", 
            imsOrgId: "imsOrgId@AdobeOrg", 
            decisioningMethod: "on-device"

        }; 
    </script>

    <script type="text/javascript" src="at.js"></script> 
</head>
```

### カスタマイズされた設定

`window.targetGlobalSettings` で `decisioningMethod` を設定しても、ユースケースに従って各 [!DNL Adobe Target] 決定の `decisioningMethod` を上書きする場合は、At.js2.5.0 以降の [getOffers （） ](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md) 呼び出しで `decisioningMethod` を指定することで、この手順を実行できます。

```javascript {line-numbers="true"}
adobe.target.getOffers({ 

  decisioningMethod:"on-device", 
  request: { 
    execute: { 
      mboxes: [ 
        { 
          index: 0, 
          name: "homepage" 
        } 
      ] 
    } 
 } 
});
```

>[!NOTE]
>
>getOffers （）呼び出しで判定方法として「on-device」または「hybrid」を使用するには、グローバル設定が「on-device」または「hybrid」に `decisioningMethod` 定されていることを確認します。 at.js ライブラリ 2.5.0 以降では、ページに読み込んだ直後に、JSON ルールアーティファクトをダウンロードしてキャッシュするかどうかを知る必要があります。 グローバル設定の判定方法が「サーバー側」に設定され、「オンデバイス」または「ハイブリッド」の判定方法が getOffers （）呼び出しに渡された場合、at.js 2.5.0 以降では、オンデバイス判定を実行するために JSON ルールアーティファクトがキャッシュされません。

### アーティファクトキャッシュ TTL

Target は、メタデータ、ルールおよび条件で構成されるアーティファクトとして [!UICONTROL on-device decisioning] に適合するアクティビティを表します。 このアーティファクトは Akamai CDN にキャッシュされます。 ユーザーの初回の訪問時に、ユーザーのブラウザーが、[!UICONTROL on-device decisioning] のアクティビティを表すアーティファクトをダウンロードしてキャッシュします。

その後サイトに訪問すると、ブラウザーはアーティファクトの新しいバージョンをダウンロードする必要があるかどうかを自動的に確認します。 このチェックにより、待ち時間が追加されます。 アーティファクトキャッシュ TTL は、最後に成功したダウンロード以降に更新されたアーティファクトをブラウザーがチェックしないようにする時間（分）を定義します。 期間が長いほど、パフォーマンスが向上します。 時間枠が短いほど、データの鮮度は高くなりますが、待ち時間が追加されるというコストがかかります。

## アクティビティが実施要件を満たしていることを確認するにはどうすればよ [!UICONTROL on-device decisioning] ですか？

実施要件を満たしてい [!UICONTROL on-device decisioning] いアクティビティを作成すると、オンデバイス判定が実施要件を満たしていることを示すラベルがアクティビティの概要ページに表示されます。

![ アクティビティの概要ページのオンデバイス判定の実施要件ラベル。](assets/on-device-decisioning-eligible-label.png)

このラベルは、アクティビティが常に [!UICONTROL on-device decisioning] 経由で配信されるとは限りません。 at.js 2.5.0 以降が [!UICONTROL on-device decisioning] を使用するように設定されている場合にのみ、このアクティビティはデバイス上で実行されます。 at.js 2.5.0 以降がオンデバイスを使用するように設定されていない場合、このアクティビティは、at.js から行われるサーバー呼び出しを使用して引き続き配信されます。

オンデバイス判定実施要件フィルターを使用すると、アクティビティページで実施要件を満たしてい [!UICONTROL on-device decisioning] いすべてのアクティビティをフィルタリングできます。

![ アクティビティページのオンデバイス判定の実施要件フィルター。](assets/on-device-decisioning-filter.png)

>[!NOTE]
>
>実施要件を満た [!UICONTROL on-device decisioning] いアクティビティを作成してアクティブ化した後、生成されて Akamai CDN POS （POS）に伝播されるルールアーティファクトに含まれるまでに 5 ～ 10 分かかる場合があります。

## [!UICONTROL on-device decisioning] アクティビティが At.js 2.5.0 を介して確実に配信されるようにする手順の概要+?

1. [!DNL Adobe Target] UI にアクセスし、**[!UICONTROL Administration]** / **[!UICONTROL Implementation]** / **[!UICONTROL Account Details]** に移動して「**[!UICONTROL On-Device Decisioning]**」切り替えスイッチを有効にします。
1. 「**[!UICONTROL "Include all existing [!UICONTROL on-device decisioning] qualified activities in the artifact"]**」切替スイッチを有効にします。

   最初の JSON ルールのアーティファクトの生成には、最大 10 分かかることがあります。

1. [!UICONTROL on-device decisioning][&#128279;](/help/dev/implement/client-side/atjs/on-device-decisioning/supported-features.md) でサポートされている  アクティビティタイプを作成してアクティブ化し、適格 [!UICONTROL on-device decisioning] あることを確認します。
1. at.js 設定 UI を使用して、**[!UICONTROL Decisioning Method]** を **[!UICONTROL "Hybrid"]** または **[!UICONTROL "On-device only"]** に設定します。
1. At.js 2.5.0 以降をダウンロードしてページにデプロイします。
