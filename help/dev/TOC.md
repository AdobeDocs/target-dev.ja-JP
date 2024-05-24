---
user-guide-title: Adobe Target開発者ガイド
breadcrumb-title: Target デベロッパーガイド
user-guide-description: web サイトやモバイルサイト、アプリ、ソーシャルメディアおよびその他のデジタルチャネルでの収益を最大化するために、顧客のエクスペリエンスを調整およびパーソナライズする方法について説明します。
source-git-commit: c963a070a7a4c5e7dc2915eb5ac7d60895340705
workflow-type: tm+mt
source-wordcount: '756'
ht-degree: 45%

---


# Adobe Target開発者ガイド {#developer}

+ [Adobe Target開発者ガイド](overview.md)
+ はじめに {#implementation}
   + 実装する前に {#before-implement}
      + [実装する前に](before-implement/considerations-before-you-implement-target.md)
      + [Target 実装の準備](before-implement/prepare-to-implement-target.md)
   + プライバシーとセキュリティ {#privacy}
      + [プライバシーの概要](before-implement/privacy/privacy.md)
      + [プライバシーとデータ保護規制](before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)
      + [Target の Cookie](before-implement/privacy/cookie-behavior.md)
      + [Target の Cookie の削除](before-implement/privacy/cookie-deleting.md)
      + [Target （at.js）に対するサードパーティ cookie の廃止の影響](/help/dev/before-implement/privacy/third-party-cookie-deprecation.md)
      + [Google Chrome SameSite cookie ポリシー](before-implement/privacy/google-chrome-samesite-cookie-policies.md)
      + [Apple Intelligent Tracking Prevention（ITP）2.x](before-implement/privacy/apple-itp-2x.md)
      + [コンテンツセキュリティポリシー（CSP）指令](before-implement/privacy/content-security-policy.md)
      + [Target のエッジノードを許可リストに登録する](before-implement/privacy/allowlist-edges.md)
   + データを Target に送信する方法 {#methods}
      + [メソッドの概要](before-implement/methods-to-get-data-into-target/methods-to-get-data-into-target.md)
      + [ページのパラメーター](before-implement/methods-to-get-data-into-target/page-parameters.md)
      + [ページ内プロファイル属性](before-implement/methods-to-get-data-into-target/in-page-profile-attributes.md)
      + [スクリプトプロファイル属性](before-implement/methods-to-get-data-into-target/script-profile-attributes.md)
      + [データプロバイダー](before-implement/methods-to-get-data-into-target/data-providers.md)
      + [プロファイル一括更新 API](before-implement/methods-to-get-data-into-target/bulk-profile-update-api.md)
      + [単一プロファイル更新 API](before-implement/methods-to-get-data-into-target/single-profile-update-api.md)
      + [顧客属性](before-implement/methods-to-get-data-into-target/customer-attributes.md)
      + [プロファイル API 設定](before-implement/methods-to-get-data-into-target/profile-api-settings.md)
   + [Target のセキュリティの概要](before-implement/target-security-overview.md)
   + [サポートされているブラウザー](before-implement/supported-browsers.md)
   + [TLS（Transport Layer Security）暗号化の変更](before-implement/tls-transport-layer-security-encryption.md)
   + [CNAME と Adobe Target](before-implement/implement-cname-support-in-target.md)
+ クライアントサイドの実装 {#client-side}
   + [概要：Target をクライアント側 web に実装する](implement/client-side/overview.md)
   + [Adobe Experience Platform Web SDK 実装の概要](implement/client-side/aep-web-sdk.md)
   + at.js の実装 {#at-js-implementation}
      + [at.js の概要](implement/client-side/atjs/how-atjs-works/overview.md)
      + at.js の仕組み {#at-js}
         + [at.js の仕組みの概要](implement/client-side/atjs/how-atjs-works/how-atjs-works.md)
         + [at.js によるちらつきの制御方法](implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)
         + [at.js の統合](implement/client-side/atjs/how-atjs-works/target-atjs-integrations.md)
      + at.js のデプロイ方法 {#deploy-at-js}
         + [at.js のデプロイ方法](implement/client-side/atjs/how-to-deployatjs/how-to-deployatjs.md)
         + [Adobe Experience Platform を使用した Target の実装](implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)
         + [タグマネージャーを使用しない Target の実装](implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)
         + [Dynamic Tag Manager（DTM）を使用した Target の実装](implement/client-side/atjs/how-to-deployatjs/implement-target-using-dtm.md)
         + [シングルページアプリケーション（SPA）への Target の実装](implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)
      + オンデバイス判定 {#on-device-decisioning}
         + [オンデバイス判定の概要](implement/client-side/atjs/on-device-decisioning/on-device-decisioning.md)
         + [サポートされる機能](implement/client-side/atjs/on-device-decisioning/supported-features.md)
         + [ルールアーティファクト](implement/client-side/atjs/on-device-decisioning/rule-artifact.md)
         + [トラブルシューティング](implement/client-side/atjs/on-device-decisioning/troubleshooting-on-device-decisioning.md)
      + at.js 関数 {#functions-overview}
         + [at.ｊs 関数の概要](implement/client-side/atjs/atjs-functions/atjs-functions.md)
         + [adobe.target.getOffer()](implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md)
         + [adobe.target.getOffers() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)
         + [adobe.target.applyOffer()](implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md)
         + [adobe.target.applyOffers() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)
         + [adobe.target.triggerView() - at.js 2.x](implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)
         + [adobe.target.trackEvent()](implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md)
         + [mboxCreate() - at.js 1.x](implement/client-side/atjs/atjs-functions/mboxcreate-atjs.md)
         + [targetGlobalSettings()](implement/client-side/atjs/atjs-functions/targetglobalsettings.md)
         + [mboxDefine() と mboxUpdate() - at.js 1.x](implement/client-side/atjs/atjs-functions/mboxdefine-mboxupdate-atjs-1x.md)
         + [targetPageParams()](implement/client-side/atjs/atjs-functions/targetpageparams.md)
         + [targetPageParamsAll()](implement/client-side/atjs/atjs-functions/targetpageparamsall.md)
         + [registerExtension() - at.js 1.x](implement/client-side/atjs/atjs-functions/registerextension-atjs-1x.md)
         + [sendNotifications() - at.js 2.1](implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md)
         + [at.js カスタムイベント](implement/client-side/atjs/atjs-functions/atjs-custom-events.md)
         + [Adobe Experience Cloud デバッガーを使用した at.js のデバッグ](implement/client-side/target-debugging-atjs/target-debugging-atjs.md)
         + [Target でのクラウドベースのインスタンスの使用](implement/client-side/target-debugging-atjs/targeting-using-cloud-based-instances.md)
      + [at.js の FAQ](implement/client-side/atjs/target-atjs-faq.md)
      + [at.js のバージョンの詳細](implement/client-side/atjs/target-atjs-versions.md)
      + [at.js 1.x から at.js 2.x へのアップグレード](implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md)
      + [at.js の cookie](implement/client-side/atjs/atjs-cookies.md)
   + [User-agent と client hints](implement/client-side/atjs/user-agent-and-client-hints.md)
   + グローバル mbox について {#global-mbox}
      + [グローバル mbox の概要について](implement/client-side/atjs/global-mbox/global-mbox-overview.md)
      + [グローバル mbox のカスタマイズ](implement/client-side/atjs/global-mbox/customize-global-mbox.md)
      + [レガシー実装のグローバル mbox の使用](implement/client-side/atjs/global-mbox/mbox-global-target-standard.md)
      + [グローバル mbox へのパラメーターの受け渡し](implement/client-side/atjs/global-mbox/pass-parameters-to-global-mbox.md)
      + [グローバル mbox に関するよくある質問](implement/client-side/atjs/global-mbox/global-mbox-faq.md)
+ サーバー側の実装 {#server-side}
   + [サーバー側：Target の実装の概要](implement/server-side/server-side-overview.md)
   + [Target SDK の概要](implement/server-side/sdk-guides/getting-started/getting-started.md)
   + [サンプルアプリ](implement/server-side/sdk-guides/sample-apps/sample-apps.md)
   + [Target の従来の API から Adobe I/O への移行](implement/server-side/transition-from-target-classic-apis.md)
   + 基本原則 {#core-principles}
      + [基本原則の概要](implement/server-side/sdk-guides/core-principles/overview.md)
      + [ユーザー ID とバケット化](implement/server-side/sdk-guides/core-principles/user-identification-and-bucketing.md)
      + [オーディエンスのターゲティング](implement/server-side/sdk-guides/core-principles/audience-targeting.md)
      + [イベントトラッキング](implement/server-side/sdk-guides/core-principles/event-tracking.md)
      + [ユーザー権限とプロパティ](implement/server-side/sdk-guides/core-principles/user-permissions-and-properties.md)
   + 統合 {#integration}
      + [統合の概要](implement/server-side/sdk-guides/integration-with-experience-cloud/overview.md)
      + [Experience CloudID サービス（ECID）](implement/server-side/sdk-guides/integration-with-experience-cloud/ecid.md)
      + [Analytics for Target（A4T）レポート](implement/server-side/sdk-guides/integration-with-experience-cloud/a4t-reporting.md)
      + [AAM セグメント](implement/server-side/sdk-guides/integration-with-experience-cloud/aam-segments.md)
   + オンデバイス判定 {#on-device-decisioning}
      + [オンデバイス判定の概要](implement/server-side/sdk-guides/on-device-decisioning/overview.md)
      + ルールアーティファクト {#rule-artifact}
         + [ルールアーティファクトの概要](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)
         + [Adobe Target SDK を使用したダウンロード](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-sdk.md)
         + [JSON ペイロードを使用したダウンロード](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-json.md)
         + [ルールアーティファクトの例](implement/server-side/sdk-guides/on-device-decisioning/rule-artifact-example.md)
      + [機能フラグを使用した A/B テストの実行](implement/server-side/sdk-guides/on-device-decisioning/execute-ab-tests-with-feature-flags.md)
      + [属性を使用した機能テストの実行](implement/server-side/sdk-guides/on-device-decisioning/execute-feature-tests-with-attributes.md)
      + [機能テストのロールアウトの管理](implement/server-side/sdk-guides/on-device-decisioning/manage-rollouts-for-feature-tests.md)
      + [パーソナライゼーションの配信](implement/server-side/sdk-guides/on-device-decisioning/deliver-personalization.md)
      + [サポートされる機能の概要](implement/server-side/sdk-guides/on-device-decisioning/supported-features.md)
      + [オンデバイス判定のトラブルシューティング](implement/server-side/sdk-guides/on-device-decisioning/troubleshooting.md)
      + [ベストプラクティス](implement/server-side/sdk-guides/best-practices/best-practices.md)
   + Node.js SDK リファレンス {#node-js}
      + [Node.js SDK の概要](implement/server-side/node-js/overview.md)
      + [Node.js SDK のインストール](implement/server-side/node-js/install-sdk.md)
      + [Node.js SDK の初期化](implement/server-side/node-js/initialize-sdk.md)
      + [オファーを取得（Node.js）](implement/server-side/node-js/get-offers.md)
      + [属性を取得（Node.js）](implement/server-side/node-js/get-attributes.md)
      + [通知の送信（Node.js）](implement/server-side/node-js/send-notifications.md)
      + [SDK イベント（Node.js）](implement/server-side/node-js/sdk-events.md)
      + [ロガー（Node.js）](implement/server-side/node-js/logger.md)
      + [プロキシ設定（Node.js）](implement/server-side/node-js/proxy-configuration.md)
   + Java SDK リファレンス {#java}
      + [Java SDK の概要](implement/server-side/java/overview.md)
      + [Java SDK のインストール](implement/server-side/java/install-sdk.md)
      + [Java SDK の初期化](implement/server-side/java/initialize-sdk.md)
      + [オファーを取得（Java）](implement/server-side/java/get-offers.md)
      + [属性を取得（Java）](implement/server-side/java/get-attributes.md)
      + [通知の送信（Java）](implement/server-side/java/send-notifications.md)
      + [SDK イベント （Java）](implement/server-side/java/sdk-events.md)
      + [ロガー（Java）](implement/server-side/java/logger.md)
      + [非同期要求（Java）](implement/server-side/java/asynchronous-requests.md)
      + [プロキシ設定（Java）](implement/server-side/java/proxy-configuration.md)
      + [カスタム HTTP クライアント設定（Java）](implement/server-side/java/custom-http-client.md)
      + [ユーティリティメソッド （Java）](implement/server-side/java/utility-methods.md)
   + .NET SDK リファレンス {#net}
      + [.NET SDK の概要](implement/server-side/net/overview.md)
      + [.Net SDK のインストール](implement/server-side/net/install-sdk.md)
      + [.NET SDK の初期化](implement/server-side/net/initialize-sdk.md)
      + [オファーを取得（.NET）](implement/server-side/net/get-offers.md)
      + [属性を取得（.NET）](implement/server-side/net/get-attributes.md)
      + [通知の送信（.NET）](implement/server-side/net/send-notifications.md)
      + [SDK イベント （.NET）](implement/server-side/net/sdk-events.md)
      + [非同期要求（.NET）](implement/server-side/net/asynchronous-requests.md)
   + Python SDK リファレンス {#python}
      + [Python SDK の概要](implement/server-side/python/overview.md)
      + [Python SDK のインストール](implement/server-side/python/install-sdk.md)
      + [Python SDK の初期化](implement/server-side/python/initialize-sdk.md)
      + [オファーを取得（Python）](implement/server-side/python/get-offers.md)
      + [属性を取得（Python）](implement/server-side/python/get-attributes.md)
      + [通知の送信（Python）](implement/server-side/python/send-notifications.md)
      + [SDK イベント （Python）](implement/server-side/python/sdk-events.md)
      + [非同期要求（Python）](implement/server-side/python/asynchronous-requests.md)
      + [ロガー（Python）](implement/server-side/python/logger.md)
+ [ハイブリッド実装](implement/hybrid/hybrid-overview.md)
+ [Recommendations実装](implement/recommendations/recommendations.md)
+ [Recommendations実装ベータ版](/help/dev/implement/recommendations/recommendations-beta.md)
+ モバイルアプリの実装 {#mobile-apps}
   + [モバイルアプリのための Target の概要](implement/mobile/overview.md)
   + [Target モバイルのプレビュー](implement/mobile/target-mobile-preview.md)
   + [Location Service の使用](implement/mobile/use-location-service.md)
   + [モバイルアプリ用 Target に関する FAQ](implement/mobile/mobile-faq.md)
   + [Web 表示を使用したネイティブアプリでの AEP Mobile SDK を使用した Target の実装](/help/dev/implement/mobile/native-app.md)
+ メールの実装 {#implement-email}
   + [電子メール：Target の実装の概要](implement/email/overview.md)
   + [画像用 adbox の作成](implement/email/testing-content-with-the-adbox.md)
   + [電子メール画像 adbox のテスト](implement/email/testing-email-image-adbox.md)
   + [リダイレクターの使用](implement/email/working-with-redirectors.md)
+ API ガイド {#api}
   + [Target API の概要](/help/dev/before-administer/target-api-overview.md)
   + [Target API の認証の設定](/help/dev/before-administer/configure-authentication.md)
   + 配信 API ガイド {#delivery-api}
      + [配信 API の概要](/help/dev/implement/delivery-api/overview.md)
      + [配信 API とやり取りする SDK](/help/dev/before-implement/delivery-api-overview/sdks.md)
      + [導入](/help/dev/before-implement/delivery-api-overview/getting-started.md)
      + [ユーザー権限（Premium）](/help/dev/before-implement/delivery-api-overview/user-permissions.md)
      + [訪問者の特定](/help/dev/before-implement/delivery-api-overview/identifying-visitors.md)
      + [単一またはバッチ配信](/help/dev/before-implement/delivery-api-overview/single-or-batch.md)
      + [プリフェッチ](/help/dev/before-implement/delivery-api-overview/prefetch.md)
      + [通知](/help/dev/before-implement/delivery-api-overview/notifications.md)
      + [Experience Cloudとの統合](before-implement/delivery-api-overview/integration.md)
      + [考慮事項と既知の制限事項](/help/dev/before-implement/delivery-api-overview/known-limitations.md)
      + [クライアントヒント](/help/dev/before-implement/delivery-api-overview/client-hints.md)
      + [配信 API](/help/dev/implement/delivery-api/delivery-api.md)
   + 管理 API {#admin-api}
      + [管理 API の概要](before-administer/admin-api-overview/admin-api-overview.md)
      + [Adobe Target管理 API](/help/dev/administer/admin-api/admin-api-overview-new.md)
   + プロファイル API {#profile-apis}
      + [プロファイル API の概要](/help/dev/administer/profile-api/profiles-api.md)
      + [プロファイルの取得](/help/dev/administer/profile-api/profile-fetch.md)
      + [プロファイルを更新](/help/dev/administer/profile-api/profile-api-overview.md)
      + [単一プロファイル更新 API](/help/dev/administer/profile-api/profile-single-api.md)
      + [プロファイル一括更新 API](/help/dev/administer/profile-api/profile-bulk-api.md)
   + [レポート API](/help/dev/administer/reporting-api/reporting-api.md)
   + RECOMMENDATIONS API {#recommendations-api}
      + [Recommendations API の概要](before-administer/recs-api/overview.md)
      + [API を使用したカタログの管理](before-administer/recs-api/manage-catalog.md)
      + [カスタム条件の管理](before-administer/recs-api/manage-custom-criteria.md)
      + [Recommendationsでの配信 API の使用](before-administer/recs-api/fetch-recs-server-side-delivery-api.md)
      + [RECOMMENDATIONS API](/help/dev/administer/recommendations-api/recommendations-api.md)
   + Models API {#models-api}
      + [Models API （ブロックリストへの登録）の概要](before-administer/models-api.md)
      + [Models API](/help/dev/administer/models-api/models-api-overview.md)
   + [Adobe Admin Consoleの API](/help/dev/before-implement/delivery-api-overview/adobe-console-api.md)
   + [Adobe Experience Platform Edge Networkサーバー API](/help/dev/before-implement/delivery-api-overview/aep-edge-network-server-api.md)
+ 実装パターン {#implementation-patterns}
   + [実装パターンの概要](/help/dev/patterns/pattern-overview.md)
   + at.js を使用したRecommendationsの実装パターン {#atjs}
      + [at.js を使用したRecommendations実装パターンの概要](/help/dev/patterns/recs-atjs/recs-implementation-pattern-atjs.md)
      + [SDK の初期化](/help/dev/patterns/recs-atjs/initialize-sdk.md)
      + [データ収集の設定](/help/dev/patterns/recs-atjs/data-collection.md)
      + [エクスペリエンスのレンダリング](/help/dev/patterns/recs-atjs/render-experiences.md)
      + [Notify Target](/help/dev/patterns/recs-atjs/notify-target.md)


