---
title: オンデバイス判定を使用する際のベストプラクティス
description: ' [!DNL Adobe Target] で [!UICONTROL on-device decisioning] を使用する場合のベストプラクティスについて説明します。'
feature: Implement Server-side
exl-id: a0ca014d-ad9f-4ecc-961d-cb7ba236507f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 0%

---

# ベストプラクティス

[!DNL Adobe] では、[!UICONTROL on-device decisioning] を使用する際に次のベストプラクティスを推奨します。

## 判定方法が「オンデバイス」の場合のベストプラクティス

判定方法として「オンデバイス」を使用すると、訪問者が初めて web ページを読み込むときにアーティファクトがダウンロードされます。 最初のページの読み込み時（キャッシュなし）に行う必要のあるアクティビティの選定は、アーティファクトが完全にダウンロードされた後でのみ行われます。 新しい匿名訪問者に対してアクティビティの選定を迅速に行うために、従うことのできるベストプラクティスがいくつかあります。

* アーティファクト内に存在しない「オンデバイス」対応のアクティビティを非アクティブ化します。
* Target Premium がある場合、[properties/workspaces](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/enterprise/property-channel.html?lang=ja) を使用して、異なるワークスペースに対して異なるアーティファクトファイルを作成できます。
* 正当な理由でアーティファクトファイルが非常に大きくなる場合は、「ハイブリッド」判定方法を使用できます。 このメソッドを使用すると、アーティファクトを並行してダウンロードでき、すべての Target API 呼び出しはアーティファクトがダウンロードされるまでワイヤーを介して行われます。 このアプローチについて詳しくは、以下の「ハイブリッド」決定モードに関するベストプラクティスの節を参照してください。
* シングルページアプリケーション（SPA）の場合は、[!DNL Adobe] 初のページ読み込み時にアプリケーションのメイン JavaScript ファイルを読み込む前に、at.js を読み込んで初期化することをお勧めします。 このアプローチは、アーティファクトのダウンロードをより早く開始し、より高速なエクスペリエンスレンダリングを提供します。

## 判定方法が「ハイブリッド」の場合のベストプラクティス

判定方法として「ハイブリッド」を使用する場合、アーティファクトは並行してダウンロードされます。 アーティファクトがダウンロードされるまで、「ロケーション」がオンデバイス対応であっても、[!DNL Target] の API 呼び出しはすべて転送されます。 この動作は、すべての `getOffers()` 呼び出しのデフォルトであり、ほとんどの状況で最高のパフォーマンスを提供します。 `decisioningMethod` を `on-device` に設定して `getOffers()` のデフォルトの動作を変更する場合は、エラーを回避し最高のパフォーマンスを確保するために、次のベストプラクティスに従ってください。

* ページの初回読み込み時に `decisioningMethod` as `on-device` を使用して `getOffers()` を呼び出す場合は、エラーを避けるために、「ARTIFACT_DOWNLOAD_SUCCEEDED」 at.js イベントハンドラー内で呼び出す必要があります。 アーティファクトが非常に大きい場合、この方法を使用する「場所」はアーティファクトが完全にダウンロードされた後にのみレンダリングされるので、レンダリングの速度が遅れる可能性があります。 [!DNL Adobe] では、この方法をほとんど使用しないことをお勧めします。 この方法を使用する場合は、上記の「デバイス上」のベストプラクティスの節にあるアーティファクトサイズを減らすためのベストプラクティスに従ってください。
