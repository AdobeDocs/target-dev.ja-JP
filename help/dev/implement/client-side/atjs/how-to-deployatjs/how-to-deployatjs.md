---
keywords: 実装, at.js, JavaScript ライブラリ
description: タグマネージャー内またはタグマネージャーなしでタグを使用して  [!DNL Adobe Target] at.js JavaScript ライブラリをデプロイする方法に  [!DNL Adobe Experience Platform]  いて説明します。
title: at.js をデプロイするにはどうすればよいですか？
feature: Implement Server-side
exl-id: e62cb27e-ea80-462b-90f8-0a033b128031
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '278'
ht-degree: 28%

---

# at.js のデプロイ方法

[!DNL Adobe Target] JavaScript ライブラリの at.js のデプロイ方法について説明します。このライブラリは、タグ内のタグを使用してデプロイする場合と、タグ [!DNL Adobe Experience Platform] ネージャーを使用せずにデプロイする場合とがあります。

次の方法を使用して at.js をデプロイできます。

* **[Adobe Experience Platformへのタグの実装  [!DNL Target]  使用](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md)**:[!DNL Adobe Experience Platform] のタグは、Adobeが提供する次世代タグ管理機能です。 タグは、適切な顧客体験の実現に必要な分析、マーケティングおよび広告タグをデプロイおよび管理するためのシンプルな手段を提供します。

>[!NOTE]
>
> [!DNL Adobe Experience Platform Launch] は、[!DNL Adobe Experience Platform] のデータ収集テクノロジーのスイートとしてリブランドされました。その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更に関する参照の一覧については、次の[ドキュメント ](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html)を参照してください。

* **[タグマネージャーを使用しない実装  [!DNL Target] ](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)**：タグマネージャー（[!DNL Adobe Experience Platform] のタグなど）を使用せずに [!DNL Target] を実装できます。
* **サードパーティのタグマネージャーを使用した [!DNL Target] の実装**:[Adobe Experience Platformのタグ ](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) は、[!DNL Target] を実装する場合に推奨される方法です。ただし、Tealium、Enghten、Google Tag などのサードパーティのタグマネージャーを使用して、[!DNL Target] を実装することもできます。 Launch を使用する利点のリストについては、[ 拡張機能を使用して at.js を実装する利点  [!DNL Adobe Target]  を参照してください ](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md#advantages-of-implementing-atjs-using-the-target-extension)。

  ただし、タグマネージャーを使用せずに [!DNL Target] を実装する方法を理解している場合は、サイトコードで at.js をハードコーディングする代わりに、サードパーティのタグマネージャーを使用して簡単に実装できます。

  サードパーティのタグマネージャーを使用した [!DNL Target] の実装に役立つ、2 つの関連トピックを次に示します。

   * [実装する前に](/help/dev/before-implement/prepare-to-implement-target.md)
   * [タグマネ  [!DNL Target]  ジャーを使用しない実装](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-without-a-tag-manager.md)

  詳しくは、サードパーティのタグマネージャーのドキュメントを確認してください。

シングルページアプリケーション（SPA）使用時に [!DNL Target] を実装するには、[ シングルページアプリケーションの実装 ](/help/dev/implement/client-side/atjs/how-to-deployatjs/target-atjs-single-page-application.md) を参照してください。
