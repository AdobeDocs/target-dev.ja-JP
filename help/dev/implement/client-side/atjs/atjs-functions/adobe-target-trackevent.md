---
keywords: adobe.target.trackEvent, trackEvent, trackevent, track event, at.js，関数， preventDefault, preventdefault, prevent default, adobe.target.trackEvent
description: 以下を使用します。 [!UICONTROL adobe.target.trackEvent()] 関数 [!DNL Adobe Target] at.js JavaScript ライブラリを使用して、サイト上でのクリック数やコンバージョン数などのユーザーのアクションをレポートするリクエストを実行します。
title: 使用方法 [!UICONTROL adobe.target.trackEvent()] 機能？
feature: at.js
exl-id: 9a55e4f1-d7f9-47c1-867c-2ce06fb26f9f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '333'
ht-degree: 59%

---

# [!UICONTROL adobe.target.trackEvent(options)]

この関数は、クリックやコンバージョンなどのユーザーアクションを報告するリクエストを実行します。応答でアクティビティを配信することはありません。

そのため、これらのイベント追跡 mbox 呼び出しは、アクティビティで指標を定義するのに使用できます。詳細については、「[成功指標](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html)」および「[コンバージョンの追跡](../how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)」を参照してください。

API の詳細を次に示します。

| キー | タイプ | 必須 | 説明 |
|--- |--- |--- |--- |
| mbox | 文字列 | ○ | mbox 名<P>**注意**: [!UICONTROL trackEvent()] の呼び出しが、ページで既に実行されている mbox 名で実行される ( [!UICONTROL trackEvent()] がリセットされ、次の値とは異なる [!DNL Target] ページのを呼び出します。 ただし、 [!UICONTROL trackEvent()] 別の mbox 名での呼び出しでは、 [!UICONTROL trackEvent()] 呼び出しの SDID が [!UICONTROL ページ読み込み要求]/[!UICONTROL triggerView()] ページのを呼び出します。 |
| selector | 文字列 | × | CSS セレクターは HTML 要素を見つけるために使用されます。イベントリスナーが、見つけた要素に添付されます  |
| type | 文字列 | × | 登録されたイベントタイプを表します。クリック、マウスダウンなどの HTML の既知のイベントとカスタム HTML イベントの両方が可能です。 |
| preventDefault | ブール値 | × | イベントリスナーコールバックで `[!UICONTROL event.preventDefault()]` を使用するかどうかを示します。デフォルトは false です。<P>**注意**：のみ `[!UICONTROL form[submit]]` および `a[click]` はサポートされています。 サポートすべきシナリオの複雑さと量の膨大さにより、その他のシナリオはサポートされません。 |
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
