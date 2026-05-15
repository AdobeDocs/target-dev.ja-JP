---
keywords: at.js リリース、at.js バージョン、リリースノート
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリの各バージョンの変更点の詳細を表示します。'
title: at.jsの各バージョンに含まれるもの
feature: at.js
exl-id: 609dacba-2ab8-45e9-b189-928d59938c98
TQID: https://experienceleague.adobe.com/95lXe4YAZ7mD12XBtKPB3ddFtGCJYdvlXR632qosuG4
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2:
  - id: df62f171-ac37-440f-8f0f-f41a72ebdd34
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: aa2f3246-cb95-4b30-8899-fdf7d73550cc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: c2be0313-b3ae-45e0-b454-d20bf54b23f2
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: d3cdead0-685a-4489-9250-4bb709942f66
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 929e1f10bc5dd0741f0fe28cd46435e680a4a308
workflow-type: tm+mt
source-wordcount: 5144
ht-degree: 63%

---

# at.js のバージョンの詳細

[!DNL Adobe Target] at. js JavaScript ライブラリの各バージョンの変更点についての詳細です。

>[!IMPORTANT]
>
>[!DNL Adobe Target]は、at.js 1.*x*&#x200B;とat.js 2.*x*&#x200B;の両方をサポートしています。
>
>at.js 1.*x*&#x200B;がメンテナンスモードに入りました。 [!DNL Target] チームは、必要に応じてバグ修正とセキュリティパッチをリリースします。
>
>[!DNL Target] チームは、at.js 2.*x*&#x200B;の完全なサポートを提供し、バグ修正、セキュリティパッチ、機能、およびパフォーマンス最適化を継続的にリリースします。
>
>対応するメジャーバージョンの以前のマイナーバージョンで検出された問題のバグ修正とセキュリティパッチを取得するには、1.*x*&#x200B;または2.*x*&#x200B;のいずれかの最新バージョンにアップグレードする必要があります。

[Adobe Experience Platform](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)のタグは、at.jsをアップグレードする際に推奨される方法です。 拡張機能の開発者は、継続的に拡張機能に新しい機能を追加し、頻繁にバグを修正します。 これらのアップデートは、拡張機能の新しいバージョンにパッケージ化され、アップグレードとしてAdobe Experience Platform カタログで利用できるようになります。 詳しくは、*タグの概要* ガイドの[拡張機能のアップグレード &#x200B;](https://experienceleague.adobe.com/docs/experience-platform/tags/ui/extensions/extension-upgrade.html?lang=ja)を参照してください。

## at.js バージョン 2.11.8（2025年3月31日（PT））

* サイズ変更および移動操作中にエッジケースを防ぐために、文字列サフィックス検証での CodeQL で特定された脆弱性を修正しました。 （TNT-51516）

## at.js バージョン 2.11.7（2025年2月26日（PT））

* `localStorage` が使用できない場合のテレメトリログを修正しました。 ブラウザーで `localStorage` を無効にしていた一部のお客様では、テレメトリによって問題が発生していました。

## at.js バージョン 2.11.6（2024年9月29日（PT））

* [!UICONTROL Visual Experience Composer]（VEC）または [!UICONTROL Form-Based Experience Composer] 内のリダイレクトオファーで [!DNL Target] が正しく動作しない問題を修正しました。

## at.js バージョン 2.11.5（2024年8月14日（PT））

* cookieの読み取りおよび書き込み操作に対するキャッシュ機能を実装し、繰り返されるコストのかかる文字列の解析および操作のオーバーヘッドを削減しました。
* 新しいURL Search Params APIが利用可能な場合に実装されました。これは、文字列を手動で解析および操作するよりも高速です。

## at.js バージョン 2.11.4（2024年1月24日（PT））

* at.jsを更新して、無効な地域データが配信APIに送信されないようにしました。

## at.js バージョン 2.11.3（2023年11月21日（PT））

* `at-content-rendering-failed` イベントで応答トークンが送信されない問題を修正しました。

## at.js バージョン 2.11.2（2023年10月26日（PT））

* カスタムイベントで送信された応答トークンで不整合が発生する問題を修正しました。

## at.js バージョン 2.11.1（2023年10月13日（PT））

* at.jsを実行しているページがquirks モードで動作しているときに、未検出のエラーが発生する問題を修正しました。

## at.js バージョン 2.11.0（2023年10月10日（PT））

* `targetGlobalSettings`のカスタム [!DNL Adobe Experience Platform] （AEP） `sandboxId`と`sandboxName`を設定するためのサポートを追加しました。これは、`getOffer/getOffers`回の呼び出しで配信APIに渡されます。
* セレクターで`:eq()`を連鎖させるためのシャドウ DOMの修正。

## at.js バージョン 2.10.3（2023年9月12日（PT））

* オファーがレンダリングされないときに`at-content-rendering-succeeded` カスタムイベントが誤ってトリガーされる問題を修正しました。 正しいイベント `at-content-rendering-no-offers`がトリガーされました。
* `at-content-rendering-failed` カスタムイベントのエラーオブジェクトに`eventToken`と`responseTokens`を追加しました。

## at.js バージョン 2.10.2（2023年3月7日（PT））

* `trackEvent` 関数が常にエラーを返す問題を修正しました。

## at.js バージョン 2.10.1（2023年2月2日（PT））

* 名前にドットの付いたパラメーターを含むオーディエンスルールが関与するアクティビティが、オンデバイス判定で期待したエクスペリエンスを返さなかったというバグを修正しました。
* mboxDisableが有効になっている場合でも、at.jsが配信呼び出しを実行するat.js 2.6.0で導入されたバグを修正しました。

## at.js バージョン 2.10.0（2022年9月19日（PT））

* サードパーティ Cookieのサポートを追加しました。

## at.js バージョン 2.9.0（2022年5月27日（PT））

* [User Agent Client Hints](user-agent-and-client-hints.md) のサポートを追加しました。
* 同じページ上の複数の mbox リクエストが異なるインプレッション ID を持っていたバグを修正しました。

## at.js バージョン 2.8.1（2022年1月28日（PT））

* On Device Decisioning （ODD） ハイブリッド実行モードで`pageLoad`がtarget-global-mboxにマッピングされない問題を修正しました。
* mbox リクエストの分析の詳細に関する問題を修正しました。
* 開発用の依存コンポーネントをアップグレードして、セキュリティの脆弱性を修正しました。

## at.js バージョン 2.8.0（2022年1月7日（PT））

[!DNL Target] at.js JavaScript ライブラリは、機能の使用状況とパフォーマンスのテレメトリデータを収集するようになりました。 個人データは収集されません。 この機能をオプトアウトするには、`targetGlobalSettings` で `telemetryEnabled` を false に設定します。 詳しくは、[targetGlobalSettings の telemetryEnabled](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#telemetryenabled) を参照してください。

## at.js バージョン 2.7.0（2021年10月28日（PT））

このリリースには、次の機能拡張が含まれています。

* [Web コンポーネント](https://developer.mozilla.org/ja/docs/Web/Web_Components)のサポートを追加しました。 このバージョンの at.js は、カスタム要素およびカスタム要素内の要素に対して、パーソナライズされたエクスペリエンスとオファーを作成し、テストするために必要です。 この機能は、[!DNL Target Standard/Premium] 21.10.5 リリースに含まれています。

## at.js 1.8.3 （2021年9月21日（PT））

このリリースには、次の変更が含まれています。

* `reactor-window`および`reactor-document`個のAdobe Experience Platform Launch モジュールを削除して、`window.default`または`document-default`が設定されているお客様に対してPlatform Launch ビルドが正しく機能するようにしました。
* at.js 1.8.3では、サードパーティドメイン cookieが適切に設定されていることを確認するために、`Samesite=None`と`Secure`を明示的に設定するようになりました。

## at.js 2.6.1（2021 年 8 月 16 日）

* オンデバイス判定を使用する際の「ハイブリッドモードでキャッシュされたアーティファクトがない」バグを修正しました。

## at.js 2.6.0（2021 年 7 月 16 日（PT））

* at.js 設定 `secureOnly` が `true` に設定されている場合は常にセキュア属性を cookie に追加するようになりました。
* `triggerView()` を使用する際に応答トークンを使用できるようになりました。
* `CONTENT_RENDERING_NO_OFFERS` イベントに関連する問題を修正しました。 これで、[!DNL Target] からコンテンツが返されない場合は常に、このイベントが正しくトリガーされます。
* `prefetch` リクエストを使用したときに、[!UICONTROL Analytics for Target]（A4T）のクリック指標の詳細が正しく返されます。
* UUID の生成では、`Math.random()` を使用しなくなり、`window.crypto` に基づくようになりました。
* `sessionId` cookie の有効期限は、すべてのネットワーク呼び出しで正しく延長されます。
* シングルページアプリケーション（SPA）ビューキャッシュの初期化が正しく処理され、`viewsEnabled`設定が尊重されるようになりました。 `viewsEnabled`を`false`値に設定すると、`triggerView()`関数が無効になります。 最初のページ読み込みについては、[操作順](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md#order)を参照してください。

## at.js 2.5.0 （2021年5月13日（PT））

at.js のこのリリースには、次の機能強化および変更が含まれています。

* at.js の[オンデバイス判定](/help/dev/implement/client-side/atjs/on-device-decisioning/on-device-decisioning.md)のサポート。
* Automated Personalization アクティビティでの[プレビューリンク](https://experienceleague.adobe.com/docs/target/using/activities/activity-qa/activity-qa.html?lang=ja)のサポート

このリリースでは、Microsoft Internet Explorer 10 以降のバージョンのサポートも削除されます。

## at.js 2.4.1（2021 年 3 月 23 日）

at.js のこのリリースはメンテナンスリリースで、次の機能強化および修正が含まれています。

* mbox リクエストに `targetPageParams` が含まれる問題を修正しました。 `targetPageParams` は `pageLoad` リクエストにのみ含まれる必要があります。 （TNT-40247）
* Adobe Experience Platform拡張機能で参照する最適化されたウィンドウとドキュメントのグローバル化。 （TNT-37124）

## at.js 2.4.0（2021 年 1 月 14 日）

at.js のこのリリースはメンテナンスリリースで、次の修正が含まれています。

* 配信APIのcustomerIdに、統合プロファイル/プラットフォーム IDのサポートを追加します。
* 無効なスタイルタグ挿入を修正します。

## at.js 2.3.3 （2020年11月13日（PT））

at.js のこのリリースはメンテナンスリリースで、次の修正が含まれています。

* mbox クリックの追跡とA4Tに関連する問題を修正しました。 0n クリックで、[!DNL Target]が正しいmboxおよびmbox パラメーターを使用して配信API呼び出しを実行しました。 ただし、SDIDが[!DNL Analytics]呼び出しのSDIDと一致しなかったため、ヒットのステッチとコンバージョンはありませんでした。 （TNT-38372）

## at.js 2.3.2（2020 年 7 月 24 日）

at.js のこのリリースはメンテナンスリリースで、次の修正が含まれています。

* スクリプトやコードが、ウィンドウまたはドキュメントにデフォルトのプロパティを追加した場合に発生するバグを修正しました。

## at.js 1.8.2 （2020年6月15日（PT））

at.js のこのリリースはメンテナンスリリースで、次の修正が含まれています。

* CNAMEとエッジの上書きを使用する際の問題を修正しました。at.js 1.*x*&#x200B;がサーバードメインを誤って作成し、[!DNL Target] リクエストが失敗する可能性があります。 （TNT-35064）

## at.js 2.3.1 リリース（2020年6月15日（PT））

at.js のこのリリースはメンテナンスリリースで、次の機能強化および修正が含まれています。

* [targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) 経由での `deviceIdLifetime` 設定のオーバーライドを可能にしました。 （TNT-36349）
* CNAMEとエッジの上書きを使用する際の問題を修正しました。at.js 2.*x*&#x200B;がサーバードメインを誤って作成し、[!DNL Target] リクエストが失敗する可能性があります。 （TNT-35065）
* [!DNL Target]拡張機能v2と[!UICONTROL Adobe Analytics Launch]拡張機能を使用する際の問題を修正しました。[!DNL Target]さんが[!DNL Analytics] `sendBeacon`の呼び出しを遅らせました。 （TNT-36407、TNT-35990、TNT-36000）

## at.js バージョン 2.3.0（2020年3月25日（PT））

at.js のこのリリースはメンテナンスリリースで、次の機能強化および修正が含まれています。

* 配信された[!DNL Target]件のオファーを適用する際に、ページ DOMに追加されるSCRIPT タグとSTYLE タグに対してコンテンツセキュリティポリシーの設定をサポートします。 顧客は`targetGlobalSettings.cspScriptNonce`と`targetGlobalSettings.cspStyleNonce`を設定して、at.jsが適用されたオファーに対応するスクリプトとスタイルタグノンスを設定できるようにすることができます。 詳しくは、[targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md)を参照してください。
* Google Tag Manager デプロイメント用のGoogle Closure コンパイラでat.jsをコンパイルする際の問題を修正しました。
* 顧客の実装との競合を避けるために、at.js チェック Cookieの名前を`check`から`at_check`に変更しました。

## at.js バージョン 1.8.1（2020年3月25日（PT））

at.js のこのリリースはメンテナンスリリースで、次の機能強化および修正が含まれています。

* 顧客の実装との競合を避けるために、at.js チェック Cookieの名前を`check`から`at_check`に変更しました。

## at.js バージョン 2.2.0（2019年10月10日（PT））

at.jsのこのリリースには、次の機能強化と修正が含まれています。

* [!DNL Adobe Analytics] コードがページ要素に存在しない場合、「[!DNL Analytics for Target]」（A4T）でクリック追跡がコンバージョンをレポートしなかった問題を修正しました。
* web ページでExperience Cloud ID サービス（ECID） v4.4とat.js 2.2の両方を使用する際のパフォーマンスが向上しました。
* 以前は、ECID は、at.js がエクスペリエンスを取得する前に、2 回のブロック呼び出しをおこなっていました。 これが 1 回の呼び出しに短縮され、パフォーマンスが大幅に向上しました。
* デフォルトのオファーのイベントトークンが送信済み通知に含まれていない、誤った先行取得ビュー処理を修正しました。

>[!NOTE]
>
>このパフォーマンス向上を活用するには、ECID拡張機能をv4.4にアップグレードします。

* at.js バージョン 2.2では、`serverState`という新しい設定も提供されています。 この設定は、[!DNL Target]のハイブリッド統合が実装されている場合に、ページのパフォーマンスを最適化するために使用できます。 ハイブリッド統合とは、クライアントサイドでat.js v2.2+とDelivery APIの両方を使用しているか、またはサーバーサイドで[!DNL Target] SDKを使用してエクスペリエンスを配信していることを意味します。 `serverState` には、at.js v2.2 以降で、サーバーサイドで取得したコンテンツからエクスペリエンスを直接適用し、提供されるページの一部としてクライアントに返す機能が備わっています。 詳しくは、[targetGlobalSettings](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverstate)の「serverState」を参照してください。

## at.js バージョン 1.8.0（2019年10月10日（PT））

at.jsのこのリリースには、次の機能強化と修正が含まれています。

* web ページでExperience Cloud ID サービス（ECID） v4.4とat.js 1.8の両方を使用する際のパフォーマンスが向上しました。
* 以前は、ECID は、at.js がエクスペリエンスを取得する前に、2 回のブロック呼び出しをおこなっていました。 これが 1 回の呼び出しに短縮され、パフォーマンスが大幅に向上しました。

>[!NOTE]
>
>このパフォーマンス向上を活用するには、ECID拡張機能をv4.4にアップグレードします。

## at.js バージョン 2.1.1（2019 年 7 月 24 日）

at.js のこのリリースはメンテナンスリリースで、次の機能強化および修正が含まれています。

（括弧内の問題番号はアドビ社内で使用されます。）

* Visual Experience Composer（VEC）の目標と設定ページでクリックの追跡指標を使用する際に複数のビーコンが実行される問題を修正しました。 （TNT-32812）
* `triggerView()` が 2 回以上オファーをレンダリングしない問題を修正しました。 （TNT-32780）
* 要求に Experience Cloud ID（ECID）情報が含まれるようにするための `triggerView()` の問題を修正しました。 （TNT-32776）
* 保存されたビューがない場合に `triggerView()` 通知が送付されない問題を修正しました。 （TNT-32614）
* URL に正しくないクエリ文字列パラメーターが含まれる場合に decodeURIcomponent の使用によりエラーが発生する問題を修正しました。 （TNT-32710）
* `Navigator.sendBeacon()` API を使用して送信された配信要求のコンテキストで、ビーコンフラグが「true」に設定されるようになりました。 （TNT-32683）
* 少数のお客様に対して、レコメンデーションオファーが web サイトに表示されない問題を修正しました。 顧客は配信API呼び出しでオファーのコンテンツを確認できますが、オファーはweb サイトに適用されませんでした。 （TNT-32680）
* 複数のエクスペリエンスにわたるクリックの追跡が期待どおりに機能していなかった問題を修正しました。 （TNT-32644）
* 最初の指標のレンダリングに失敗した後、at.js が 2 番目の指標に適用されなかった問題を修正しました。 （TNT-32628）
* `targetPageParams` 関数を使用して `mbox3rdPartyId` を渡す際に、要求ペイロードがクエリパラメーターか要求ペイロードのいずれかに存在しなかった問題を修正しました。 （TNT-32613）
* Chromium ベースのブラウザー（Google Chrome を含む）で、表示およびクリック通知応答がブロックされていた問題を修正しました。 （TNT-32290）

## at.js バージョン 2.1.0（2019 年 6 月 4 日）

このリリースには、次の機能および機能強化が含まれています。

* **Adobe Opt-In サポート**：Adobe Opt-In は、アドビソリューションと同意管理プラットフォームの統合を簡略化する方法です。 Adobe Opt-in について詳しくは、[プライバシーと一般データ保護規則（GDPR）](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)を参照してください。

* **業界標準の CSP 準拠**：at.js は、eval() を使用して JavaScript を実行しなくなりました。

* **クライアントサイド分析ログ**：クライアントサイドまたはサーバーサイドのいずれでも、顧客が[!DNL Adobe Analytics]に分析データを送信する方法を完全に制御できるようにします。

  詳しくは、[&#x200B; クライアントサイド  [!DNL Analytics]  ログ &#x200B;](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/before-implement.html?lang=ja#client-side)を参照してください。

* **通知の送信**：`applyOffer()` または `applyOffers()` を使用する代わりにコードでエクスペリエンスがレンダリングされる場合、開発者は通知を送信できます。

  詳しくは、[adobe.target.sendNotifications(options)](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md) を参照してください。

* **at.js のサイズが最大 24％減少**：at.js のサイズが最大 24％小さくなります。 ファイルサイズが小さくなることで、ページ読み込みパフォーマンスが向上し、ページへの at.js ダウンロード時間が短縮します。

## at.js バージョン 2.0.1（2019 年 3 月 19 日）

これはメンテナンスリリースで、次の機能強化および修正が含まれています。

（括弧内の問題番号はアドビ社内で使用されます。）

* 特定の顧客に JavaScript の例外が発生する、DOM ポーリングコードの競合状態を解消しました。 （TNT-31869）
* 表示される通知が、クリック追跡イベントハンドラーから切り離されていた問題を修正しました。 最初は、レンダリングされたビューに属するクリックイベントハンドラーを添付できなかった場合、[!DNL Target]は通知を送信しませんでした。 クリック要素が見つからない場合でも、[!DNL Target]がビュー通知を送信するようになりました。 （TNT-31969）
* リクエストに成功したイベントのリダイレクトフラグが常に true に設定されていた問題を修正しました。 （TNT-31907）
* 要素が見つからない場合でも VEC の並べ替えアクションが成功として記録される問題を修正しました。 （TNT-31924）
* 特定の顧客への通知に Enterprise 権限のプロパティトークンが含まれていない問題を修正しました。 （TNT-31999）

## at.js バージョン 1.7.1（2019 年 3 月 19日）

これはメンテナンスリリースで、次の修正が含まれています。

（括弧内の問題番号はアドビ社内で使用されます。）

* 特定の顧客に JavaScript の例外が発生する、DOM ポーリングコードの競合状態を解消しました。 （TNT-31869）

## at.js バージョン 2.0.0

at.js 2.x は、次世代のクライアントサイドテクノロジーでパーソナライゼーションを実行するための機能セットを提供します。 この新しいバージョンは、シングルページアプリケーション（SPA）と調和したインタラクションを実現するための at.js のアップグレードに焦点を当てています。

以前のバージョンでは利用できない、at.js 2.x を使用するメリットを紹介します。

* ページ読み込み時にすべてのオファーをキャッシュして、単一のサーバーコールに対する複数のサーバー呼び出しを減らす機能。
* 従来のサーバー呼び出しで発生する遅延時間なしで、キャッシュ経由でオファーが即座に表示されるため、サイトでのエンドユーザーのエクスペリエンスが著しく向上します。
* 単純な 1 行のコードと一度限りの開発者セットアップで、マーケターは、単一ページアプリケーション上の Visual Experience Composer（VEC）を介して A/B およびエクスペリエンス（XT）アクティビティを作成して実行できます。

at.js 2.x では、次の新しい関数が導入されています。

* getOffers（）
* applyOffers（）
* triggerView（）

at.js 2 x の導入に伴い、次の関数が廃止されました。

* mboxCreate()
* mboxDefine
* registerExtension()

詳しくは、「[at.js 1.x から at.js 2 へのアップグレード](/help/dev/implement/client-side/atjs/upgrading-from-atjs-1x-to-atjs-20.md)」と「[at.js 関数](/help/dev/implement/client-side/atjs/atjs-functions/atjs-functions.md)」を参照してください。

>[!NOTE]
>
>[General Data Protection Regulation](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) （GDPR）に対してAdobe オプトインのサポートが必要な場合は、現在at.js 1.7.0またはat.js 2.1.0以降を使用する必要があります。

## at.js バージョン 1.7.0

at.js 1.7.0 では、Adobe Opt-in サポートが導入されています。 Adobe Opt-In は、アドビソリューションと同意管理プラットフォームの統合を簡略化する方法です。

Adobe Opt-in に関する詳細については、「[プライバシーと一般データ保護規則](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)（GDPR）」を参照してください。

このリリースでは、[!DNL Target]がリダイレクト URLから取得したパラメーターでリダイレクト URL パラメーターを上書きする可能性がある問題も修正されています。

>[!NOTE]
>
>GDPRにAdobe オプトインサポートが必要な場合は、現在at.js 1.7.0またはat.js 2.1.0以降を使用する必要があります。

## at.js バージョン 1.6.4

at.js 1.6.4 は メンテナンスリリースで、次の問題に対応しています。

* Microsoft Internet Explorer 11 で、重複するオファーが適用される競合条件のマニフェストを修正しました。

## at.js バージョン 1.6.3

at.js バージョン 1.6.3 には、次の修正および機能強化が含まれています。

* 先頭が数字、2 つのハイフン、またはハイフンの後に数字が続く（例えば #-123）ID や CSS クラスがセレクターに含まれる場合、セレクターは CSS エスケープされるようになりました。 （TNT-31061）
* at.js 1.6.2 で発生した、同じ CSS セレクターに適用される異なるアクティビティから Visual Experience Composer（VEC）オファーによってアクティビティの優先度が考慮されない問題を修正しました。 （TNT-31052）
* プロミスのネイティブサポートがない環境で、プロミスのタイムアウトに関する問題を修正しました。 （TNT-30974）
* コンテンツレンダリングに失敗したイベントを使用して、問題が正しくキャプチャおよびレポートされるようになりました。 以前は、JavaScript が大文字と小文字が異なる場合でも、正常に実行されていた可能性がありました。 （TNT-30599）

## at.js バージョン 1.6.2

これはメンテナンスリリースで、次の問題に対処しています。

* 一部のお客様サイトで「非同期」ループが発生する問題を修正しました。

>[!WARNING]
>
>また、at.js バージョン 1.6.2には、at.js バージョン 1.6.1および1.6.0に含まれるすべての機能強化と修正も含まれています。 これらのバージョンはダウンロードできなくなりました。 1.6.1 または 1.6.0 を使用している場合は、バージョン 1.6.2 にアップグレードすることをお勧めします。

at.js バージョン 1.6.1 に含まれている機能強化と修正を以下に示します。

* Microsoft Internet Explorer 11 でレコメンデーションエクスペリエンスが重複する原因となっていた at.js 1.6.0 の問題を修正しました。 （TNT-30593）
* at.js のエッジオーバーライドロジックでエッジクラスター Cookie の存在がチェックされるようになりました。これにより、ユーザーがセッション中にエッジ間を移動した場合に異なるエッジ番号が使用される問題を回避できます。 （TNT-30563）
* HTML コンテンツに無効な JS コードが含まれている場合に at.js が後続のアクションを実行できなかった問題を修正しました。 これにより、at.js でエラーが記録され、残りのアクションが問題なくレンダリングされるようになりました。 （TNT-30546）
* リダイレクトページがリダイレクトアクティビティの条件を再度満たしたときに例外をスローするように変更を加えました。 （TNT-30532）
* getOffer() API リクエストから正しいリクエストタイムアウトが伝達されていなかった問題を修正しました。 （TNT-30498）
* ファイルプロトコルが使用されている場合に at.js 1.6.0 が Cookie を保存できなかった問題を修正しました。 （TNT-30454）
* [!DNL Analytics for Target] （A4T）を使用する際に、すべてのエクスペリエンスがリダイレクトで配信されていないように見える問題を修正しました。 （TNT-30444）
* [!DNL Target]呼び出しが成功した後にページが非表示になる問題を修正しました。 （TNT-30358）

at.js バージョン 1.6.0 に含まれている機能強化と修正を以下に示します。

* リダイレクトオファーは、[!UICONTROL Analytics for Target] （A4T）統合で自動的にサポートされるようになりました。 クライアント側の回避策は削除されました。 （TNT-30247）
* クライアント側のエッジルーティングがデフォルトで有効になりました。 （TNT-30261）
* アクション間に依存関係がある場合に Visual Experience Composer（VEC）でアクションをレンダリングするときに発生していた問題を修正しました。 （TNT-30248）

## at.js バージョン 1.5.0

at.js バージョン 1.5.0 がリリースされました。

* `at-request-succeeded` イベントの詳細には、リダイレクトフラグが含まれています。 このフラグを使用すると、ページが別の URL にリダイレクトされるかどうかを判断することができます。 その URL を知る必要がある場合は、subscribe to `at-content-rendering-redirect` をサブスクライブします。 （TNT-29834）
* `window.targetGlobalSettings.enabled` を false に設定すると失敗して実行時例外が発生する原因となっていた問題を修正しました。 （TNT-29829）
* グローバル mbox リクエストを発行したり本文を非表示にしたりすると、Visual Experience Composer（VEC）への読み込み中にページが失敗する原因となっていた問題を修正しました。 （TNT-29795）
* `screenOrientation`、`devicePixelRatio`、および `webGLRenderer` のサポートを追加しました。 これらの新しい[!DNL Target] リクエストパラメーターは、iPhone Xおよびその他の最新のデバイス検出に使用されます。 詳しくは、[モバイル](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/mobile.html?lang=ja)を参照してください。 （TNT-29781）
* Adobe Audience Manager（AAM）のロケーションヒントが送信されないことがある問題を修正しました。 （TNT-29695）
* これをサポートしているブラウザーの場合、at.js 1.5.0 はセレクターポーリングの際に MutationObserver に切り替わります。 at.js 1.0.0 以前のバージョンでは、MutationObserver ポリフィルを使用していましたが、これは問題があることがわかっていました。 ポリフィルの問題を回避するために、バージョン 1.5.0 では次の擬似コードを使用して、どのスケジューリングメカニズムを使用するかを決定しています。

  ```
  if MutationObserver is supported
    scheduler = MutationObserver
  else if document is visible
    scheduler = requestAnimationFrame
  else
    scheduler = setTimeout
  ```

## at.js バージョン 1.3.0

at.js バージョン 1.3.0 がリリースされました。

* at.js とのインタラクションの追跡、デバッグ、カスタマイズに役立つ次の新しいイベントを利用できます。

   * LIBRARY_LOADED
   * REQUEST_START
   * CONTENT_RENDERING_START
   * CONTENT_RENDERING_NO_OFFERS
   * CONTENT_RENDERING_REDIRECT

  詳細については、「[at.js カスタムイベント](/help/dev/implement/client-side/atjs/atjs-functions/atjs-custom-events.md)」を参照してください。

* データプロバイダーから取得した追加パラメーターを利用して at.js リクエストを拡張できます。 データプロバイダーは、`dataProviders key` の `window.targetGlobalSettings` に追加する必要があります。

  詳細については、「[データプロバイダー](atjs-functions/targetglobalsettings.md#data-providers)」を参照してください。

* at.js リクエストで GET が使用されるようになりましたが、URL が 2,048 文字を超える場合は POST に切り替わります。 `urlSizeLimit` という名前の新しいプロパティを利用して、この文字数の上限を引き上げることができます。 この変更により、[!DNL Target]は同じ手法を使用するAppMeasurementにat.jsを整列させることができます。
* [!DNL Target]では、`adobe.target.applyOffer(options)`関数の`mbox` キーが使用されるようになりました。 このキーは以前は必要でしたが、[!DNL Target]では、[!DNL Target]が適切な検証を行い、顧客が関数を正しく使用していることを確認するために、その使用が適用されるようになりました。
* at.js のイベントとクリック追跡機能が強化されました。 at.js では、`navigator.sendBeacon()` を使用してイベント追跡データを送信し、`navigator.sendBeacon()` がサポートされていない場合は同期 XHR にフォールバックします。 このフォールバックが影響するのは、主に Internet Explorer 10 および 11 と、Safari の一部のバージョンです。 Safari では、今後の iOS 11.3 のリリースで `navigator.sendBeacon()` のサポートが追加されます。
* at.js で、バックグラウンドタブでページが開かれている場合でもオファーをレンダリングできるようになりました。 一部の[!DNL Target]のお客様で、背景タブのブラウザーのスロットル動作が原因で`requestAnimationFrame()`が無効になったときに問題が発生しました。
* このリリースで、Chrome の CPU プロファイルを検査する際のコールスタックの短縮など、パフォーマンスの改良が多数加えられています。
* at.js 1.3.0 では、Microsoft Internet Explorer 9 でのコンテンツ配信がサポート対象外になりました。 サポートされているブラウザーの一覧については、[サポートされているブラウザー](/help/dev/before-implement/supported-browsers.md)を参照してください。 今後、すべてのリクエストは、JSONP リクエストを使用せず、CORS に対応した `XMLHttpRequest` を介して実行されます。 この変更によってセキュリティが大幅に高まります。

## at.js バージョン 1.2.3

at.js バージョン 1.2.3 がリリースされました。

* JSON オファーのサポートを追加しました。 JSON オファーは、フォームベースの Experience Composer を使用して作成されたアクティビティでのみ利用できます。 現時点で JSON オファーを使用できる方法は、直接の API 呼び出しのみとなっています。 「[JSON オファーの作成](https://experienceleague.adobe.com/docs/target/using/experiences/offers/create-json-offer.html?lang=ja)」を参照してください。

## at.js バージョン 1.2.2

at.js バージョン 1.2.2 がリリースされました。

* [!DNL Target] ライブラリがQUIRKS モードを使用してページに読み込まれたときにJavaScript エラーが返される問題を修正しました。 （TNT-28312）
* [!DNL Target] クリックの追跡で[!DNL Analytics] データ収集の呼び出しが解除される問題を修正しました。 （TNT-28261）
* `targetPageParams()` が空の文字列を返した場合に `getOffer() params` が失敗する問題を修正しました。 （TNT-28359）
* 「x のみ」を使用している場合にセッション ID の生成で発生する問題を修正しました。 （TNT-28361）

## at.js バージョン 1.2.1

at.js バージョン 1.2.1 がリリースされました。

* target=&quot;_blank&quot;を含むリンクをクリックして追跡すると、[!DNL Target]が新しいタブでリンクを開くことができない問題を修正しました。

## at.js バージョン 1.2.0

at.js バージョン 1.2は、主にバグ修正を含むメンテナンスリリースとして使用できるようになりました。

* クリック追跡の特殊なケースでのデフォルトのアクションを妨げていた問題を修正しました。 （TNT-28089）
* [!DNL Target]が新しいタブでリンクを開くことができなかった`target="_blank"`のリンクをクリック追跡する際の問題を修正しました。 （TNT-28072）
* IP アドレスを Cookie ドメインとして使用できます。 （TNT-28002）
* グローバル mbox またはその他のリージョナル mbox を含むリダイレクトオファーでちらつきが生じる問題を修正しました。 （TNT-27978）
* 参照と作成を切り替える際にVEC内で[!UICONTROL Experience Targeting] アクティビティ設定が失敗する問題を修正しました。 （TNT-27942）
* クリック追跡要素の flicker スタイルクラスの処理に関する問題を修正しました。 （TNT-27896）
* グローバル mbox パラメーターがすべての mbox パラメーターと混同される問題を修正しました。 （TNT-27846）
* Handlebars、Mustache、その他のクライアントサイドのテンプレートライブラリがat.jsで適切に処理されるように変更しました。 （TNT-27831）
* `sdidParamExpiry` が適切に初期化され、訪問者 API に渡されるよう変更しました。 これは、`at.js 1.1.0` に追加された回帰です。 以前のat.js バージョンは影響を受けません。 リダイレクトオファーと A4T を使用するクライアントのみに影響します。 （TNT-27791）
* どの type 属性を使用していても `SCRIPT` が実行されるよう変更しました。 （TNT-27865）

## at.js バージョン 1.1.0

**日付：** 2017 年 8 月 3 日

at.js バージョン 1.1には、次の機能強化と修正が含まれています。

* レスポンストークンの処理を追加しました。 詳しくは、[レスポンストークン](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html?lang=ja)を参照してください。
* 問題を解消し、`document.currentScript polyfill` が Angular 1.X に干渉しないようにしました。
* 変更を加え、クリック追跡が visibility プロパティに干渉しないようにしました。 クリック追跡要素が、`at-element-click-tracking` ではなく `at-element-marker` の CSS クラスに分類されます。

## at.js バージョン 1.0.0

**日付：** 2017 年 7 月 8 日

at.js バージョン 1.0 には、次の機能強化および修正が含まれています。

* ページ読み込みを高速化するために、at.js の非同期での読み込みに対応。
* at.js を非同期で読み込む際のページコンテンツの事前非表示に対応。
* コンテンツ配信が無効にされたときのエラーメッセージを改善。
* 複数のアクティビティを配信する際のパフォーマンスを向上。
* YUI Compressor に対応。
* アクティビティ配信時のカスタムイベントのバグ／エラーレポート。
* Microsoft Internet Explorer 11 のパフォーマンスの問題を修正。
* 一部の Web サイトで `getOffer()` 関数によりエラーが発生する問題を修正。
* [!DNL Target] ライブラリを非同期で読み込みます。 詳細については、「[at.js に関するよくある質問](/help/dev/implement/client-side/atjs/target-atjs-faq.md)」を参照してください。

## at.js バージョン 0.9.7

**日付：** 2017 年 5 月 23 日

at.js バージョン 0.9.7には、次の機能強化と修正が含まれています。

* Visual Experience Composer（VEC）で、`insertAfter` および `insertBefore` アクションにないアセットキーに関連する問題を修正しました。 これは、ビジュアルオファーからオファーテンプレートへの移行に関連する問題でした。

## at.js バージョン 0.9.6

**日付：** 2017 年 4 月 14 日

at.js バージョン 0.9.6には、次の機能強化と修正が含まれています。

* A4T のリダイレクトオファーのサポート。 at.js バージョン 0.9.6をダウンロードしてインストールした後、[!UICONTROL Adobe Analytics as the Reporting Source for Target] （A4T）を使用するアクティビティでリダイレクトオファーを使用できます。 at.js バージョン 0.9.6に加えて、リダイレクトオファーとA4Tを使用するには、実装で満たす必要がある他の最小要件があります。 詳細および追加の重要な情報については、[リダイレクトオファー - A4T に関する FAQ](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t-faq/a4t-faq-redirect-offers.html?lang=ja) を参照してください。
* at.js 0.9.6より前のバージョンでは、訪問者APIがページに存在し、`visitorApiTimeout`設定がアグレッシブすぎるため、[!DNL Target]では、[!DNL Target] リクエストでMCID データが送信されない場合があります。 その結果、A4T を使用しているときに、[!DNL Analytics] で未関連付けヒットなどの問題が発生することがありました。

  この動作はat.js 0.9.6で変更されました。`visitorApiTimeout`が1 ミリ秒と設定されている場合でも、[!DNL Target]はSDID、トラッキングサーバー、顧客IDのデータを収集し、[!DNL Target] リクエストでそれらを送信しようとします。

* `selectorsPollingTimeout` 設定が追加されました。 詳しくは、[targetGlobalSettings()](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) を参照してください。
* `getOffer()` からの応答の形式が変更されました。 詳しくは、[adobe.target.getOffer(options)](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) を参照してください。
* サポートされていない `<!DOCTYPE>` 宣言のコンソールログが追加されました。
* 複数のデフォルトオファーが1つのmboxに配信されたときに[!DNL Target Classic] プラグインが正しく適用されない問題を修正しました。 （TGT-22664）
* 2文字のトップレベルドメイン（TLD）のcookie設定を改善して、これらのドメイン（test.no、autodrives.caなど）にmbox cookieが正しく設定されるようにしました。
* at.js バージョン 0.9.6で、Cookieを保存する際に使用すべきトップレベルドメインを抽出するアルゴリズムが変更されました。 この変更により、IPを使用するアドレスにCookieを保存できません。 IP アドレスはテスト目的で使用されるケースがほとんどですが、DNS エントリを使用したり、ローカルボックスのホストファイルを変更したりすることで対処できます。
* プロパティが整数値ではなく文字列値の場合の移動および整列操作の処理に関する記述を修正しました。

## at.js バージョン 0.9.4

**日付：** 2017 年 1 月 20 日

* mbox 名にアンパサンド（&amp;）を含む特殊文字を含められるようになりました。

  使用可能な特殊文字の一覧については、[at.js設定](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)を参照してください。

* `secureOnly` 設定が追加され、at.js で HTTPS のみを使用するか、ページのプロトコルによって HTTP と HTTPS との切り替えを許可するかを指定できるようになりました。 この詳細設定のデフォルト値は False で、`targetGlobalSettings` で上書きできます。
* レガシーブラウザーサポートオプションは、at.js バージョン 0.9.3以前で使用できます。 このオプションは、at.js バージョン 0.9.4 で削除されました。

## at.js バージョン 0.9.3

**日付：** 2016 年 10 月 11 日

* at.js 設定でレガシーブラウザーが無効になっている場合に、Microsoft Internet Explorer 11 で mbox 呼び出しを実行します。
* 動的リモートオファーに失敗する場合（例えば、URL が正しくなく、404 エラーが返される場合）に、デフォルトコンテンツがレンダリングされます。
* VEC クリックトラッキングセレクターが DOM で見つからない場合に、要素がすばやく表示されます。

## at.js バージョン 0.9.2

**日付：** 2016 年 9 月 22日

* デバイスグラフオプトアウト機能を有効化または無効化するための `optoutEnabled` 設定が追加されました。 これが `true` に設定されており、訪問者がトラッキングをオプトアウトしている場合は、訪問者のブラウザーで mbox の呼び出しは一切おこなわれません。 Device Graph は現在ベータ版です。 この設定はデフォルトで`false`に設定されていますが、デバイスグラフを使用している場合は`true`に設定する必要があります。
* `CustomEvent` のサポートを通知メカニズムに追加しました。 以前は at.js のイベント通知メカニズムが `document.addEventListener()` () など通常の DOM API では使用できませんでした。 現在は、`document.addEventListener()` を使用し、リクエストイベントやコンテンツレンダリングイベントなどの at.js イベントに登録できます。
* Visual Experience Composer（VEC）で作成されたオファーに関連する問題が修正されました。 このリリース以前は、[!DNL Target]はセレクターを非表示にし、すべてのセレクターが一致した場合にのみ非表示にしていました。 at.js 0.9.2 [!DNL Target]では、セレクターが一致するとすぐに非表示になります。

## at.js バージョン 0.9.1

**日付：** 2016 年 7 月 15 日

* at.jsに、サービス自体のタイムアウトから独立した訪問者ID サービスのタイムアウトを提供します。
* 一部のページではat.jsを、他のページではmbox.js （現在は非推奨）を使用する実装に影響を与えた0.9.0の問題を修正しました。
* アクティビティのレポートソースとして[!DNL Adobe Analytics]を使用する場合、mbox.js バージョン 61 （以降）またはat.js バージョン 0.9.1 （以降）を使用している場合、アクティビティの作成中にトラッキングサーバーを指定する必要はありません。 at.js ライブラリは、トラッキングサーバーの値を自動的に [!DNL Target] へ送信します。 アクティビティの作成時に、「目標と設定」ページの「トラッキングサーバー」フィールドを空のままにすることができます。

## at.js バージョン 0.9.0

**[!DNL Target]リリース：** 16.6.1

**日付：** 2016 年 6 月 24 日

* VEC オファー使用中の白い画面の問題を修正します。 at.jsを使用しているすべてのユーザーは、この新しいバージョンにアップグレードする必要があります。
* 新規 `registerExtension` API

  この新しいAPIにより、開発者はat.jsで使用される特定のjQuery モジュールにアクセスして、ライブラリの拡張機能（別名プラグイン）を開発できます。 この変更に伴い、気をつけるべき事項がいくつかあります。 これは、次の機能を使用しているユーザーのみに影響します。

   * `getSettings()` API は廃止されましたが、`registerExtension()` を使用すると同じ機能が利用できます。
   * `getTracking()` API は廃止されましたが、`registerExtension()` を使用すると同じ機能が利用できます。

   * 既存の拡張（AngularJS 拡張など）は `registerExtension()` の手法を使用するように更新する必要があります。

* 新しいat.js通知API。

  この通知システムの目的は、ページ上でat.jsが何をしているのか、問題がいつあるのかについて、より多くのinsightを提供することです。 VEC でよく見られる問題は、IT がページの変更をリリースすると VEC セレクターが壊れ、テストがコンテンツを正しく配信しなくなることです。 この通知システムの目的は、この配信の問題をページに知らせることで、開発者がその情報にアクセスして [!DNL Adobe Analytics] のようなシステムに渡し、テストが壊れたという通知をビジネスオーナーに送信できるようにすることです。

* 新規 `targetGlobalSettings()` API メソッド

  at.js ライブラリの設定は、[!DNL Target Standard/Premium] UIやREST APIを使用して設定するのではなく、上書きできます。

## at.js バージョン 0.8.0

**日付：** 2016 年 5 月 6 日

at.js ライブラリの最初の正式リリースです。

at.jsは、[!DNL Target]用の新しい実装ライブラリで、一般的なweb実装とシングルページアプリケーションの両方に対応しています。

at.js は、mbox.js に替わる [!DNL Adobe Target] 実装ライブラリです。

at.jsは、web実装のページ読み込み時間の向上、セキュリティの向上、シングルページアプリケーションの実装オプションの改善などの利点があります。

at.js は、target.js に含まれるコンポーネントを含んでいるので、target.js を呼び出す必要がありません。

at.js を実装する際には、以下のことに注意してください。

* Internet Explorer 8 より前のバージョンはサポートされません。
* 非同期実装とは、[!UICONTROL Test&Target to SiteCatalyst] プラグインなどのレガシー統合が機能しない可能性があることを意味します。
* mbox.js オブジェクトとメソッドを参照する[!DNL Target] プラグインはサポートされていません。
* [!DNL Target] に対するすべての呼び出しは XMLHTTPRequest を使用しておこなわれ、コンテンツは JSON を使用して返されます。

