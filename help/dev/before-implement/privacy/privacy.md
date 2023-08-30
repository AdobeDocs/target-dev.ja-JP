---
keywords: プライバシー， ip アドレス，地理特性，オプトアウト，オプトアウト，データプライバシー，政府規制，規制， gdpr, ccpa，プライバシー，個人を特定できる情報， PII
description: 方法を学ぶ [!DNL Adobe Target] は、IP アドレス、PII、オプトアウト手順の収集と処理を含む、適用されるデータプライバシー法に準拠しています。
title: PII を含むプライバシーの問題に Target はどのように対処しますか？
feature: Privacy & Security
exl-id: 4330e034-2483-4a25-9c87-48dbef6fc9de
source-git-commit: 88bde40aa6dfb96e1d53e4db6ba5547d38dbbb99
workflow-type: tm+mt
source-wordcount: '790'
ht-degree: 42%

---

# プライバシー

[!DNL Adobe Target] のプロセスおよび設定はプライバシーを考慮した設計になっており、個人情報保護法に準拠した方法で [!DNL Target] を使用できます。

## IP アドレスと個人を特定できる情報 (PII) の収集

Web サイトの訪問者の IP アドレスは、Adobe Data Processing Center（DPC）に送信されます。訪問者のネットワーク設定によっては、この IP アドレスが訪問者のコンピューターの IP アドレスと一致しない場合があります。 例えば、この IP アドレスは、Network Address Translation（NAT）ファイアウォール、HTTP プロキシ、またはインターネットゲートウェイの外部 IP アドレスである可能性があります。

>[!IMPORTANT]
>
>[!DNL Target] では、ユーザーの IP アドレスや個人を特定できる情報 (PII) を保存しません。 IP アドレスは、 [!DNL Target] （メモリ内、永続化なし）。

## IP アドレスの最終オクテットの置き換え

Adobeは、ユーザーがAdobeを有効にできる「プライバシーバイデザイン」設定を開発しました [!DNL Target]. 有効な場合、Adobe [!DNL Target] は、IP アドレスの収集時に、IP アドレスの最後のオクテット（最後の部分）を直ちに難読化します。 この匿名化処理は、IP アドレスに何らかの処理（オプションとして行われる IP アドレスの地域ルックアップを含む）を加える前に実行されます。

この機能を有効にすると、IP アドレスの匿名性が高まり、個人情報を特定できなくなります。その結果、 [!DNL Target] は、個人情報の収集を許可しない国のデータプライバシー法に準拠して使用できます。 IP アドレスの不明化を行うと、市レベルの情報の取得が著しく困難になる場合があります。地域レベルおよび国レベルの情報の取得に関しては、それほど大きな影響はありません。

以下の設定を [!DNL Target] に移動して UI を表示 **[!UICONTROL 管理]** > **[!UICONTROL 実装]**:

* [!UICONTROL 最終オクテットの難読化]: [!DNL Target] IP アドレスの最後のオクテットを非表示にします。
* [!UICONTROL IP 全体の難読化]: [!DNL Target] IP アドレス全体を非表示にします。
* [!UICONTROL なし]: [!DNL Target] は、IP アドレスの一部を非表示にしません。

  ![obfuscate-ip-options](assets/obfuscate-ip.png)

[!DNL Target] は完全な IP アドレスを受け取り、（「最後のオクテット」または「IP 全体」に設定されている場合）指定に従って難読化します。 [!DNL Target] 次に、現在のセッション中にのみ、不明化された IP アドレスをメモリに保持します。

### を使用する場合のデータストリームレベルの IP 難読化 [!DNL Adobe Experience Platform Web SDK] {#aep}

を使用する場合、 [!DNL Platform Web SDK] （バージョン 23.4 以降）では、データストリームレベルの IP の不明化設定は、 [!DNL Target]. 例えば、datastream-level IP obfuscation（データストリームレベルの IP の不明化）オプションが [!UICONTROL 完全] そして [!DNL Target] IP obfuscation（IP の不明化）オプションが [!UICONTROL 最終オクテットの難読化], [!DNL Target] は、完全に不明化された IP を受け取ります。

詳しくは、 [!UICONTROL IP Obfuscation（IP の不明化）] in [データストリームの設定](https://experienceleague.adobe.com/docs/experience-platform/datastreams/configure.html){target=_blank} （内） *[!DNL Adobe Experience Platfrom]データストリームガイド*.

## 地理特性

IP アドレスの最後のオクテットの置き換えを有効にした場合、IP アドレスの残りの値は、 [!DNL Target]. IP アドレスの最後のオクテットが不明化されていない場合は、 [!DNL Target]. 地理特性機能を使用して、地理的な地域によって訪問者の位置を把握できます。地理特性データの精度は市レベルまたは郵便番号レベルにとどまり、個人レベルでの特定はできません。

IP アドレスが完全に不明化されている場合、地理特性と Geotargeting は使用できません。

## オプトアウトリンク

訪問者がカウントやコンテンツ配信をすべて停止（オプトアウト）できるオプトアウトリンクを、サイトに追加できます。

1. サイトに次のリンクを追加します。

   `<a href="https://clientcode.tt.omtrdc.net/optout"> Your Opt Out Language Here</a>`

1. （条件付き）CNAME を使用している場合、リンクには「client=`clientcode` 次に例を示します。
   `https://my.cname.domain/optout?client=clientcode` を参照してください。

1. `clientcode` をお客様のクライアントコードに置き換え、このオプトアウト URL にリンクさせるテキストまたは画像を追加します。

このリンクをクリックした訪問者は、Cookie を削除するか、最初に訪問してから 2 年経過するまで、閲覧しているセッションから呼び出される mbox リクエストに含まれません。`disableClient` ドメイン内の「`clientcode.tt.omtrdc.net`」という訪問者に Cookie が設定され、この機能が適用されます。

ファーストパーティ Cookie の導入を使用している場合でも、提供されたオプトアウトは、サードパーティ Cookie を使用するように設定されます。クライアントがファーストパーティ Cookie のみを使用している場合、 [!DNL Target] はオプトアウト Cookie が設定されているかどうかを確認します。

## プライバシーとデータ保護規制

詳しくは、 [プライバシーとデータ保護規制](/help/dev/before-implement/privacy/cmp-privacy-and-general-data-protection-regulation.md) 欧州連合 (EU) の一般データ保護規則 (GDPR)、カリフォルニア州消費者プライバシー法 (CCPA) およびその他の国際的なプライバシー要件、およびこれらの規制が組織および [!DNL Target].

## 機能使用状況データの収集

個々の機能使用状況データは、内部Adobeの目的で収集され、 [!DNL Target] 機能は、意図したとおりに実行され、使用率が低い機能を識別します。 様々な待ち時間の測定値は、パフォーマンス上の問題に対処するために収集されます。個人データは収集されません。 

SDK での使用状況データのレポートからオプトアウトするには、クライアント初期化オプションで `telemetryEnabled` を false に設定します。詳しくは、[targetGlobalSettings の telemetryEnabled](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#telemetryenabled) を参照してください。
