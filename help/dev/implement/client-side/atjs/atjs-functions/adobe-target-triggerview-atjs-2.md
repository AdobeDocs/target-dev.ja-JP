---
keywords: adobe.target.triggerView, triggerView, triggerview,トリガービュー， at.js，関数， viewName, viewname, view name, adobe.target.triggerView1
description: adobe.target.triggerView() 関数を使用すると、 [!DNL Adobe Target] シングルページアプリケーション (SPA) で使用する at.js JavaScript ライブラリ。 (at.js 2.x)
title: adobe.target.triggerView() 関数の使用方法を教えてください。
feature: at.js
exl-id: d6130c56-4e77-4668-ad21-a5b335f8b234
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '338'
ht-degree: 37%

---

# adobe.target.triggerView (viewName, options) - at.js 2.x

この関数は、新しいページが読み込まれるときや、ページ上のコンポーネントが再レンダリングされるときに呼び出すことができます。`adobe.target.triggerView()` を使用するには、シングルページアプリケーション (SPA) にを実装する必要があります。 [!UICONTROL Visual Experience Composer] (VEC) 作成 [!UICONTROL A/B テスト] および [!UICONTROL エクスペリエンスのターゲット設定] (XT) アクティビティ 次の場合 `[!UICONTROL adobe.target.triggerView()]` がサイトに実装されていない場合、VEC はSPAで使用できません。 詳細については、「[シングルページアプリケーションの実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)」を参照してください。

>[!NOTE]
>
>この関数は at.js 2.*x*.この関数は at.js バージョン 1 では使用できません。*x*.

| パラメーター | タイプ | 必須？ | 説明 |
| --- | --- | --- | --- |
| viewName | 文字列 | ○ | ビューを表す文字列型として任意の名前を渡します。このビュー名は、 [!UICONTROL 変更] VEC のパネル（マーケティング担当者がアクションを作成して実行するためのパネル） [!UICONTROL A/B テスト] および [!UICONTROL エクスペリエンスのターゲット設定] XT アクティビティ。 |
| options | オブジェクト | × |  |
| options > page | ブール値 | × | **TRUE：** ページのデフォルト値は true です。page = true の場合、インプレッション数を増分するために [!DNL Target] のバックエンドに通知が送信されます。<P>デフォルトでは、 `[!UICONTROL triggerView]` が呼び出されます。ただし、 options / page が false に設定されている場合は除きます。<P>**FALSE：** page = false の場合、インプレッション数を増分するための通知は送信されません。この方法は、オファーを含むページ上のコンポーネントのみを再レンダリングする場合に使用します。<P>**注意**:VEC のカスタムコードオファーが、 `[!UICONTROL triggerView()]` が次の値で呼び出されます： `{page: false}` を選択します。 |

## 例：True

アクティビティインプレッション数およびその他の指標の値を増加させるために バックエンドに通知を送信するための `[!UICONTROL triggerView()]`[!DNL Target] 呼び出し。

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView")
```

## 例：False

 バックエンドにインプレッションをカウントするための通知を送信しないようにするための `[!UICONTROL triggerView()]`[!DNL Target] 呼び出し。

```javascript {line-numbers="true"}
adobe.target.triggerView("homeView", {page: false})
```

## 例：と連携する Promise `getoffers()` および `applyOffers()`

実行する `triggerView()` 時に `getOffers()` promise が解決された場合、 `triggerView()` 最後のブロックに貼り付けます。 これは、VEC が `Views` （オーサリングモード）

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
