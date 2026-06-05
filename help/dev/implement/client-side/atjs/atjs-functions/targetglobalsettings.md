---
keywords: serverstate, targetGlobalSettings, targetglobalsettings, globalSettings, globalSettings, at.js, functions, function, clientCode, clientcode, serverDomain, serverdomain, cookieDomain, serverstate5, serverstate6, serverstate7, serverstate8, serverstate9, targetGlobalSettings0, targetGlobalSettings1, targetGlobalSettings2, targetGlobalSettings3, targetGlobalSettings4, targetGlobalSettings5, cookiedomain, crossDomain, crossdomain, timeout, timeout globalMboxAutoCreate, visitorApiTimeout, defaultContentHiddenStyle, defaultContentVisibleStyle, bodyHiddenStyle, bodyHidingEnabled, imsOrgId, secureOnly, overrideMboxEdgeServer, overrideMboxEdgeTimeout, cookiedomain5, cookiedomain6, cookiedomain7, cookiedomain8, cookiedomain9, crossDomain0, crossDomain1, crossDomain2, crossDomain3 optoutEnabled, optout, optout, selectorsPollingTimeout, dataProviders, Hybrid Personalization, deviceIdLifetime
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリの[!UICONTROL targetGlobalSettings （） &#x200B;]関数を使用して、 [!DNL Target] UIまたはREST APIを使用する代わりに設定を上書きします。'
title: '[!UICONTROL targetGlobalSettings （） &#x200B;]関数の使用方法を教えてください。'
feature: at.js
exl-id: f6218313-6a70-448e-8555-b7b039e64b2c
TQID: https://experienceleague.adobe.com/6IeQo7RCys6Qe6bPydmmtgaAERi7rnneBYFOzseaL2g
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: a004cc84-67b9-4a33-a3a7-8ec7273ef4dc
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 2705
ht-degree: 56%

---

# [!UICONTROL targetGlobalSettings （） &#x200B;]

at.js ライブラリの設定は、[!DNL Target] UIやREST APIで設定するのではなく、`[!UICONTROL targetGlobalSettings()]`を使用して上書きできます。

## 設定

次の設定を上書きできます。

### aepSandboxId

* **型**：String
* **デフォルト値**: null
* **説明**: デフォルト以外のサンドボックスで作成された[!DNL Adobe Experience Platform]宛先を[!DNL Target]と共有するために、[!DNL Adobe Experience Platform] サンドボックス IDを送信するために使用されるオプションのパラメーターです。 `aepSandboxId`がnull以外の場合は、`aepSandboxName`も指定する必要があります。

### aepSandboxName

* **型**：String
* **デフォルト値**: null
* **説明**: デフォルト以外のサンドボックスで作成された[!DNL Adobe Experience Platform]宛先を[!DNL Target]と共有するために、[!DNL Adobe Experience Platform] サンドボックス名を送信するために使用されるオプションのパラメーターです。 `aepSandboxName`がnull以外の場合は、`aepSandboxId`も指定する必要があります。

### artifactLocation

* **型**：String
* **デフォルト値**：なし
* **説明**: [&#x200B; デバイス上の決定ルール アーティファクト &#x200B;](../../../server-side/sdk-guides/on-device-decisioning/rule-artifact-overview.md)への完全修飾URL

### bodyHiddenStyle

* **型**：String
* **デフォルト値**：body { opacity:0 }
* **説明**：`globalMboxAutocreate === true` の場合にのみ使用して、ちらつきの発生を最小限に抑えます。

  詳しくは、「[at.js によるちらつきの制御方法](/help/dev/implement/client-side/atjs/how-atjs-works/manage-flicker-with-atjs.md)」を参照してください。

### bodyHidingEnabled

* **タイプ**：ブール値
* **デフォルト値**：true
* **説明**：Visual Experience Composer で作成されたオファー（）の配信に `target-global-mbox` が使用される場合に、ちらつきの制御に使用します。

### clientCode

* **型**：String
* **デフォルト値**：UI で設定された値。
* **説明**：クライアントコードを表します。

### cookieDomain

* **型**：String
* **デフォルト値**：可能であれば、トップレベルドメインに設定します。
* **説明**：Cookie の保存時に使用するドメインを表します。

### crossDomain

* **型**：String
* **デフォルト値**：UI で設定された値。
* **説明**：クロスドメイントラッキングが有効になっているかどうかを表します。 許可される値は、at.js バージョンによって異なります。 at.js v1.*x*&#x200B;の場合、`enabled` （ブラウザーが1st パーティ Cookieと3rd パーティ Cookieの両方を設定）を選択して、クロスドメイン機能が`disabled` （ブラウザーがドメイン内のCookieを設定）、`x only` （ブラウザーが[!DNL Target]のドメイン内のCookieを設定）、またはその両方かどうかを指定します。 at.js v2.10以降の場合、クロスドメイン機能が`enabled` （ブラウザーがファーストパーティ Cookieとサードパーティ Cookieの両方を設定）または`disabled` （ブラウザーがサードパーティ Cookieを設定しない）のどちらかを指定します。

### cspScriptNonce

* **タイプ**：以下の[コンテンツセキュリティポリシー](#content-security-policy)を参照してください。
* **デフォルト値**：以下の[コンテンツセキュリティポリシー](#content-security-policy)を参照してください。
* **説明**：以下の[コンテンツセキュリティポリシー](#content-security-policy)を参照してください。

### cspStyleNonce

* **タイプ**：以下の[コンテンツセキュリティポリシー](#content-security-policy)を参照してください。
* **デフォルト値**：以下の[コンテンツセキュリティポリシー](#content-security-policy)を参照してください。
* **説明**：以下の[コンテンツセキュリティポリシー](#content-security-policy)を参照してください。

### dataProviders

* **タイプ**：以下の[データプロバイダー](#data-providers)を参照してください。
* **デフォルト値**：以下の[データプロバイダー](#data-providers)を参照してください。
* **説明**：以下の[データプロバイダー](#data-providers)を参照してください。

### decisioningMethod

* **型**：String
* **デフォルト値**：server-side
* **その他の値**：on-device、hybrid
* **説明**：以下の「判定方法」を参照してください。

  **判定方法**

  オンデバイス判定では、[!DNL Target]は、at.jsがエクスペリエンスをどのように配信するかを決定するDecisioning Methodと呼ばれる新しい設定を導入します。 `decisioningMethod` には、server-side（サーバー側のみ）、on-device（オンデバイスのみ）、hybrid（ハイブリッド）の 3 つの値があります。 `decisioningMethod` が `targetGlobalSettings()` で設定されると、[!DNL Target] のあらゆる決定のデフォルトの判定方法として機能します。

  **サーバー側のみ**：

  at.js 2.5以降をweb プロパティに実装してデプロイする場合に、デフォルトの決定メソッドはサーバーサイドのみになります。

  デフォルト設定としてサーバーサイドのみを使用すると、サーバーコールをブロックする[!DNL Target] エッジネットワークですべての決定が行われます。 このアプローチでは、遅延が増加する可能性がありますが、[Recommendations](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html?lang=ja)、[Automated Personalization](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html?lang=ja) （AP）、[自動ターゲット &#x200B;](https://experienceleague.adobe.com/docs/target/using/activities/auto-target/auto-target-to-optimize.html?lang=ja) アクティビティを含む[!DNL Target]のマシンラーニング機能を適用できる機能など、大きなメリットも得られます。

  さらに、セッションやチャネルをまたいで保持される[!DNL Target]のユーザープロファイルを使用して、パーソナライズされたエクスペリエンスを強化することで、ビジネスに強力な成果をもたらすことができます。

  さらに、サーバーサイドでは、Adobe Experience Cloudを使用して、Audience ManagerやAdobe Analyticsのセグメントを通じてターゲティングできるオーディエンスを調整することしかできません。

  **オンデバイスのみ**：

  オンデバイス判定は、web ページ全体でのみ使用する必要がある場合にat.js 2.5以降で設定する必要がある決定方法です。

  オンデバイス判定では、オンデバイス判定の対象となるすべてのアクティビティを含んだ、キャッシュされたルールアーティファクトから決定が行われるので、エクスペリエンスとパーソナライゼーションアクティビティを超高速で配信できます。

  どのアクティビティがオンデバイス判定の対象となるかについて詳しくは、サポートされている機能の節を参照してください。

  この判定方法は、[!DNL Target] からの決定を必要とするすべてのページでパフォーマンスが非常に重要な場合にのみ使用してください。 さらに、この判定方法を選択した場合、オンデバイス判定の対象とならない [!DNL Target] アクティビティは配信も実行もされないことに注意してください。 at.js ライブラリ 2.5 以降は、キャッシュされたルールアーティファクトのみを探して決定を下すように設定されています。

  **ハイブリッド**：

  ハイブリッドは、[!DNL Adobe Target] Edge ネットワークへのネットワーク呼び出しを必要とするデバイス上の決定とアクティビティの両方を実行する必要がある場合に、at.js 2.5以降で設定する必要がある決定方式です。

  オンデバイス判定アクティビティとサーバー側アクティビティの両方を管理している場合は、ページに [!DNL Target] をデプロイしてプロビジョニングする方法を考える際に、少し複雑で面倒な場合があります。 ハイブリッドを決定メソッドとして使用すると、[!DNL Target]は、サーバーサイドの実行を必要とするアクティビティに対して[!DNL Adobe Target] Edge ネットワークへのサーバーコールを実行する必要があるタイミングと、オンデバイスの意思決定のみを実行するタイミングを把握できます。

  JSON ルールアーティファクトには、mbox がサーバー側アクティビティを実行しているか、オンデバイス判定アクティビティを実行しているかを at.js に通知するメタデータが含まれています。 この決定方法は、迅速に配信する予定のアクティビティがデバイス上の決定を通じて実行されることを保証し、より強力なマシンラーニング主導のパーソナライゼーションを必要とするアクティビティの場合は、[!DNL Adobe Target] Edge ネットワークを介して実行されます。

### defaultContentHiddenStyle

* **型**：String
* **デフォルト値**：visibility: hidden
* **説明**：クラス名が「mboxDefault」である DIV を使用し、`mboxCreate()`、`mboxUpdate()` または `mboxDefine()` から実行される mbox のラッピングにのみ使用されて、デフォルトコンテンツを非表示にします。

### defaultContentVisibleStyle

* **型**：String
* **デフォルト値**：visibility: visible
* **Description**：クラス名が「mboxDefault」である DIV を使用し、`mboxCreate()`、`mboxUpdate()` または `mboxDefine()` から実行される mbox のラッピングにのみ使用されて、適用されたオファー（存在する場合）またはデフォルトコンテンツを表示します。

### deviceIdLifetime

* **タイプ**：数値
* **デフォルト値**：63244800000 ミリ秒 = 2 年
* **説明**：`deviceId` が Cookie に保持される時間。

>[!NOTE]
>
>deviceIdLifetime 設定は、at.js バージョン 2.3.1 以降で上書きできます。

### 有効

* **タイプ**：ブール値
* **デフォルト値**：true
* **説明**：有効にすると、エクスペリエンスおよびエクスペリエンスをレンダリングする DOM 操作を取得する [!DNL Target] リクエストが自動的に実行されます。 さらに、[!DNL Target] 呼び出しは、`getOffer(s)`／`applyOffer(s)` を介して手動で実行できます。

  無効にした場合、[!DNL Target] リクエストは自動でも手動でも実行されません。

### globalMboxAutoCreate

* **タイプ**：数値
* **デフォルト値**：UI で設定された値。
* **説明**：グローバル mbox リクエストをトリガーするかどうかを示します。

### imsOrgId

* **型**：String
* **デフォルト値**：true
* **説明**：IMS ORG ID を表します。

### optinEnabled

* **タイプ**：ブール値
* **デフォルト値**：false
* **説明**: [!DNL Target]は、同意管理戦略をサポートするために、Adobe Experience Platformを介したオプトイン機能のサポートを提供しています。 オプトイン機能を使用すると、[!DNL Target] タグを実行する方法とタイミングを制御できます。 Adobe Experience Platformを介して、[!DNL Target] タグを事前承認するオプションもあります。 [!DNL Target] の at.js ライブラリでオプトインを使用する機能を有効にするには、`optinEnabled=true` 設定を追加する必要があります。 Adobe Experience Platformでは、拡張機能のインストールビューのGDPR オプトインドロップダウンリストから「有効」を選択する必要があります。 詳しくは、[Adobe Experience Platform ドキュメント &#x200B;](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)を参照してください。 欧州連合（EU）の一般データ保護規則（GDPR）やカリフォルニア州消費者プライバシー法（CCPA）など、プライバシーとデータ保護に関する規制に関連するこの設定について詳しくは、[&#x200B; プライバシーとデータ保護に関する規制](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)を参照してください。

### optoutEnabled

* **タイプ**：ブール値
* **デフォルト値**：false
* **説明**: [!DNL Target]がVisitor API `isOptedOut()`関数を呼び出す必要があるかどうかを示します。 これは、デバイスグラフ有効化の一部です。

### overrideMboxEdgeServer

* **タイプ**：ブール値
* **デフォルト値**：true（at.js バージョン 1.6.2 以降）
* **説明**：`<clientCode>.tt.omtrdc.net`ドメインまたは `mboxedge<clusterNumber>.tt.omtrdc.net` ドメインを使用する必要があるかどうかを示します。

  この値が true の場合、`mboxedge<clusterNumber>.tt.omtrdc.net` ドメインは Cookie に保存されます。 at.js 1.8.2およびat.js 2.3.1より前のバージョンのat.jsを使用している場合、現在[CNAME](/help/dev/before-implement/implement-cname-support-in-target.md)を使用できません。 これが問題になる場合は、[at.js](/help/dev/implement/client-side/atjs/target-atjs-versions.md)を新しいサポートされているバージョンに更新することを検討してください。

### overrideMboxEdgeServerTimeout

* **タイプ**：数値
* **デフォルト値**：1860000 => 31 分
* **説明**：`mboxedge<clusterNumber>.tt.omtrdc.net` 値を含んだ Cookie の有効期間を示します。

### pageLoadEnabled

* **タイプ**：ブール値
* **デフォルト値**：true
* **説明**：有効にすると、ページ読み込み時に返す必要のあるエクスペリエンスが自動的に取得されます。

### pollingInterval

* **タイプ**：数値
* **デフォルト値**: 300000 （5分（ミリ秒単位））
* **説明**: at.jsがオンデバイス判定アーティファクトの新しいバージョンを取得し、キャッシュを更新する間隔。 300000は、`pollingInterval`に許可されている最小値です。

### secureOnly

* **タイプ**：ブール値
* **デフォルト値**：false
* **説明**：at.js で HTTPS のみを使用するか、ページのプロトコルに基づいて HTTP と HTTPS を切り替えることができるかを示します。 true に設定した場合、secureOnly は Secure 属性と SameSite 属性も mbox Cookie に設定します。

### selectorsPollingTimeout

* **タイプ**：数値
* **デフォルト値**：5000 ミリ秒 = 5 秒
* **説明**：at.js 0.9.6 では、`targetGlobalSettings` で上書きできるこの新しい設定が [!DNL Target] に導入されました。

  `selectorsPollingTimeout` 設定は、セレクターで識別されたすべての要素がページに表示されるまでクライアントが待機できる時間を表します。

  Visual Experience Composer（VEC）によって作成されたアクティビティには、セレクターが含まれたオファーがあります。

### serverDomain

* **型**：String
* **デフォルト値**：UI で設定された値。
* **説明**: [!DNL Target] エッジ サーバーを表します。

### serverState

* **タイプ**：以下の[ハイブリッドパーソナライゼーション](#hybrid-personalization)を参照してください。
* **デフォルト値**：以下の[ハイブリッドパーソナライゼーション](#hybrid-personalization)を参照してください。
* **説明**：以下の[ハイブリッドパーソナライゼーション](#hybrid-personalization)を参照してください。

### telemetryEnabled

* **タイプ**：ブール値
* **デフォルト値**：true
* **説明**：有効にすると、AdobeはSDK機能の使用状況およびパフォーマンス テレメトリ データを収集します。 個人データは収集されません。

### timeout

* **タイプ**：数値
* **デフォルト値**：UI で設定された値。
* **説明**：[!DNL Target] エッジリクエストのタイムアウトを表します。

### viewsEnabled {#viewsenabled}

* **タイプ**：ブール値
* **デフォルト値**：true
* **説明**：有効にすると、ページの読み込み時にビューが自動的に取得されます。 `triggerView`が呼び出されると、該当するビューがブラウザーに表示されます。 このオプションを無効にすると、ページ読み込み時にビューが取得されず、`triggerView`は何も実行しません。 ビューは、at.js 2.*x*&#x200B;でのみサポートされています。

### visitorApiTimeout

* **タイプ**：数値
* **デフォルト値**：2000 ms = 2 秒
* **説明**：訪問者API リクエストのタイムアウトを表します。

## 使用方法

この関数は、at.js が読み込まれる前か、**管理**／**実装**／**at.js 設定を編集**／**コード設定**／**ライブラリヘッダー**&#x200B;で定義できます。

「ライブラリのヘッダー」フィールドでは、JavaScript を自由形式で入力できます。 カスタマイズコードは次の例のようになります。

```javascript {line-numbers="true"}
window.targetGlobalSettings = {
   timeout: 200, // using custom timeout
   visitorApiTimeout: 500, // using custom API timeout
   enabled: document.location.href.indexOf('https://www.adobe.com') >= 0 // enabled ONLY on adobe.com
};
```

## データプロバイダー {#data-providers}

この設定を使用すると、顧客はDemandbase、BlueKai、カスタム サービスなどのサードパーティのデータ プロバイダーからデータを収集し、グローバル mbox リクエストのmbox パラメーターとして[!DNL Target]にデータを渡すことができます。 非同期および同期リクエストを介した複数のプロバイダーからのデータ収集もサポートしています。 この手法では、デフォルトのページコンテンツのちらつきを制御しながら、プロバイダーごとに個別のタイムアウトを指定し、ページのパフォーマンスへの影響を抑制することが簡単にできます。

>[!NOTE]
>
>データプロバイダーには、at.js 1.3以降が必要です。

詳細は次のビデオで説明されています。

| ビデオ | 説明 |
|--- |--- |
| [Adobe Target でのデータプロバイダーの使用](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html?lang=ja) | データプロバイダーは、サードパーティから Target へデータを簡単に渡すことのできる機能です。 サードパーティとしては、気象予報サービス、DMP、自社の Web サービスなども利用可能です。 このデータを利用して、オーディエンスやターゲットコンテンツを構築したり、訪問者プロファイルを充実させることができます。 |
| [Adobe Target でのデータプロバイダーの実装](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html?lang=ja) | 実装の詳細と、Adobe [!DNL Target]のdataProviders機能を使用してサードパーティのデータプロバイダーからデータを取得し、[!DNL Target] リクエストに渡す方法の例。 |

`window.targetGlobalSettings.dataProviders` 設定は、データプロバイダーの配列です。

各データプロバイダーの構造は次のとおりです。

| キー | タイプ | 説明 |
|--- |--- |--- |
| name | 文字列 | プロバイダーの名前。 |
| version | 文字列 | プロバイダーのバージョン。 このキーはプロバイダーの展開に使用されます。 |
| timeout | 数値 | ネットワークリクエストの場合は、プロバイダーのタイムアウトを表します。  このキーはオプションです。 |
| provider | 関数 | プロバイダーデータの取得ロジックを含む関数。<p>関数には、1つの必須パラメーターがあります：`callback`。 この callback パラメーターは、データを適切に取得した場合またはエラーが発生した場合にのみ呼び出す必要があります。<p>このコールバックは、次の 2 つのパラメーターを受け取ります。<ul><li>error：エラーの有無を表します。 エラーがない場合、このパラメーターは null に設定されます。</li><li>params: [!DNL Target] リクエストで送信されるパラメーターを表すJSON オブジェクト。</li></ul> |

データプロバイダーで同期実行を使用する場合の例は次のとおりです。

```javascript {line-numbers="true"}
var syncDataProvider = {
  name: "simpleDataProvider",
  version: "1.0.0",
  provider: function(callback) {
    callback(null, {t1: 1});
  }
};

window.targetGlobalSettings = {
  dataProviders: [
    syncDataProvider
  ]
};
```

at.jsが`window.targetGlobalSettings.dataProviders`を処理した後、[!DNL Target] リクエストには新しいパラメーター`t1=1`が含まれます。

次に、[!DNL Target] リクエストに追加するパラメーターが、BluekaiやDemandbaseなどのサードパーティサービスから取得される場合の例を示します。

```javascript {line-numbers="true"}
var blueKaiDataProvider = {
   name: "blueKai",
   version: "1.0.0",
   provider: function(callback) {
      // simulating network request
     setTimeout(function() {
       callback(null, {t1: 1, t2: 2, t3: 3});
     }, 1000);
   }
}

window.targetGlobalSettings = {
   dataProviders: [
      blueKaiDataProvider
   ]
};
```

at.jsが`window.targetGlobalSettings.dataProviders`を処理した後、[!DNL Target] リクエストには追加のパラメーターが含まれます：`t1=1`、`t2=2`、および`t3=3`。

次の例では、データプロバイダーを使用して気象API データを収集し、[!DNL Target] リクエストのパラメーターとして送信します。 [!DNL Target] リクエストには、`country`や`weatherCondition`など、追加のパラメーターが含まれます。

```javascript {line-numbers="true"}
var weatherProvider = {
      name: "weather-api",
      version: "1.0.0",
      timeout: 2000,
      provider: function(callback) {
        var API_KEY = "caa84fc6f5dc77b6372d2570458b8699";
        var lat = 44.426767399999996;
        var lon = 26.1025384;
        var url = "//api.openweathermap.org/data/2.5/weather?";
        var data = {
          lat: lat,
          lon: lon,
          appId: API_KEY
        }

        $.ajax({
          type: "GET",
                url: url,
          dataType: "json",
          data: data,
          success: function(data) {
            console.log("Weather data", data);
            callback(null, {
              country: data.sys.country,
              weatherCondition: data.weather[0].main
            });
          },
          error: function(err) {
            console.log("Error", err);
            callback(err);
          }
        });
      }
    };

    window.targetGlobalSettings = {
      dataProviders: [weatherProvider]
    };
```

`dataProviders` 設定を使用する際は、次の点に注意してください。

* `window.targetGlobalSettings.dataProviders` に追加されたデータプロバイダーが非同期の場合は、同時並行で実行されます。 訪問者 API リクエストは、待ち時間を最小限に抑えるために、`window.targetGlobalSettings.dataProviders` に追加された関数と同時に実行されます。
* at.js では、データはキャッシュされません。 データプロバイダーが 1 回だけデータを取得する場合は、データをキャッシュし、そのプロバイダーの関数が呼び出されたら、2 回目の呼び出しでキャッシュデータを配信できるようにする必要があります。

## コンテンツセキュリティポリシー

at.js 2.3.0以降では、配信された[!DNL Target]件のオファーを適用する際に、ページ DOMに追加されるSCRIPT タグとSTYLE タグに対してコンテンツセキュリティポリシーのノンスを設定することがサポートされています。

at.js 2.3.0 以降の読み込みに先立って、SCRIPT と STYLE の Nonce を `targetGlobalSettings.cspScriptNonce` と `targetGlobalSettings.cspStyleNonce` で設定する必要があります。 以下の例を参照してください。

```javascript {line-numbers="true"}
...
<head>
 <script nonce="<script_nonce_value>">
window.targetGlobalSettings = {
  cspScriptNonce: "<csp_script_nonce_value>",
  cspStyleNonce: "<csp_style_nonce_value>"
};
 </script>
 <script nonce="<script_nonce_value>" src="at.js"></script>
...
</head>
...
```

`cspScriptNonce`と`cspStyleNonce`の設定を指定した後、at.js 2.3.0以降では、これらを[!DNL Target]のオファーを適用する際にDOMに追加するすべてのSCRIPT タグとSTYLE タグにnonce属性として設定します。

## ハイブリッドパーソナライゼーション

`serverState`は、at.js v2.2以降で使用できる設定で、[!DNL Target]のハイブリッド統合が実装されたときにページパフォーマンスを最適化するために使用できます。 ハイブリッド統合とは、クライアントサイドでat.js v2.2+とDelivery APIの両方を使用しているか、またはサーバーサイドで[!DNL Target] SDKを使用してエクスペリエンスを配信していることを意味します。 `serverState` には、at.js v2.2 以降で、サーバーサイドで取得したコンテンツからエクスペリエンスを直接適用し、提供されるページの一部としてクライアントに返す機能が備わっています。

### 前提条件

[!DNL Target] のハイブリッド統合が必要です。

* **サーバーサイド**: [配信API](/help/dev/implement/delivery-api/overview.md)または[Target SDK](/help/dev/implement/server-side/sdk-guides/getting-started/getting-started.md)を使用する必要があります。
* **クライアント側**：[at.js バージョン 2.2 以降](/help/dev/implement/client-side/atjs/target-atjs-versions.md)を使用する必要があります。

### コードサンプル

この仕組みをより深く理解するには、サーバー上にある以下のコード例を参照してください。 このコードは、[Target Node.js SDK](https://github.com/adobe/target-nodejs-sdk) の使用を前提としています。

```javascript {line-numbers="true"}
// First, we fetch the offers via Target Node.js SDK API, as usual
const targetResponse = await targetClient.getOffers(options);
// A successfull response will contain Target Delivery API request and response objects, which we need to set as serverState
const serverState = {
  request: targetResponse.request,
  response: targetResponse.response
};
// Finally, we should set window.targetGlobalSettings.serverState in the returned page, by replacing it in a page template, for example
const PAGE_TEMPLATE = `
<!doctype html>
<html>
<head>
  ...
  <script>
    window.targetGlobalSettings = {
      overrideMboxEdgeServer: true,
      serverState: ${JSON.stringify(serverState, null, " ")}
    };
  </script>
  <script src="at.js"></script>
</head>
...
</html>
`;
// Return PAGE_TEMPLATE to the client ...
```

ビュー事前読み込みの `serverState` オブジェクト JSON の例を次に示します。

```javascript {line-numbers="true"}
{
 "request": {
  "requestId": "076ace1cd3624048bae1ced1f9e0c536",
  "id": {
   "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "context": {
   "channel": "web",
   "timeOffsetInMinutes": 0
  },
  "experienceCloud": {
   "analytics": {
    "logging": "server_side",
    "supplementalDataId": "7D3AA246CC99FD7F-1B3DD2E75595498E"
   }
  },
  "prefetch": {
   "views": [
    {
     "address": {
      "url": "my.testsite.com/"
     }
    }
   ]
  }
 },
 "response": {
  "status": 200,
  "requestId": "076ace1cd3624048bae1ced1f9e0c536",
  "id": {
   "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "client": "testclient",
  "edgeHost": "mboxedge21.tt.omtrdc.net",
  "prefetch": {
   "views": [
    {
     "name": "home",
     "key": "home",
     "options": [
      {
       "type": "actions",
       "content": [
        {
         "type": "setHtml",
         "selector": "#app > DIV.app-container:eq(0) > DIV.page-container:eq(0) > DIV:nth-of-type(2) > SECTION.section:eq(0) > DIV.container:eq(1) > DIV.heading:eq(0) > H1.title:eq(0)",
         "cssSelector": "#app > DIV:nth-of-type(1) > DIV:nth-of-type(1) > DIV:nth-of-type(2) > SECTION:nth-of-type(1) > DIV:nth-of-type(2) > DIV:nth-of-type(1) > H1:nth-of-type(1)",
         "content": "<span style=\"color:#FF0000;\">Latest</span> Products for 2020"
        }
       ],
       "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
       "responseTokens": {
        "profile.memberlevel": "0",
        "geo.city": "dublin",
        "activity.id": "302740",
        "experience.name": "Experience B",
        "geo.country": "ireland"
       }
      }
     ],
     "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
    }
   ]
  }
 }
}
```

ページがブラウザーに読み込まれた後、at.js は [!DNL Target] エッジに対するネットワーク呼び出しを実行せずに、`serverState` からのすべての [!DNL Target] オファーを直ちに適用します。 さらに、at.js は、コンテンツが取得されたサーバー側で [!DNL Target] オファーが使用可能な DOM 要素のみを事前に非表示にするので、ページ読み込みのパフォーマンスとエンドユーザーエクスペリエンスに好影響を及ぼします。

### 重要事項

`serverState` を使用する場合は、以下の事項を考慮してください。

* 現時点で、at.js v2.2 は、以下に対してのみ serverState を介したエクスペリエンスの配信をサポートしています。

   * ページの読み込み時に実行される、VEC で作成されたアクティビティ。
   * 事前読み込みされたビュー。

     at.js APIで[!DNL Target] ビューと`triggerView()`を使用するSPAの場合、at.js v2.2はサーバーサイドでプリフェッチされたすべてのビューのコンテンツをキャッシュし、各ビューが`triggerView()`を介してトリガーされるとすぐに適用します。[!DNL Target]への追加のコンテンツフェッチ呼び出しを実行する必要はありません。

   * **注意**：現在、サーバー側で取得された mbox は、`serverState` ではサポートされていません。

* `serverState`件のオファーを適用する場合、at.jsは`pageLoadEnabled`および`viewsEnabled`件の設定を考慮します。例えば、`pageLoadEnabled`件の設定がfalseの場合、ページ読み込みオファーは適用されません。

  これらの設定を有効にするには、**管理/実装/編集/ページ読み込み有効**&#x200B;で切り替えを有効にします。

  ![「ページ読み込みが有効」設定](../../assets/page-load-enabled-setting.png)

* `serverState` を使用し、返されるコンテンツで `<script>` タグを使用する場合は、HTML コンテンツで `</script>` の代わりに `<\/script>` が使用されていることを確認してください。 `</script>` を使用すると、ブラウザーが `</script>` をインライン SCRIPT の終わりと解釈して、HTML ページが破損する可能性があります。

### その他のリソース

`serverState` の仕組みについて詳しくは、次のリソースを参照してください。

* [サンプルコード](https://github.com/Adobe-Marketing-Cloud/target-node-client-samples/tree/master/advanced-atjs-integration-serverstate).
* [`serverState` を使用した単一ページアプリケーション（SPA）サンプルアプリケーション](https://github.com/Adobe-Marketing-Cloud/target-node-client-samples/tree/master/react-shopping-cart-demo)。
