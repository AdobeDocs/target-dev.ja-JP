---
keywords: プライバシー，ip アドレス，地理特性，オプトアウト，オプトアウト，データプライバシー，政府規制，規制，gdpr, ccpa, プライバシー，個人を特定できる情報，PII
description: IP アドレス  [!DNL Adobe Target] PII、オプトアウト手順の収集と取り扱いを含む、適用されるデータプライバシー法に準拠する方法を説明します。
title: Target では、PII を含むプライバシーの問題をどのように処理しますか？
feature: Privacy & Security
exl-id: 4330e034-2483-4a25-9c87-48dbef6fc9de
source-git-commit: 88bde40aa6dfb96e1d53e4db6ba5547d38dbbb99
workflow-type: tm+mt
source-wordcount: '771'
ht-degree: 43%

---

# プライバシー

[!DNL Adobe Target] のプロセスおよび設定はプライバシーを考慮した設計になっており、個人情報保護法に準拠した方法で [!DNL Target] を使用できます。

## IP アドレスと個人を特定できる情報（PII）の収集

Web サイトの訪問者の IP アドレスは、Adobe Data Processing Center（DPC）に送信されます。訪問者のネットワーク設定によっては、IP アドレスが訪問者のコンピューターの IP アドレスを表しているとは限りません。 例えば、この IP アドレスは、Network Address Translation（NAT）ファイアウォール、HTTP プロキシ、またはインターネットゲートウェイの外部 IP アドレスである可能性があります。

>[!IMPORTANT]
>
>[!DNL Target] は、ユーザーの IP アドレスや個人情報（PII）を保存しません。 IP アドレスは、セッション中に [!DNL Target] によってのみ使用されます（メモリ内、永続化されません）。

## IP アドレスの最終オクテットの置き換え

Adobeは、ユーザーがAdobe[!DNL Target] 定を有効にできる「プライバシーバイデザイン」設定を開発しました。 有効にすると、Adobe[!DNL Target] は IP アドレスを収集した時点での IP アドレスの最後のオクテット（最後の部分）を直ちに不明化します。 この匿名化処理は、IP アドレスに何らかの処理（オプションとして行われる IP アドレスの地域ルックアップを含む）を加える前に実行されます。

この機能を有効にすると、IP アドレスの匿名性が高まり、個人情報を特定できなくなります。その結果、個人情報の収集を許可していない国のデータプライバシー法に準拠して [!DNL Target] を使用することができます。 IP アドレスの不明化を行うと、市レベルの情報の取得が著しく困難になる場合があります。地域レベルおよび国レベルの情報の取得に関しては、それほど大きな影響はありません。

[!DNL Target] UI で **[!UICONTROL Administration]**/**[!UICONTROL Implementation]** に移動すると、次の設定を使用できます。

* [!UICONTROL Last octet obfuscation]: [!DNL Target] は IP アドレスの最後のオクテットを非表示にします。
* [!UICONTROL Entire IP obfuscation]: [!DNL Target] は IP アドレス全体を非表示にします。
* [!UICONTROL None]: [!DNL Target] は IP アドレスのどの部分も非表示にしません。

  ![obfuscate-ip-options](assets/obfuscate-ip.png)

[!DNL Target] は、完全な IP アドレスを受け取り、指定されたとおりに（最後のオクテットまたは IP 全体に設定されている場合は）不明化します。 [!DNL Target] の後、現在のセッションの間だけ、不明化された IP アドレスをメモリに保持します。

### [!DNL Adobe Experience Platform Web SDK] を使用する場合のデータストリームレベルの IP の不明化 {#aep}

[!DNL Platform Web SDK] （バージョン 23.4 以降）を使用する場合、データストリームレベルの IP の不明化の設定は、[!DNL Target] で設定した IP の不明化オプションよりも優先されます。 例えば、データストリームレベルの IP の不明化オプションが [!UICONTROL Full] に設定され、[!DNL Target] IP の不明化オプションが [!UICONTROL Last octet obfuscation] に設定されている場合、[!DNL Target] は完全に不明化された IP を受け取ります。

詳しくは、*[!DNL Adobe Experience Platfrom]データストリームガイドの [ データストリームの設定 ](https://experienceleague.adobe.com/docs/experience-platform/datastreams/configure.html?lang=ja){target=_blank} の [!UICONTROL IP Obfuscation] を参照してください*。

## 地理特性

IP アドレスの最後のオクテットを置き換えることを有効にすると、[!DNL Target] のレポートを使用して、IP アドレスの残りの値を分析できます。 IP アドレスの最後のオクテットが不明化されていない場合は、完全な IP アドレスを [!DNL Target] で分析できます。 地理特性機能を使用して、地理的な地域によって訪問者の位置を把握できます。地理特性データの精度は市レベルまたは郵便番号レベルにとどまり、個人レベルでの特定はできません。

IP アドレスが完全に不明化されている場合、地理特性と Geotargeting は使用できません。

## オプトアウトリンク

訪問者がカウントやコンテンツ配信をすべて停止（オプトアウト）できるオプトアウトリンクを、サイトに追加できます。

1. サイトに次のリンクを追加します。

   `<a href="https://clientcode.tt.omtrdc.net/optout"> Your Opt Out Language Here</a>`

1. （条件付き） CNAME を使用している場合、リンクには&quot;client=`clientcode` パラメーターを含める必要があります。次に例を示します。
   `https://my.cname.domain/optout?client=clientcode` を参照してください。

1. `clientcode` をお客様のクライアントコードに置き換え、このオプトアウト URL にリンクさせるテキストまたは画像を追加します。

このリンクをクリックした訪問者は、Cookie を削除するか、最初に訪問してから 2 年経過するまで、閲覧しているセッションから呼び出される mbox リクエストに含まれません。`disableClient` ドメイン内の「`clientcode.tt.omtrdc.net`」という訪問者に Cookie が設定され、この機能が適用されます。

ファーストパーティ Cookie の導入を使用している場合でも、提供されたオプトアウトは、サードパーティ Cookie を使用するように設定されます。クライアントがファーストパーティ cookie のみを使用してい [!DNL Target] 場合、オプトアウト cookie が設定されているかどうかを確認します。

## プライバシーとデータ保護規制

欧州連合（EU）の一般データ保護規則（GDPR）、カリフォルニア州消費者プライバシー法（CCPA）およびその他の国際的なプライバシー要件、およびこれらの規制が組織および [!DNL Target] ーザーに与える影響については、[ プライバシーとデータ保護規制 ](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) を参照してください。

## 機能使用状況データの収集

個々の機能使用状況データは、内部Adobeのために収集され、[!DNL Target] の機能が意図したとおりに実行されているかを確認する、または使用率が低い機能を特定する。 様々な待ち時間の測定値は、パフォーマンス上の問題に対処するために収集されます。個人データは収集されません。 

SDK での使用状況データのレポートからオプトアウトするには、クライアント初期化オプションで `telemetryEnabled` を false に設定します。詳しくは、[targetGlobalSettings の telemetryEnabled](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#telemetryenabled) を参照してください。
