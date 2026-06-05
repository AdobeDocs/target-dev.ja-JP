---
title: ' [!DNL Adobe Target] と [!DNL Web SDK] を使用してパーソナライゼーションを行います。'
description: ' [!DNL Adobe Target]を使用して [!DNL Experience Platform Web SDK] でパーソナライズされたコンテンツをレンダリングする方法について説明します。'
feature: AEP Web SDK
exl-id: 31c00779-20a8-4d18-9ee4-0430e5e9a84c
source-git-commit: 925a150c06057f5830a1370eee65b5984f81a72d
workflow-type: tm+mt
source-wordcount: '1560'
ht-degree: 6%

---

# パーソナライゼーションに[!DNL Adobe Target]と[!DNL Web SDK]を使用

[!DNL Adobe Experience Platform] [!DNL Web SDK]は、[!DNL Adobe Target]で管理されているパーソナライズされたエクスペリエンスをweb チャネルに配信してレンダリングできます。 [Visual Experience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html) （VEC）と呼ばれるWYSIWYG エディター、または非ビジュアル インターフェイス [&#x200B; フォームベースのExperience Composer](https://experienceleague.adobe.com/docs/target/using/experiences/form-experience-composer.html)を使用して、アクティビティとパーソナライズされたエクスペリエンスを作成、アクティブ化、配信できます。

>[!IMPORTANT]
>
>at.js 2.xからExperience Platform Web SDK[&#128279;](https://experienceleague.adobe.com/docs/platform-learn/migrate-target-to-websdk/introduction.html?lang=ja)へのTargetの移行チュートリアルで、[!DNL Target]実装を[!DNL Experience Platform Web SDK]に移行する方法について説明します。
>
>[Web SDKを使用したAdobe Experience Cloudの実装](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/overview.html?lang=ja) チュートリアルで、[!DNL Target]を初めて実装する方法について説明します。 [!DNL Target]について詳しくは、「[Experience Platform Web SDKを使用したTargetの設定](https://experienceleague.adobe.com/docs/platform-learn/implement-web-sdk/applications-setup/setup-target.html)」というタイトルのチュートリアルの節を参照してください。

次の機能がテストされ、現在[!DNL Target]でサポートされています。

* [A/B テスト](https://experienceleague.adobe.com/docs/target/using/activities/abtest/test-ab.html)
* [A4T インプレッションとコンバージョンレポート](https://experienceleague.adobe.com/docs/target/using/integrate/a4t/a4t.html)
* [Automated Personalization アクティビティ](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [エクスペリエンスのターゲット設定](https://experienceleague.adobe.com/docs/target/using/activities/automated-personalization/automated-personalization.html)
* [多変量分析テスト（MVT）](https://experienceleague.adobe.com/docs/target/using/activities/multivariate-test/multivariate-testing.html)
* [Recommendations アクティビティ](https://experienceleague.adobe.com/docs/target/using/recommendations/recommendations.html)
* [ネイティブのターゲットインプレッションとコンバージョンレポート](https://experienceleague.adobe.com/docs/target/using/reports/reports.html)
* [VEC サポート](https://experienceleague.adobe.com/docs/target/using/experiences/vec/visual-experience-composer.html)

## [!DNL Web SDK] システム ダイアグラム

次の図は、[!DNL Target]と[!DNL Web SDK] エッジ決定のワークフローを理解するのに役立ちます。

![Experience Platform Web SDKを使用したAdobe Target Edge Decisioningの図](/help/dev/implement/client-side/aep-web-sdk/assets/target-platform-web-sdk-new.png)

| 呼び出し | 詳細 |
| --- | --- |
| 1 | デバイスが[!DNL Web SDK]を読み込みます。 [!DNL Web SDK]は、XDM データ、Datastreams Environment ID、渡されたパラメーター、Customer ID （オプション）を含むリクエストをEdge Networkに送信します。 ページ（またはコンテナ）は事前に非表示になっています。 |
| 2 | Edge Networkは、エッジサービスにリクエストを送信し、訪問者ID、同意、および位置情報やデバイスに適した名前などの訪問者のコンテキスト情報でリクエストを強化します。 |
| 3 | Edge Networkは、訪問者IDと渡されたパラメーターを使用して、エンリッチメントされたパーソナライゼーションリクエストを[!DNL Target] エッジに送信します。 |
| 4 | プロファイルスクリプトが実行され、[!DNL Target]個のプロファイルストレージにフィードされます。 プロファイルストレージは、[!UICONTROL &#x200B; オーディエンスライブラリ &#x200B;]からセグメントを取得します（例えば、[!DNL Adobe Analytics]、[!DNL Adobe Audience Manager]、[!DNL Adobe Experience Platform]から共有されたセグメント）。 |
| 5 | URL リクエストパラメーターとプロファイルデータに基づいて、[!DNL Target]は、現在のページビューと将来のプリフェッチ済みビューに訪問者に表示するアクティビティとエクスペリエンスを決定します。 [!DNL Target]は、これをEdge Networkに送り返します。 |
| 6 | a. Edge Networkは、パーソナライゼーションのレスポンスをページに送り返します。オプションで、追加のパーソナライゼーション用のプロファイル値を含めます。 現在のページ上のパーソナライズされたコンテンツは、デフォルトのコンテンツのちらつきを使用せずに、できるだけ早く表示されます。<br>b. シングルページアプリケーション（SPA）でのユーザーアクションの結果として表示されるビュー用のパーソナライズされたコンテンツはキャッシュされるので、ビューがトリガーされたときに追加のサーバーコールなしで即座に適用できます。 <br>c. Edge Networkは、同意、セッション ID、ID、Cookie チェック、パーソナライゼーションなど、訪問者IDおよびその他の値をCookieに送信します。 |
| 7 | Web SDKは、デバイスからEdge Networkに通知を送信します。 |
| 8 | Edge Networkは[!UICONTROL Analytics for Target] （A4T）の詳細（アクティビティ、エクスペリエンス、コンバージョンメタデータ）を[!DNL Analytics] エッジに転送します。 |

## [!DNL Adobe Target]を有効にしています

[!DNL Target]を有効にするには、次の操作を行います。

1. 適切なクライアントコードを使用して、[&#x200B; データストリーム &#x200B;](https://experienceleague.adobe.com/en/docs/experience-platform/datastreams/overview)の[!DNL Target]を有効にします。
1. イベントに`renderDecisions` オプションを追加します。

次に、オプションで次のオプションを追加することもできます。

* **`decisionScopes`**：このオプションをイベントに追加して、特定のアクティビティ（フォームベースのコンポーザーで作成されたアクティビティに便利）を取得します。
* **[スニペットの事前非表示](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/personalization/manage-flicker)**: ページの特定の部分のみを非表示にします。

## [!UICONTROL Adobe Target] VECの使用

[!DNL Web SDK]実装でVECを使用するには、[Firefox](https://addons.mozilla.org/en-US/firefox/addon/adobe-target-vec-helper/)または[Chrome](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension) VEC Helper Extensionのいずれかをインストールしてアクティベートします。

詳しくは、*Adobe Target ガイド*&#x200B;の[Visual Experience Composer ヘルパー拡張機能](https://experienceleague.adobe.com/docs/target/using/experiences/vec/troubleshoot-composer/vec-helper-browser-extension.html)を参照してください。

## パーソナライズされたコンテンツのレンダリング

詳しくは、[&#x200B; パーソナライゼーションコンテンツのレンダリング &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-platform/web-sdk/personalization/rendering-personalization-content)を参照してください。

## XDMのオーディエンス

[!DNL Web SDK]を介して配信される[!DNL Target] アクティビティのオーディエンスを定義する場合、[XDM](https://experienceleague.adobe.com/docs/experience-platform/xdm/home.html)を定義して使用する必要があります。 XDM スキーマ、クラス、スキーマフィールドグループを定義したら、ターゲット用にXDM データで定義された[!DNL Target] オーディエンスルールを作成できます。 [!DNL Target]内では、XDM データは[!UICONTROL Audience Builder]にカスタムパラメーターとして表示されます。 XDMは、ドット表記法を使用してシリアル化されます（例：`web.webPageDetails.name`）。

カスタムパラメーターまたはユーザープロファイルを使用する、事前定義されたオーディエンスを持つ[!DNL Target]件のアクティビティがある場合、SDKを介して正しく配信されません。 カスタムパラメーターやユーザープロファイルを使用する代わりに、代わりにXDMを使用する必要があります。 ただし、XDMを必要としない[!DNL Web SDK]経由でサポートされている、すぐに使用できるオーディエンスターゲティングフィールドがあります。 これらのフィールドは、XDMを必要としない[!DNL Target] UIで使用できます。

* ターゲットライブラリ
* 地域
* ネットワーク
* オペレーティングシステム
* サイトのページ
* ブラウザー
* トラフィックソース
* 時間枠

詳しくは、*Adobe Target ガイド*&#x200B;の「[&#x200B; オーディエンスのカテゴリ &#x200B;](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/target-rules.html)」を参照してください。

### レスポンストークン

応答トークンは、GoogleやFacebookなどのサードパーティにメタデータを送信するために使用されます。 応答トークンが返されます
`propositions` > `items`の中の`meta` フィールドにあります。

次にサンプルを示します。

```json
{
  "id": "AT:eyJhY3Rpdml0eUlkIjoiMTI2NzM2IiwiZXhwZXJpZW5jZUlkIjoiMCJ9",
  "scope": "__view__",
  "scopeDetails": ...,
  "renderAttempted": true,
  "items": [
    {
      "id": "0",
      "schema": "https://ns.adobe.com/personalization/dom-action",
      "meta": {
        "experience.id": "0",
        "activity.id": "126736",
        "offer.name": "Default Content",
        "offer.id": "0"
      }
    }
  ]
}
```

応答トークンを収集するには、`alloy.sendEvent` promiseを購読し、`propositions`を繰り返し、詳細を`items` -> `meta`から抽出する必要があります。

すべての`proposition`には、`proposition`がレンダリングされたかどうかを示す`renderAttempted` ブール値フィールドがあります。 次のサンプルを参照してください。

```js
alloy("sendEvent",
  {
    "renderDecisions": true,
    "decisionScopes": [
      "hero-container"
    ]
  }).then(result => {
    const { propositions } = result;

    // filter rendered propositions
    const renderedPropositions = propositions.filter(proposition => proposition.renderAttempted === true);

    // collect the item metadata that represents the response tokens
    const collectMetaData = (items) => {
      return items.filter(item => item.meta !== undefined).map(item => item.meta);
    }

    const pageLoadResponseTokens = renderedPropositions
      .map(proposition => collectMetaData(proposition.items))
      .filter(e => e.length > 0)
      .flatMap(e => e);
  });
  
```

自動レンダリングが有効になっている場合、提案の配列には次のものが含まれます。

#### ページ読み込み時：

* `renderAttempted` フラグが`false`に設定された`propositions`に基づくフォームベースのコンポーザー
* `renderAttempted` フラグが`true`に設定されたVisual Experience Composer ベースの提案
* `renderAttempted` フラグが`true`に設定された単一ページアプリケーションビューのVisual Experience Composer ベースの提案

#### 表示時 – 変更（キャッシュされたビューの場合）:

* `renderAttempted` フラグが`true`に設定された単一ページアプリケーションビューのVisual Experience Composer ベースの提案

自動レンダリングが無効になっている場合、提案の配列には次のものが含まれます。

#### ページ読み込み時：

* `renderAttempted` フラグが`false`に設定された[!DNL Form-based Composer] ベースの`propositions`
* `renderAttempted` フラグが`false`に設定された[!DNL Visual Experience Composer] ベースの提案
* `renderAttempted` フラグが`false`に設定された単一ページ アプリケーション ビューの[!DNL Visual Experience Composer] ベースの提案

#### 表示時 – 変更（キャッシュされたビューの場合）:

* `renderAttempted` フラグが`false`に設定された単一ページアプリケーションビューのVisual Experience Composer ベースの提案

### 単一プロファイルの更新

[!DNL Web SDK]を使用すると、プロファイルを[!DNL Target] プロファイルに更新し、エクスペリエンスイベントとして[!DNL Web SDK]に更新できます。

[!DNL Target] プロファイルを更新するには、プロファイルデータが次のように渡されていることを確認します。

* `"data {"`未満
* `"__adobe.target"`未満
* 接頭辞`"profile."`

| キー | タイプ | 説明 |
| --- | --- | --- |
| `renderDecisions` | ブール値 | パーソナライゼーションコンポーネントにDOM アクションを解釈するかどうかを指示します |
| `decisionScopes` | 配列`<String>` | 決定を取得するスコープのリスト |
| `xdm` | オブジェクト | XDMでフォーマットされたデータで、Web SDKにエクスペリエンスイベントとして格納されます |
| `data` | オブジェクト | ターゲット クラスの[!DNL Target]個のソリューションに任意のキー/値のペアが送信されました。 |

<!--Typical [!DNL Web SDK] code using this command looks like the following:-->

**コンテンツがエンドユーザーに表示されるまで、プロファイルまたはエンティティのパラメーターの保存を遅らせる**

コンテンツが表示されるまでプロファイル内の属性の記録を遅らせるには、リクエストに`data.adobe.target._save=false`を設定します。

例えば、web サイトには、web サイト上の3つのカテゴリリンク（男性、女性、子供）に対応する3つの決定範囲が含まれており、最終的にユーザーが訪問したカテゴリを追跡する必要があります。 これらの要求を`__save` フラグを`false`に設定して送信し、コンテンツが要求された時点でカテゴリを保持しないようにします。 コンテンツが視覚化されたら、記録する対応する属性に対して適切なペイロード（`eventToken`と`stateToken`を含む）を送信します。

次の例では、trackEvent スタイルのメッセージを送信し、プロファイルスクリプトを実行して属性を保存し、イベントを即座に記録します。

```js
alloy("sendEvent", {
    "renderDecisions": true,
    "xdm": { /* Experience Event XDM data */ },
    "data": {
        "__adobe": {
            "target": {
                " __save": true|false,
                //defaults to true if omitted
                "profile.gender": "female",
                "profile.age": 30,
                "entity.name": "T-shirt",
                "entity.id": "1234"
            }
        }
    }
})
```

>[!NOTE]
>
>`__save` ディレクティブを省略すると、プロファイル属性とエンティティ属性の保存がすぐに行われます。 `__save` ディレクティブは、プロファイル属性とエンティティの詳細にのみ関連します。

## レコメンデーションを依頼

次の表は、[!DNL Recommendations]属性と、各属性が[!DNL Web SDK]を介してサポートされているかどうかを示しています。

| カテゴリ | 属性 | サポートステータス |
| --- | --- | --- |
| Recommendations - デフォルトのエンティティ属性 | entity.id | 対応 |
|  | entity.name | 対応 |
|  | entity.categoryId | 対応 |
|  | entity.pageUrl | 対応 |
|  | entity.thumbnailUrl | 対応 |
|  | entity.message | 対応 |
|  | entity.value | 対応 |
|  | entity.inventory | 対応 |
|  | entity.brand | 対応 |
|  | entity.margin | 対応 |
|  | entity.event.detailsOnly | 対応 |
| Recommendations - カスタムエンティティ属性 | entity.yourCustomAttributeName | 対応 |
| Recommendations – 予約済みmbox/ページパラメーター | excludedIds | 対応 |
|  | cartIds | 対応 |
|  | productPurchasedId | 対応 |
| カテゴリーの親和性を考慮したページまたはアイテムのカテゴリー | user.categoryId | 対応 |

**Recommendations属性を[!DNL Target]に送信する方法：**

```js
alloy("sendEvent", {
  "renderDecisions": true,
  "data": {
    "__adobe": {
      "target": {
        "entity.id": "123",
        "entity.genre": "Drama"
      }
    }
  }
});
```

## mbox コンバージョン指標の表示 {#display-mbox-conversion-metrics}

次のサンプルは、コンテンツやアクティビティの対象となることなく、表示mboxのコンバージョンを追跡し、プロファイルパラメーターを[!DNL Target]に送信する方法を示しています。

```js
alloy("sendEvent", {
    "xdm": {
        "_experience": {
            "decisioning": {
                "propositions": [{
                    "scope": "conversion-step-1" //example scope name
                }],
                "propositionEventType": {
                    "display": 1
                }
            }
        },
        "eventType": "decisioning.propositionDisplay"
    }
});
```


| プロパティ | 説明 |
|---------|----------|
| `xdm._experience.decisioning.propositions[x].scope` | 成功指標を関連付ける範囲（ターゲット側の特定のアクティビティに関連付ける範囲）。 |
| `xdm._experience.decisioning.propositions[x].eventType` | 意図されたイベントタイプを表す文字列。 この使用例では、これを`"decisioning.propositionDisplay"`に設定します。 |

## デバッグ

mboxTraceおよびmboxDebugは非推奨（廃止予定）となりました。 代わりに、[Web SDK デバッグ &#x200B;](https://experienceleague.adobe.com/en/docs/experience-platform/web-sdk/use-cases/debugging)のメソッドを使用してください。

## 用語

**提案**: [!DNL Target]では、提案はアクティビティから選択されたエクスペリエンスに関連付けられます。

**スキーマ**：決定のスキーマは、[!DNL Target]のオファーのタイプです。

**範囲**：決定の範囲。 [!DNL Target]では、スコープはmboxです。 グローバル mboxは`__view__` スコープです。

**XDM**: XDMはドット表記法にシリアル化され、mbox パラメーターとして[!DNL Target]に入力されます。
