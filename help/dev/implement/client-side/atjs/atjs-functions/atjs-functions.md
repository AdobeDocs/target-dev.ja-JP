---
keywords: at.js，関数，javascript ライブラリ
description: at.js JavaScript ライブラリの1.xおよび2.x バージョンで使用できる関数のリストを [!DNL Adobe Target]で表示します。
title: at.jsではどのような関数を使用できますか？
feature: at.js
exl-id: 1efed365-8a74-4c85-bdb1-8daaaf53d642
TQID: https://experienceleague.adobe.com/7uABK1rDaMpA7a0skEo3g1KxTnoc-gif-uHMkMnE8QE
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: a1af9d2c909d9b3d506dd4875d1bd75149dbf636
workflow-type: tm+mt
source-wordcount: 557
ht-degree: 40%

---

# at.js 関数

[!DNL Adobe Target] at.js JavaScript ライブラリで使用できる関数のリスト。 詳細および例については、「関数」列のリンクをクリックしてください。

| 関数 | 詳細 |
| --- | --- |
| [[!UICONTROL adobe.target.getOffer （options） ]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) | この関数は、[!DNL Target] オファーを取得するためのリクエストを起動します。 `adobe.target.applyOffer()` と併用して、応答を処理するか、独自の成功処理を使用します。 |
| [[!UICONTROL adobe.target.getOffers （options） ]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)<P>(at.js 2.x) | この関数を使用すると、複数の mbox を渡すことで複数のオファーを取得できます。 さらに、アクティブなアクティビティのすべてのビュー向けに複数のオファーを取得できます。<P>**注：**&#x200B;この関数はat.js 2.xで導入されました。 この関数は、at.js バージョン 1.*x*&#x200B;では使用できません。 |
| [[!UICONTROL adobe.target.applyOffer （options） ]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md) | この関数は、応答内容を適用するために使用します。 |
| [[!UICONTROL adobe.target.applyOffers （options） ]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)<P>(at.js 2.x) | この関数を使用すると、[!UICONTROL adobe.target.getOffers （） ]によって取得された複数のオファーを適用できます。<P>**注：**&#x200B;この関数はat.js 2.xで導入されました。 この関数は、at.js バージョン 1.*x*&#x200B;では使用できません。 |
| [[!UICONTROL adobe.target.triggerView （viewName, options） ]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)<P>(at.js 2.x) | この関数は、新しいページが読み込まれるときや、ページ上のコンポーネントが再レンダリングされるときに呼び出すことができます。<P> この関数は、[!UICONTROL Visual Experience Composer] （VEC）を使用して[!UICONTROL A/B テスト ]および[!UICONTROL Experience Targeting] （XT）アクティビティを作成するために、シングルページアプリケーション （SPA）に対して実装する必要があります。 |
| [[!UICONTROL adobe.target.trackEvent （options） ]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) | この関数は、クリックやコンバージョンなどのユーザーアクションを報告するリクエストを実行します。 応答でアクティビティを配信することはありません。 |
| [[!UICONTROL mboxCreate （mbox,params） ]](/help/dev/implement/client-side/atjs/atjs-functions/mboxcreate-atjs.md)<P>（at.js 1.x） | リクエストを実行し、mboxDefault クラス名を持つ最も近い DIV にオファーを適用します。<P>**注意：**&#x200B;この関数は、at.js バージョン 1.*x*&#x200B;でのみ使用できます。 この関数は、at.js 2.xのリリースで廃止されました。 この関数は、at.js 2.xで使用した場合、デフォルトコンテンツを返します。 |
| [[!UICONTROL mboxDefine （options） ]および[!UICONTROL mboxUpdate （options） ]](/help/dev/implement/client-side/atjs/atjs-functions/mboxdefine-mboxupdate-atjs-1x.md)<P>（at.js 1.x） | mbox を定義および更新します。<P>**注意：**&#x200B;この関数は、at.js バージョン 1.*x*&#x200B;でのみ使用できます。 この関数は、at.js 2.xのリリースで廃止されました。 この関数は、at.js 2.xで使用した場合、デフォルトコンテンツを返します。 |
| [[!UICONTROL targetGlobalSettings （options） ]](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) | at.js ライブラリの設定は、[!DNL Target Standard/Premium] UIやREST APIで設定するのではなく、`[!UICONTROL targetGlobalSettings()]`を使用して上書きできます。<ul><li>データプロバイダー：この設定では、Demandbase、BlueKai、カスタムサービスなどのサードパーティのデータプロバイダーからデータを収集し、そのデータをグローバル mbox リクエストで mbox パラメーターとして Target に渡すことができます。</li></ul> |
| [[!UICONTROL targetPageParams （options） ]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) | このメソッドにより、リクエストコードの外部からグローバル mbox にパラメーターを付加できます。 |
| [[!UICONTROL targetPageParamsAll （options） ]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparamsall.md) | このメソッドにより、リクエストコードの外部からすべての mbox にパラメーターを付加できます。 |
| [[!UICONTROL registerExtension （options） ]](/help/dev/implement/client-side/atjs/atjs-functions/registerextension-atjs-1x.md)<P>（at.js 1.x） | 特定の拡張を登録するための標準的な方法を提供します。<P>**注意：**&#x200B;この関数は、at.js バージョン 1.*x*&#x200B;でのみ使用できます。 この関数は、at.js 2.xのリリースで廃止されました。 この関数は、at.js 2.xで使用した場合、デフォルトコンテンツを返します。 |
| [[!UICONTROL at.js カスタムイベント ]](/help/dev/implement/client-side/atjs/atjs-functions/atjs-custom-events.md) | at.js カスタムイベントを使用すると、mbox リクエストまたはオファーが失敗または成功した場合に通知できます。 |
| [[!UICONTROL adobe.target.sendNotifications （options） ]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md)<P>（at.js 2.1.0） | `[!UICONTROL adobe.target.applyOffer()]`または`[!UICONTROL adobe.target.applyOffers()]`を使用せずにエクスペリエンスをレンダリングすると、この関数は[!DNL Target] エッジに通知を送信します。<P>**注意**：この関数はat.js 2.1.0で導入され、2.1.0以降のバージョンで使用できます。 |



