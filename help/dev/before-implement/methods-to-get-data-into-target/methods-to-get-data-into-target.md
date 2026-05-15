---
keywords: 実装、実装、設定、ページパラメーター、tomcat、url エンコード、ページ内プロファイル属性、mbox パラメーター、ページ内プロファイル属性、スクリプトプロファイル属性、バルクプロファイル更新API、単一ファイル更新API、顧客属性、implement5、implement6、implement7、implement8、implement9、implementing0、implementing1、implementing2、implementing3、implementing4、implementing5、データプロバイダー、dataprovider、データプロバイダー
description: データを [!DNL Target]  （ページパラメーター、プロファイル属性、スクリプトプロファイル属性、データプロバイダー、単一および一括プロファイル更新API、顧客属性）に取り込みます。
title: Adobe Targetにデータを取り込むにはどうすればよいですか？
feature: Implementation
exl-id: a54e306a-ea8e-4d3f-bc5d-b5895b6b9a84
TQID: https://experienceleague.adobe.com/pmlPWRHb9tnrdSFm7s5OZ-RRsJJOxw-ntBY5AeswIcM
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 377
ht-degree: 27%

---

# メソッドの概要

Adobe Targetへのデータの取り込み方法について説明します。

使用可能な方法は次のとおりです。

| メソッド | 詳細 |
| --- | --- |
| [&#x200B; ページパラメーター](page-parameters.md)<br /> （「mbox パラメーター」とも呼ばれます） | ページパラメーターは、ページコードを介して直接渡される名前と値のペアで、今後の使用のために訪問者のプロファイルに保管されることはありません。<br /> ページパラメーターは、今後のターゲティング使用のために訪問者のプロファイルと一緒に保存する必要がない[!DNL Target]にページデータを送信するのに便利です。 これらの値は、ページまたはユーザーが特定のページでおこなったアクションの記述に使用されます。 |
| [&#x200B; ページ内プロファイル属性](in-page-profile-attributes.md)<br /> （「mbox内プロファイル属性」とも呼ばれます） | ページ内プロファイル属性は、後で使用するために訪問者のプロファイルに保存されるページコードを通じて直接渡される名前と値のペアです。<br /> ページ内プロファイル属性を使用すると、ユーザー固有のデータをTargetのプロファイルに保存して、後でターゲティングとセグメンテーションを行うことができます。 |
| [スクリプトプロファイル属性](script-profile-attributes.md) | スクリプト プロファイル属性は、[!DNL Target] ソリューションで定義された名前と値のペアです。 値は、サーバー呼び出しごとに、Target サーバーで JavaScript スニペットが実行されることで決まります。<br /> ユーザーは、mbox呼び出しごとに実行される小さなコードスニペットを作成し、訪問者がオーディエンスとアクティビティメンバーシップについて評価される前に作成します。 |
| [データプロバイダー](data-providers.md) | データプロバイダーを利用すれば、サードパーティのデータをAdobe Targetに容易に渡すことができます。 |
| [プロファイル一括更新 API](bulk-profile-update-api.md) | API経由で、多くの訪問者のプロファイルを更新して、.csv ファイルを[!DNL Target]に送信します。 各訪問者プロファイルでは、1 回の呼び出しで複数のページ内プロファイル属性を更新できます。 |
| [単一プロファイル更新 API](single-profile-update-api.md) | 一括プロファイル更新APIとほぼ同じですが、1人の訪問者プロファイルは、API呼び出しで.csv ファイルではなく行に沿って一度に更新されます。 |
| [顧客属性](customer-attributes.md) | 顧客属性を利用すると、FTP を介して訪問者のプロファイルデータを Experience Cloud にアップロードできます。 アップロードしたら、Adobe AnalyticsとAdobe Targetのデータを使用します。 |
