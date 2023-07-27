---
keywords: 実装，実装，実装， adobe launch, launch，レース，リダイレクト，エクスペリエンスplatform launch,platform launch，タグ， adobe platform，実装 2
description: の実装方法を学ぶ [!DNL Adobe Target] at.js ライブラリの使用 [!DNL Adobe Experience Platform]:Target を実装する推奨される方法です。
title: ' [!DNL Adobe Experience Platform] を使用して  [!DNL Target]  を実装する方法'
feature: Implement Server-side
exl-id: 0a325871-194a-479c-a3bf-294e3dde3e9a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '434'
ht-degree: 65%

---

# [!DNL Target] を使用した [!DNL Adobe Experience Platform] の実装

[!DNL Adobe Experience Platform] のタグは、[!DNL Adobe] の次世代タグ管理機能です。タグは、適切な顧客体験の実現に必要な分析、マーケティングおよび広告タグをデプロイおよび管理するためのシンプルな手段を提供します。

>[!NOTE]
>
>Adobe Experience Platform Launchは、 [!DNL Adobe Experience Platform]. その結果、製品ドキュメント全体でいくつかの用語の変更がロールアウトされました。用語の変更に関する参照の一覧については、次の[ドキュメント ](https://experienceleague.adobe.com/docs/experience-platform/tags/term-updates.html?)を参照してください。

詳細な情報を取得できる様々なソースを次の表に示します。

| リソース | 詳細 |
|--- |--- |
| [ Adobe Target](https://experienceleague.adobe.com/docs/launch-learn/implementing-in-websites-with-launch/implement-solutions/target.html?lang=ja#implement-solutions) の追加 | このチュートリアルでは、[!DNL Adobe Experience Platform] のタグを使用して web サイトに [!DNL Target] を実装する手順を詳しく説明します。at.js JavaScript ライブラリの追加、グローバル mbox の発行、パラメーターの追加、他のソリューションとの統合といったトピックが含まれます。この記事は、Adobe Experience Platformおよびその他のAdobe Experience Cloudソリューションの実装方法を示す大規模なチュートリアルの一部です。 |
| [クイックスタートガイド](https://experienceleague.adobe.com/docs/experience-platform/tags/get-started/quick-start.html?lang=ja) | 適切な顧客体験の実現に必要な分析、マーケティングおよび広告タグのデプロイおよび管理に関する情報。 |
| [Adobe  [!DNL Target]  拡張機能の概要](https://experienceleague.adobe.com/docs/experience-platform/tags/extensions/adobe/target/overview.html?lang=ja) | [!DNL Adobe Experience Platform] を使用した [!DNL Target] の実装に関する情報 |

## [!DNL Target] 拡張機能を使用して at.js を実装するメリット

次のメリットは、[!DNL Adobe Experience Platform] のタグを使用して at.js を実装する場合にのみ当てはまります。このため、Adobeでは、タグを [!DNL Adobe Experience Platform] at.js の手動実装ではなく

* **[!DNL Adobe Analytics] と [!DNL Target] の競合状態の解消：**[!DNL Analytics] 呼び出しは [!DNL Target] 呼び出しより先に実行される可能性があるので、[!DNL Target] 呼び出しは [!DNL Analytics] 呼び出しには結合されません。これにより、データが正しくなくなるおそれがあります。[!DNL Target] 拡張機能では、[!DNL Target] 呼び出しが成否に関わらず完了するまで [!DNL Analytics] ビーコン呼び出しが待機します。[!DNL Adobe Experience Platform] でタグを使用すると、手動で実装する場合に発生する可能性のあるデータ不整合の問題を解決できます。

  >[!NOTE]
  >
  >[!DNL Adobe Analytics] 呼び出しが [!DNL Analytics] 呼び出しを待つように、[!DNL Target] 拡張機能で「ビーコンを送信」アクションを使用します。 カスタムコードを使用して `s.t()` または `s.tl()` を直接呼び出す場合、[!DNL Analytics] 呼び出しは [!DNL Target] 呼び出しが完了するまで待機することはありません。

* **誤ったリダイレクトオファーの処理を防ぎます。** 次の条件を満たしている場合： [!DNL Target] および [!DNL Analytics] ページ上に、Target によって実行されるリダイレクトオファーがある場合、 [!DNL Analytics] トラッカーは、リクエストが不適切な場合（ユーザーが別の URL にリダイレクトされているため）にリクエストを実行します。 実装する場合 [!DNL Target] および [!DNL Analytics] タグを使用 [!DNL Adobe Experience Platform]に値を入力しない場合、この問題は発生しません。 [!DNL Adobe Experience Platform] でタグを使用すると、[!DNL Target] は、[!DNL Analytics] ビーコンリクエストを中止するように [!DNL Analytics] に指示します。
