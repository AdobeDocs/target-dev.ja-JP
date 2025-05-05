---
keywords: adobe.target.triggerView、triggerView、triggerview、トリガービュー、at.js、関数、function、viewName、viewname、ビュー名、adobe.target.triggerView1
description: シングルページアプリケーション（SPA）で使用する at [!DNL Adobe Target] js JavaScript ライブラリの adobe.target.triggerView （）関数を使用します。 (at.js 2.x)
title: adobe.target.triggerView （）関数の使用方法
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
source-git-commit: fe4e607173c760f782035a10f52936d96e9db300
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# adobe.target.triggerView (viewName, options) - at.js 2.x

この関数は、新しいページが読み込まれるときや、ページ上のコンポーネントが再レンダリングされるときに呼び出すことができます。[!UICONTROL Visual Experience Composer] （VEC）を使用して [!UICONTROL A/B Test] と [!UICONTROL Experience Targeting] （XT）アクティビティを作成するには、シングルページアプリケーション（SPA）に `adobe.target.triggerView()` を実装する必要があります。 `[!UICONTROL adobe.target.triggerView()]` がサイトに実装されていない場合、VEC はSPAに使用できません。 詳細については、「[シングルページアプリケーションの実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)」を参照してください。

>[!NOTE]
>
>この関数は at.js 2 で導入されました。*x* を通じてクロスドメイントラッキングを使用している場合です。この関数は at.js バージョン 1 では使用できません。*x*.

| パラメーター | タイプ | 必須？ | 説明 |
| --- | --- | --- | --- |
| viewName | 文字列 | ○ | ビューを表す文字列型として任意の名前を渡します。このビュー名は、マーケターがアクションを作成して [!UICONTROL A/B Test] アクティビティと [!UICONTROL Experience Targeting] XT アクティビティを実行するために、VEC の [!UICONTROL Modifications] パネルに表示されます。 |
| options | オブジェクト | × |  |
| options > page | ブール値 | × | **TRUE：** ページのデフォルト値は true です。page = true の場合、インプレッション数を増分するために [!DNL Target] のバックエンドに通知が送信されます。<P>通知は、オプション / ページが false に設定されている場合を除き、`[!UICONTROL triggerView]` が呼び出されたときに常にデフォルトで送信されます。<P>**FALSE:** page=false の場合、インプレッション数を増分するための通知は送信されません。 この方法は、オファーを含むページでコンポーネントの再レンダリングのみを行う場合に使用します。<P>**メモ**：オプションとして `{page: false}` を使用して VEC が呼び出される場合、`[!UICONTROL triggerView()]` のカスタムコードオファーは再レンダリングされません。 |

## 例：True

アクティビティのインプレッションやその他の指標を増分するための通知を [!DNL Target] バックエンドに送信する `[!UICONTROL triggerView()]` 呼び出し。

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView")
```

## 例：False

`[!UICONTROL triggerView()]` の呼び出しには、インプレッション数をカウントするための通知が [!DNL Target] バックエンドに送信されません。

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView", {page: false})
```

## 例：`getoffers()` および `applyOffers()` を使用した Promise 連結

`getOffers()` promise が解決されたときに `triggerView()` を実行するには、次の例に示すように、最終ブロックで `triggerView()` を実行することが重要です。 これは、VEC がオーサリングモードで `Views` を検出するために必要です。

```javascript {line-numbers="true"}
adobe.target.getOffers({
    'request': {
        'prefetch': {
            'views': [{
                'parameters': {}
            }]
        }
    }
}).then(function(response) {
    // Apply Offers
    adobe.target.applyOffers({
        response: response
    });
}).catch(function(error) {
    console.log("AT: getOffers failed - Error", error);
}).finally(() => {
    // Trigger View call, assuming pageView is defined elsewhere
    adobe.target.triggerView(pageView, {
        page: true
    });
    console.log('AT: View triggered on : ' + pageView);
});
```

## 例：[!UICONTROL Adobe Visual Editing Helper extension] との `triggerView()` の最適な互換性

[Adobe Visual Editing Helper 拡張機能を使用する場合は、次の点を考慮してください ](https://experienceleague.adobe.com/ja/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank}

[!DNL Googl]e の [!DNL Chrome] 拡張機能の新しい V3 マニフェストポリシーにより、[!UICONTROL Visual Editing Helper extension] は VEC の [!DNL Target] ライブラリを読み込む前に `DOMContentLoaded` イベントを待機する必要があります。 この遅延により、オーサリングライブラリの準備が整う前に web ページで `triggerView()` 呼び出しが実行され、読み込み時にビューが入力されない場合があります。

この問題を軽減するには、ページ `load` イベントのリスナーを使用します。

実装の例を次に示します。

```javascript
function triggerViewIfLoaded() {
    adobe.target.triggerView("homeView");
}

if (document.readyState === "complete") {
    // If the page is already loaded
    triggerViewIfLoaded();
} else {
    // If the page is not yet loaded, set up an event listener
    window.addEventListener("load", triggerViewIfLoaded);
}
```


