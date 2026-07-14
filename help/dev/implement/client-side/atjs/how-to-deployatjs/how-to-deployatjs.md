---
keywords: 実装, at.js, JavaScript ライブラリ
description: タグを使用して [!DNL Adobe Target]  at.js JavaScript ライブラリをデプロイする方法（ [!DNL Adobe Experience Platform] またはタグマネージャーを使用しない場合）について説明します。
title: at.jsのデプロイ方法
feature: Implement Server-side
exl-id: e62cb27e-ea80-462b-90f8-0a033b128031
TQID: https://experienceleague.adobe.com/V80R3Ds7eaUkkJazzCLK-tIePgqund6rMfQfLBZZvRQ
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ceid: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2: id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: bce87dde-a4ab-44c9-8a18-ad66e4ddb377id: d3cdead0-685a-4489-9250-4bb709942f66
source-git-commit: ca4254966a337a0215d66bd28506128b9751d0e0
workflow-type: tm+mt
source-wordcount: 287
ht-degree: 27%

---

# at.js のデプロイ方法

[!DNL Adobe Target] JavaScript ライブラリ at.jsを、[!DNL Adobe Experience Platform]のタグを使用するか、タグマネージャーを使用せずにデプロイする方法に関する情報。

次の方法を使用して at.js をデプロイできます。

* **[Adobe Experience Platformでタグを使用して [!DNL Target] 実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)**:[!DNL Adobe Experience Platform]のタグは、Adobeの次世代型タグ管理機能です。 タグは、適切な顧客体験の実現に必要な分析、マーケティングおよび広告タグをデプロイおよび管理するためのシンプルな手段を提供します。

>[!NOTE]
>
> [!DNL Adobe Experience Platform Launch] は、[!DNL Adobe Experience Platform] のデータ収集テクノロジーのスイートとしてリブランドされました。 その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。 用語の変更に関する参照の一覧については、次の[ドキュメント ](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html)を参照してください。

* **[タグマネージャーを使用せずに [!DNL Target] 実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)**: タグマネージャーを使用せずに[!DNL Target]を実装できます（例：[!DNL Adobe Experience Platform]のタグ）。
* **サードパーティのタグマネージャーを使用して[!DNL Target]を実装する**: [Adobe Experience Platformのタグ ](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)は、[!DNL Target]を実装する際に推奨される方法です。ただし、Tealium、Ensighten、Google タグなどのサードパーティのタグマネージャーを使用して[!DNL Target]を実装することもできます。 Launchを使用する利点の一覧については、[拡張機能 [!DNL Adobe Target] を使用したat.js実装の利点](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md#advantages-of-implementing-atjs-using-the-target-extension)を参照してください。

  ただし、タグマネージャーを使用せずに[!DNL Target]を実装する方法を知っている場合は、サイトコードでat.jsをハードコーディングする代わりに、サードパーティのタグマネージャーを使用して簡単に実装できます。

  サードパーティのタグマネージャーを使用して[!DNL Target]を実装するのに役立つ2つの関連トピックを次に示します。

   * [実装する前に](/help/dev/before-implement/prepare-to-implement-target.md)
   * [タグマネージャーなしで [!DNL Target] 実装する](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

  詳しくは、サードパーティのタグマネージャーに関するドキュメントを確認してください。

シングルページアプリケーション （SPA）を使用する場合に[!DNL Target]を実装するには、[ シングルページアプリケーションの実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md)を参照してください。



