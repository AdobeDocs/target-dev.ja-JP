---
keywords: adobe.target.trackEvent, trackEvent, trackevent, track event, at.js, functions, function, preventDefault, preventdefault, prevent default, adobe.target.trackEvent
description: at.js JavaScript ライブラリの [!UICONTROL adobe.target.trackEvent()] 関数を使用して  [!DNL Adobe Target]  サイトでのクリック数やコンバージョン数などのユーザーアクションをレポートするリクエストを実行します。
title: '[!UICONTROL adobe.target.trackEvent()] 関数の使用方法'
feature: at.js
exl-id: 9a55e4f1-d7f9-47c1-867c-2ce06fb26f9f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '314'
ht-degree: 59%

---

# [!UICONTROL adobe.target.trackEvent(options)]

この関数は、クリックやコンバージョンなどのユーザーアクションを報告するリクエストを実行します。応答でアクティビティを配信することはありません。

そのため、これらのイベント追跡 mbox 呼び出しは、アクティビティで指標を定義するのに使用できます。詳細については、「[成功指標](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html)」および「[コンバージョンの追跡](../how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)」を参照してください。

API の詳細を次に示します。

| キー | タイプ | 必須 | 説明 |
|--- |--- |--- |--- |
| mbox | 文字列 | ○ | mbox 名<P>**注意**：ページで既に実行された mbox 名を使用して [!UICONTROL trackEvent()] 呼び出しが実行された場合、[!UICONTROL trackEvent()] の SDID はリセットされ、ページでの [!DNL Target] 呼び出しとは異なります。 ただし、異なる mbox 名で [!UICONTROL trackEvent()] 呼び出しを実行すると、[!UICONTROL trackEvent()] 呼び出しの SDID がページ上の [!UICONTROL Page Load Request]/[!UICONTROL triggerView()] 呼び出しと一致した状態が維持されます。 |
| selector | 文字列 | × | CSS セレクターは HTML 要素を見つけるために使用されます。イベントリスナーは、見つかった要素に添付されます。 |
| type | 文字列 | × | 登録されたイベントタイプを表します。クリック、マウスダウンなどの HTML の既知のイベントとカスタム HTML イベントの両方が可能です。 |
| preventDefault | ブール値 | × | イベントリスナーコールバックで `[!UICONTROL event.preventDefault()]` を使用するかどうかを示します。デフォルトは false です。<P>**メモ**：サポートされているのは `[!UICONTROL form[submit]]` と `a[click]` だけです。 サポートすべきシナリオの複雑さと量の膨大さにより、その他のシナリオはサポートされません。 |
| params | オブジェクト | × | mbox パラメーター。次の構造を持つキーと値のペアのオブジェクト。<P>`{ "param1": "value1", "param2": "value2"}` |
| timeout | 数値 | × | タイムアウト（ミリ秒）。<P>指定しない場合は、次のデフォルト値が使用されます。<P>`...timeoutInSeconds: 0.15...}` |
| success | 関数 | × | イベントが繰り返されたことを伝えるために使用されるコールバック関数。 |
| error | 関数 | × | イベントを繰り返せなかったことを伝えるために使用されるコールバック関数。 |

## 例

```javascript {line-numbers="true"}
<a href="https://asite.com">click me!</a> 
```

さらに、`trackEvent` を割り当てるための JavaScript コード：

```javascript {line-numbers="true"}
<script> 
$('a').click(function(event){ 
  adobe.target.trackEvent({'mbox':'homePageHero'}) 
}); 
</script> 
```

または

```javascript {line-numbers="true"}
adobe.target.trackEvent({ 
    "mbox": "clicked-cta", 
    "params": { 
        "param1": "value1" 
    } 
});
```

>[!WARNING]
>
>必須フィールドが設定されていない場合、リクエストは実行されず、エラーがスローされます。
