---
keywords: adobe.target.triggerView, triggerView, triggerview, トリガービュー，at.js，関数，関数，viewName, viewname, ビュー名，adobe.target.triggerView1
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリのadobe.target.triggerView （）関数を使用して、シングルページアプリケーション（SPA）で使用します。 (at.js 2.x)'
title: adobe.target.triggerView （）関数の使用方法を教えてください。
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
TQID: https://experienceleague.adobe.com/pBC1GRKG0mxeaZ1hfaByKv2tu-XScrSJfm7lUw-3yKw
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 4d0e7f9f2887db71229061fa64b2633a84c6d054
workflow-type: tm+mt
source-wordcount: 446
ht-degree: 20%

---

# adobe.target.triggerView (viewName, options) - at.js 2.x

この関数は、新しいページが読み込まれるときや、ページ上のコンポーネントが再レンダリングされるときに呼び出すことができます。 `adobe.target.triggerView()`は、[!UICONTROL Visual Experience Composer] （VEC）を使用して[!UICONTROL A/B テスト ]および[!UICONTROL  エクスペリエンスのターゲット設定] （XT）アクティビティを作成するシングルページアプリケーション （SPA）用に実装する必要があります。 `[!UICONTROL adobe.target.triggerView()]`がサイトに実装されていない場合、VECをSPAに使用することはできません。 詳細については、「[シングルページアプリケーションの実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)」を参照してください。

>[!NOTE]
>
>この関数は、at.js 2.*x*&#x200B;で導入されました。 この関数は、at.js バージョン 1.*x*&#x200B;では使用できません。

| パラメーター | タイプ | 必須？ | 説明 |
| --- | --- | --- | --- |
| viewName | 文字列 | ○ | ビューを表す文字列型として任意の名前を渡します。 このビュー名は、VECの[!UICONTROL 変更] パネルに表示され、マーケターがアクションを作成し、[!UICONTROL A/B テスト ]および[!UICONTROL  エクスペリエンスのターゲット設定] XT アクティビティを実行できるようにします。 |
| options | オブジェクト | × |  |
| options > page | ブール値 | × | **TRUE：** ページのデフォルト値は true です。 page = true の場合、インプレッション数を増分するために [!DNL Target] のバックエンドに通知が送信されます。<P>オプション/ページがfalseに設定されている場合を除き、`[!UICONTROL triggerView]`が呼び出されると、通知が常にデフォルトで送信されます。<P>**FALSE:** page=falseの場合、インプレッション数を増やすための通知は送信されません。 このアプローチは、オファーを含むページ上のコンポーネントのみを再レンダリングする場合に使用します。<P>**注意**: `[!UICONTROL triggerView()]`が`{page: false}`をオプションとして呼び出された場合、VEC内のカスタムコードオファーは再レンダリングされません。 |

## 例：True

アクティビティのインプレッションやその他の指標を増分するための通知を[!DNL Target] バックエンドに送信する`[!UICONTROL triggerView()]`呼び出し。

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView")
```

## 例：False

インプレッション数をカウントするために[!DNL Target] バックエンドに通知を送信しない`[!UICONTROL triggerView()]`呼び出し。

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView", {page: false})
```

## 例：`getoffers()`および`applyOffers()`とのプロミス チェーン

`getOffers()` Promiseが解決されたときに`triggerView()`を実行するには、次の例に示すように、最終ブロックで`triggerView()`を実行することが重要です。 これは、VECがオーサリングモードで`Views`を検出するために必要です。

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

## 例：`triggerView()`と[!UICONTROL Adobe Visual Editing Helper拡張機能]の最適な互換性

[Adobe Visual Editing Helper拡張機能](https://experienceleague.adobe.com/en/docs/target/using/experiences/vec/troubleshoot-composer/visual-editing-helper-extension){target=_blank}を使用する場合は、次の点を考慮してください。

[!DNL Googl]eの[!DNL Chrome]拡張機能に対する新しいV3 マニフェストポリシーにより、[!UICONTROL Visual Editing Helper拡張機能]は`DOMContentLoaded` イベントを待ってから、VECに[!DNL Target] ライブラリを読み込む必要があります。 この遅延により、オーサリングライブラリの準備が整う前にweb ページで`triggerView()`呼び出しが実行され、ビューが読み込み時に入力されない場合があります。

この問題を軽減するには、ページ `load` イベントにリスナーを使用します。

導入の例を次に示します。

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



