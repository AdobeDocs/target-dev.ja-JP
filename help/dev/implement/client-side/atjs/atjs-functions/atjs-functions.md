---
keywords: at.js，関数， javascript ライブラリ
description: の at.js JavaScript ライブラリの 1.x および 2.x バージョンで使用できる関数のリストを表示します。 [!DNL Adobe Target].
title: at.js で使用できる関数
feature: at.js
exl-id: 1efed365-8a74-4c85-bdb1-8daaaf53d642
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '531'
ht-degree: 73%

---

# at.js 関数

と共に使用できる関数のリスト [!DNL Adobe Target] at.js JavaScript ライブラリ。 詳細および例については、「関数」列のリンクをクリックしてください。

| 関数 | 詳細 |
| --- | --- | 
| [[!UICONTROL adobe.target.getOffer(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffer.md) | この関数は、 [!DNL Target] オファー。 `adobe.target.applyOffer()` と併用して、応答を処理するか、独自の成功処理を使用します。 |
| [[!UICONTROL adobe.target.getOffers(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-getoffers-atjs-2.md)<P>(at.js 2.x) | この関数を使用すると、複数の mbox を渡すことで複数のオファーを取得できます。さらに、アクティブなアクティビティのすべてのビュー向けに複数のオファーを取得できます。<P>**注：**&#x200B;この関数は at.js. 2.x で導入されました。この関数は at.js バージョン 1.x では使用できません。*x*. |
| [[!UICONTROL adobe.target.applyOffer(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffer.md) | この関数は、応答内容を適用するために使用します。 |
| [[!UICONTROL adobe.target.applyOffers(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-applyoffers-atjs-2.md)<P>(at.js 2.x) | この関数を使用すると、 [!UICONTROL adobe.target.getOffers()].<P>**注：**&#x200B;この関数は at.js. 2.x で導入されました。この関数は at.js バージョン 1.x では使用できません。**。 |
| [[!UICONTROL adobe.target.triggerView (viewName, options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-triggerview-atjs-2.md)<P>(at.js 2.x) | この関数は、新しいページが読み込まれるときや、ページ上のコンポーネントが再レンダリングされるときに呼び出すことができます。<P> この関数は、 [!UICONTROL Visual Experience Composer] (VEC) 作成 [!UICONTROL A/B テスト] および [!UICONTROL エクスペリエンスのターゲット設定] (XT) アクティビティ |
| [[!UICONTROL adobe.target.trackEvent(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-trackevent.md) | この関数は、クリックやコンバージョンなどのユーザーアクションを報告するリクエストを実行します。応答でアクティビティを配信することはありません。 |
| [[!UICONTROL mboxCreate(mbox,params)]](/help/dev/implement/client-side/atjs/atjs-functions/mboxcreate-atjs.md)<P>(at.js 1.x) | リクエストを実行し、mboxDefault クラス名を持つ最も近い DIV にオファーを適用します。<P>**注：**&#x200B;この関数は at.js バージョン 1.*x* のみで使用できます。この関数は at.js 2.x のリリースで廃止されました。at.js 2.x で使用する場合、この関数はデフォルトコンテンツを返します。 |
| [[!UICONTROL mboxDefine(options)] および [!UICONTROL mboxUpdate(options)]](/help/dev/implement/client-side/atjs/atjs-functions/mboxdefine-mboxupdate-atjs-1x.md)<P>(at.js 1.x) | mbox を定義および更新します。<P>**注：**&#x200B;この関数は at.js バージョン 1.*x* のみで使用できます。この関数は at.js 2.x のリリースで廃止されました。at.js 2.x で使用する場合、この関数はデフォルトコンテンツを返します。 |
| [[!UICONTROL targetGlobalSettings(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md) | at.js ライブラリの設定は、 `[!UICONTROL targetGlobalSettings()]`を設定する代わりに、 [!DNL Target Standard/Premium] UI または REST API を使用します。<ul><li>データプロバイダー：この設定では、Demandbase、BlueKai、カスタムサービスなどのサードパーティのデータプロバイダーからデータを収集し、そのデータをグローバル mbox リクエストで mbox パラメーターとして Target に渡すことができます。</li></ul> |
| [[!UICONTROL targetPageParams(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparams.md) | このメソッドにより、リクエストコードの外部からグローバル mbox にパラメーターを付加できます。 |
| [[!UICONTROL targetPageParamsAll(options)]](/help/dev/implement/client-side/atjs/atjs-functions/targetpageparamsall.md) | このメソッドにより、リクエストコードの外部からすべての mbox にパラメーターを付加できます。 |
| [[!UICONTROL registerExtension(options)]](/help/dev/implement/client-side/atjs/atjs-functions/registerextension-atjs-1x.md)<P>(at.js 1.x) | 特定の拡張を登録するための標準的な方法を提供します。<P>**注：**&#x200B;この関数は at.js バージョン 1.*x* のみで使用できます。この関数は at.js 2.x のリリースで廃止されました。at.js 2.x で使用する場合、この関数はデフォルトコンテンツを返します。 |
| [[!UICONTROL at.js カスタムイベント]](/help/dev/implement/client-side/atjs/atjs-functions/atjs-custom-events.md) | at.js カスタムイベントを使用すると、mbox リクエストまたはオファーが失敗または成功した場合に通知できます。 |
| [[!UICONTROL adobe.target.sendNotifications(options)]](/help/dev/implement/client-side/atjs/atjs-functions/adobe-target-sendnotifications-atjs-21.md)<P>(at.js 2.1.0) | この関数は、に通知を送信します。 [!DNL Target] エッジ（を使用せずにエクスペリエンスがレンダリングされる場合） `[!UICONTROL adobe.target.applyOffer()]` または `[!UICONTROL adobe.target.applyOffers()]`.<P>**注意**：この関数は、at.js 2.1.0 で導入され、2.1.0 以上の任意のバージョンで使用できます。 |
