---
keywords: adobe.target.triggerView、triggerView、triggerview、トリガービュー、at.js、関数、function、viewName、viewname、ビュー名、adobe.target.triggerView1
description: シングルページアプリケーション（SPA）で使用する at [!DNL Adobe Target] js JavaScript ライブラリの adobe.target.triggerView （）関数を使用します。 （at.js 2.x）
title: adobe.target.triggerView （）関数の使用方法
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '326'
ht-degree: 31%

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
