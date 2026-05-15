---
title: オンデバイス判定を使用する際のベストプラクティス
description: ' [!DNL Adobe Target]で[!UICONTROL on-device decisioning]を使用する際のベストプラクティスについて説明します'
feature: Implement Server-side
exl-id: a0ca014d-ad9f-4ecc-961d-cb7ba236507f
TQID: https://experienceleague.adobe.com/GgVJaAal4uS1RqpCK3wNCVwPjAOaXzjXNV7EoqWhwcY
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 393
ht-degree: 3%

---

# ベストプラクティス

[!DNL Adobe]では、[!UICONTROL on-device decisioning]を使用する際に、次のベストプラクティスを推奨しています。

## 決定方法が「オンデバイス」である場合のベストプラクティス

決定方法として「オンデバイス」を使用する場合、訪問者が初めてweb ページを読み込むと、アーティファクトがダウンロードされます。 最初のページ読み込み（キャッシュなし）で行う必要があるアクティビティの選定は、アーティファクトが完全にダウンロードされた後にのみ行われます。 匿名の新規訪問者のアクティビティの選定を迅速におこなうために、いくつかのベストプラクティスを実施します。

* アーティファクトに含まれない「オンデバイス」対応アクティビティを無効にします。
* Target Premiumをお持ちの場合は、[properties/workspaces](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=ja)を使用して、異なるワークスペースに対して異なるアーティファクトファイルを作成できます。
* 正当な理由でアーティファクトファイルが非常に大きくなった場合は、「ハイブリッド」決定方法を使用できます。 この方法では、アーティファクトを並行してダウンロードでき、アーティファクトがダウンロードされるまで、すべてのTarget API呼び出しはワイヤーを経由します。 このアプローチについて詳しくは、以下の「ハイブリッド」意思決定モードに関するベストプラクティスの節を参照してください。
* シングルページアプリケーション （SPA）がある場合、[!DNL Adobe]は、最初のページ読み込み時にアプリケーションのメイン JavaScript ファイルを読み込む前に、at.jsを読み込み、初期化することをお勧めします。 このアプローチにより、アーティファクトのダウンロードがより早く開始され、エクスペリエンスのレンダリングが高速化されます。

## 決定方法が「ハイブリッド」である場合のベストプラクティス

決定方法として「ハイブリッド」を使用する場合、アーティファクトは並行してダウンロードされます。 アーティファクトがダウンロードされるまで、「locations」がデバイス上で有効な場合でも、[!DNL Target] API呼び出しはワイヤーを経由します。 この動作は、すべての`getOffers()`呼び出しのデフォルトであり、ほとんどの状況で最高のパフォーマンスを提供します。 `decisioningMethod`を`on-device`に設定して`getOffers()`の既定の動作を変更する場合は、次のベストプラクティスに従ってエラーを回避し、最高のパフォーマンスを確保してください。

* ページが初めて読み込まれたときに、`decisioningMethod`を`on-device`として`getOffers()`を呼び出す場合は、「ARTIFACT_DOWNLOAD_SUCCEEDED」 at.js イベントハンドラー内で呼び出す必要があります。エラーを回避するには、 アーティファクトが非常に大きい場合、このアプローチを使用する「場所」は、アーティファクトが完全にダウンロードされた後にのみレンダリングされるため、エクスペリエンスのレンダリングが遅れる可能性があります。 [!DNL Adobe]さんは、この方法はほとんど使用しないことをお勧めします。 このアプローチを使用する場合は、上記の「デバイス上」のベストプラクティスの節で、アーティファクトのサイズを小さくするためのベストプラクティスに従ってください。
