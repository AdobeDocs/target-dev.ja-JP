---
keywords: 実装, at.js, JavaScript ライブラリ
description: をデプロイする方法を説明します。 [!DNL Adobe Target]  タグを使用した at.js JavaScript ライブラリ [!DNL Adobe Experience Platform] またはタグマネージャーがない場合は、
title: at.js のデプロイ方法を教えてください。
feature: Implement Server-side
exl-id: e62cb27e-ea80-462b-90f8-0a033b128031
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '283'
ht-degree: 27%

---

# at.js のデプロイ方法

のデプロイ方法に関する情報 [!DNL Adobe Target]  JavaScript ライブラリ、at.js、タグを使用 [!DNL Adobe Experience Platform] またはタグマネージャーがない場合は、

次の方法を使用して at.js をデプロイできます。

* **[実装方法 [!DNL Target] Adobe Experience Platformでのタグの使用](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)**：タグ ( [!DNL Adobe Experience Platform] は、Adobeが提供する次世代タグ管理機能です。 タグは、適切な顧客体験の実現に必要な分析、マーケティングおよび広告タグをデプロイおよび管理するためのシンプルな手段を提供します。

>[!NOTE]
>
> [!DNL Adobe Experience Platform Launch] は、[!DNL Adobe Experience Platform] のデータ収集テクノロジーのスイートとしてリブランドされました。その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更に関する参照の一覧については、次の[ドキュメント ](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html)を参照してください。

* **[実装方法 [!DNL Target] （タグマネージャーなし）](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)**：を実装できます。 [!DNL Target] タグマネージャーを使用しない ( 例えば、 [!DNL Adobe Experience Platform]) をクリックします。
* **実装方法 [!DNL Target] サードパーティのタグマネージャーの使用**: [Adobe Experience Platformのタグ](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) は、 [!DNL Target]ただし、 [!DNL Target] Tealium、Ensighten、Google Tag などのサードパーティのタグマネージャーを使用する。 Launch を使用する利点の一覧については、 [を使用して at.js を実装するメリット [!DNL Adobe Target]  拡張](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md#advantages-of-implementing-atjs-using-the-target-extension).

  ただし、 [!DNL Target] タグマネージャーがないと、サイトコード内で at.js をハードコーディングする代わりに、サードパーティのタグマネージャーを使用して簡単に実装できます。

  以下に、の実装に役立つ、2 つの関連トピックを示します [!DNL Target] サードパーティのタグマネージャーを使用する場合：

   * [実装する前に](/help/dev/before-implement/prepare-to-implement-target.md)
   * [実装方法 [!DNL Target] タグマネージャーなし](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

  詳しくは、サードパーティのタグマネージャーのドキュメントを参照してください。

実装するには [!DNL Target] シングルページアプリ (SPA) を使用する場合は、 [シングルページアプリケーションの実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md).
