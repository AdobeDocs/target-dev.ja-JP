---
keywords: adobe.target.trackEvent, trackEvent, trackevent, track event, at.js, functions, function, preventDefault, preventdefault, prevent default, adobe.target.trackEvent
description: ' [!DNL Adobe Target] at.js JavaScript ライブラリの[!UICONTROL adobe.target.trackEvent （） ]関数を使用して、サイトでのクリックやコンバージョンなどのユーザーアクションを報告するリクエストを実行します。'
title: '[!UICONTROL adobe.target.trackEvent （） ]関数の使用方法を教えてください。'
feature: at.js
exl-id: 9a55e4f1-d7f9-47c1-867c-2ce06fb26f9f
TQID: https://experienceleague.adobe.com/Jib9C5FvmsgIF6CA-0UbdMdnMiXxQCkU2-O3Zys3vrY
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 336
ht-degree: 56%

---

# [!UICONTROL adobe.target.trackEvent （options） ]

この関数は、クリックやコンバージョンなどのユーザーアクションを報告するリクエストを実行します。 応答でアクティビティを配信することはありません。

そのため、これらのイベント追跡 mbox 呼び出しは、アクティビティで指標を定義するのに使用できます。 詳細については、「[成功指標](https://experienceleague.adobe.com/docs/target/using/activities/success-metrics/success-metrics.html)」および「[コンバージョンの追跡](../how-to-deployatjs/implement-target-without-a-tag-manager.md#track-conversions)」を参照してください。

API の詳細を次に示します。

| キー | タイプ | 必須 | 説明 |
|--- |--- |--- |--- |
| mbox | 文字列 | ○ | mbox 名<P>**注意**: [!UICONTROL trackEvent （） ]呼び出しが、既にページ上で実行されているmbox名で実行された場合、[!UICONTROL trackEvent （） ]のSDIDはリセットされ、ページ上の[!DNL Target]呼び出しとは異なります。 ただし、異なるmbox名で[!UICONTROL trackEvent （） ]呼び出しを実行すると、[!UICONTROL trackEvent （） ]呼び出しのSDIDは、ページ上の[!UICONTROL  ページ読み込み要求]/[!UICONTROL triggerView （） ]呼び出しと一致します。 |
| selector | 文字列 | × | CSS セレクターは HTML 要素を見つけるために使用されます。 イベントリスナーは、見つかった要素に添付されます。 |
| type | 文字列 | × | 登録されたイベントタイプを表します。 クリック、マウスダウンなどの HTML の既知のイベントとカスタム HTML イベントの両方が可能です。 |
| preventDefault | ブール値 | × | イベントリスナーコールバックで `[!UICONTROL event.preventDefault()]` を使用するかどうかを示します。 デフォルトは false です。<P>**注**: `[!UICONTROL form[submit]]`と`a[click]`のみがサポートされています。 サポートすべきシナリオの複雑さと量の膨大さにより、その他のシナリオはサポートされません。 |
| params | オブジェクト | × | mbox パラメーター。 次の構造を持つキーと値のペアのオブジェクト。<P>`{ "param1": "value1", "param2": "value2"}` |
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
