---
keywords: プライバシー、ip アドレス、ジオセグメンテーション、オプトアウト、オプトアウト、オプトアウト、データプライバシー、政府規制、規制、gdpr、ccpa、プライバシー、個人情報、PII
description: ' [!DNL Adobe Target] が、IP アドレス、PII、オプトアウト手順の収集と処理を含む、適用されるデータプライバシー法に準拠する方法について説明します。'
title: Targetは、PIIを含むプライバシーの問題をどのように処理しますか？
feature: Privacy & Security
exl-id: 4330e034-2483-4a25-9c87-48dbef6fc9de
TQID: https://experienceleague.adobe.com/lEllQscRLJ1I-5mu3r2TyoxYfaOb2nLHVQzG9YnL0ig
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: c2be0313-b3ae-45e0-b454-d20bf54b23f2id: d095671a-1355-40aa-8b5f-06c33c68080bid: eddd9b14-83bd-4ff4-9072-54a4a484abb7id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 814
ht-degree: 43%

---

# プライバシー

[!DNL Adobe Target] のプロセスおよび設定はプライバシーを考慮した設計になっており、個人情報保護法に準拠した方法で [!DNL Target] を使用できます。

## IP アドレスと個人情報（PII）の収集

Web サイトの訪問者の IP アドレスは、Adobe Data Processing Center（DPC）に送信されます。 訪問者のネットワーク構成によっては、IP アドレスが必ずしも訪問者のコンピューターのIP アドレスを表すわけではありません。 例えば、この IP アドレスは、Network Address Translation（NAT）ファイアウォール、HTTP プロキシ、またはインターネットゲートウェイの外部 IP アドレスである可能性があります。

>[!IMPORTANT]
>
>[!DNL Target]には、ユーザーのIP アドレスや個人を特定できる情報（PII）は保存されません。 IP アドレスは、セッション中に[!DNL Target]によってのみ使用されます（メモリ内、永続化されません）。

## IP アドレスの最終オクテットの置き換え

Adobeでは、Adobe [!DNL Target]に対してユーザーが有効にできる「プライバシーバイデザイン」設定を開発しました。 有効にすると、Adobe [!DNL Target]は、IP アドレスの収集時に、IP アドレスの最後のオクテット（最後の部分）をすぐに難読化します。 この匿名化処理は、IP アドレスに何らかの処理（オプションとして行われる IP アドレスの地域ルックアップを含む）を加える前に実行されます。

この機能を有効にすると、IP アドレスの匿名性が高まり、個人情報を特定できなくなります。 その結果、[!DNL Target]は、個人情報の収集を許可していない国のデータプライバシー法に準拠して使用できます。 IP アドレスの不明化を行うと、市レベルの情報の取得が著しく困難になる場合があります。 地域レベルおよび国レベルの情報の取得に関しては、それほど大きな影響はありません。

**[!UICONTROL 管理]** > **[!UICONTROL 実装]**&#x200B;に移動すると、[!DNL Target] UIで次の設定を使用できます。

* [!UICONTROL 最後のオクテットの難読化]: [!DNL Target]は、IP アドレスの最後のオクテットを非表示にします。
* [!UICONTROL IP全体の難読化]: [!DNL Target]は、IP アドレス全体を非表示にします。
* [!UICONTROL なし]: [!DNL Target]は、IP アドレスの一部を非表示にしません。

  ![obfuscate-ip-options](assets/obfuscate-ip.png)

[!DNL Target]は完全なIP アドレスを受け取り、指定されたとおりに（最終オクテットまたはIP全体に設定されている場合）それを難読化します。 [!DNL Target]は、現在のセッション中にのみ、難読化されたIP アドレスをメモリに保持します。

### [!DNL Adobe Experience Platform Web SDK]を使用する際のデータストリームレベルのIP難読化 {#aep}

[!DNL Platform Web SDK] （バージョン 23.4以降）を使用する場合、データストリームレベルのIP難読化設定は、[!DNL Target]で設定されているIP難読化オプションよりも優先されます。 例えば、データストリームレベルのIP難読化オプションが[!UICONTROL Full]に設定され、[!DNL Target] IP難読化オプションが[!UICONTROL 最後のオクテット難読化]に設定されている場合、[!DNL Target]は完全に難読化されたIPを受信します。

詳しくは、*[!DNL Adobe Experience Platfrom]データストリームガイド*&#x200B;の「[ データストリームの設定](https://experienceleague.adobe.com/docs/experience-platform/datastreams/configure.html?lang=ja){target=_blank}」の「[!UICONTROL IP難読化]」を参照してください。

## 地理特性

IP アドレスの最後のオクテットの置換を有効にすると、IP アドレスの残りの値を[!DNL Target]のレポートを使用して分析できます。 IP アドレスの最後のオクテットが難読化されていない場合は、完全なIP アドレスを[!DNL Target]で分析できます。 地理特性機能を使用して、地理的な地域によって訪問者の位置を把握できます。 地理特性データの精度は市レベルまたは郵便番号レベルにとどまり、個人レベルでの特定はできません。

IP アドレスが完全に不明化されている場合、地理特性と 地域ターゲティングは使用できません。

## オプトアウトリンク

訪問者がカウントやコンテンツ配信をすべて停止（オプトアウト）できるオプトアウトリンクを、サイトに追加できます。

1. サイトに次のリンクを追加します。

   `<a href="https://clientcode.tt.omtrdc.net/optout"> Your Opt Out Language Here</a>`

1. （条件付き） CNAMEを使用している場合、リンクには「client=`clientcode` パラメーターを含める必要があります。次に例を示します。
   `https://my.cname.domain/optout?client=clientcode` を参照してください。

1. `clientcode` をお客様のクライアントコードに置き換え、このオプトアウト URL にリンクさせるテキストまたは画像を追加します。

このリンクをクリックした訪問者は、Cookie を削除するか、最初に訪問してから 2 年経過するまで、閲覧しているセッションから呼び出される mbox リクエストに含まれません。 `disableClient` ドメイン内の「`clientcode.tt.omtrdc.net`」という訪問者に Cookie が設定され、この機能が適用されます。

ファーストパーティ Cookie の導入を使用している場合でも、提供されたオプトアウトは、サードパーティ Cookie を使用するように設定されます。 クライアントがファーストパーティ Cookieのみを使用している場合、[!DNL Target]はオプトアウト Cookieが設定されているかどうかを確認します。

## プライバシーとデータ保護規制

欧州連合の一般データ保護規則（GDPR）、カリフォルニア州消費者プライバシー法（CCPA）、その他の国際的なプライバシー要件、およびこれらの規制が組織と[!DNL Target]にどのような影響を与えるかについては、[ プライバシーおよびデータ保護規則](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md)を参照してください。

## 機能使用状況データの収集

個々の機能使用状況データは、Adobeの内部で収集され、[!DNL Target]機能が意図したとおりに機能しているか、使用率が低い機能を特定するためのものです。 様々な待ち時間の測定値は、パフォーマンス上の問題に対処するために収集されます。 個人データは収集されません。

SDK での使用状況データのレポートからオプトアウトするには、クライアント初期化オプションで `telemetryEnabled` を false に設定します。 詳しくは、[targetGlobalSettings の telemetryEnabled](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#telemetryenabled) を参照してください。
