---
keywords: gdpr, eu，欧州連合，プライバシー，faq，よくある質問，カリフォルニア州消費者プライバシー法，ccpa, プライバシー，データ保護，オプトアウト，オプトアウト，政府，規制，gdpr5, gdpr6, gdpr7, gdpr8, gdpr9, eu0, eu1, eu2, eu3, eu5
description: Target と EU 一般データ保護規則（GDPR）、カリフォルニア州消費者プライバシー法（CCPA）、その他のプライバシー要件について説明します。
title: Target では、プライバシーとデータ保護規制にどのように対応しますか？
feature: Privacy & Security
exl-id: 40bac3c5-8e6f-4a90-ac0c-eddce1dbe6c0
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '2329'
ht-degree: 62%

---

# プライバシーとデータ保護規制

EU 一般データ保護規則（GDPR）、カリフォルニア州消費者プライバシー法（CCPA）およびその他の国際的なプライバシー要件に関する情報です。これらの規制が組織とAdobe Targetに与える影響を説明します。

## プライバシーと一般データ保護規則（GDPR）の概要

2018年5月25日（PT）に、欧州連合の GDPR が発効されました。この規制にはどのような意味があるのか、詳細は、[GDPR とビジネス](https://business.adobe.com/jp/privacy/general-data-protection-regulation.html)を参照してください。

アドビが企業に対してソフトウェアやサービスを提供する場合、アドビはサービス提供の一環として同社が処理または保管する個人データのデータ処理事業者に該当します。アドビはデータ処理者として、お客様の許可と指示（お客様とアドビとの間で締結された契約の内容など）に従って個人データを処理します。

データ管理者であるお客様は、アドビに処理および保管を委任する個人データを決めます。Adobe Experience Cloud ソリューションをご利用のお客様の場合は、お客様が使用しているソリューションと、お客様が Adobe Experience Cloud アカウントに送信するよう設定した情報に基づいて、アドビは個人データをホストします。詳細な例については、[Adobe Experience Cloud のプライバシー](https://www.adobe.com/jp/privacy/experience-cloud.html#collect)を参照してください。

Adobe Experience Cloudではデータ管理者向けの GDPR に対応した API が備わっており、これらの API を利用することで次の作業を実行できます。

* Target に保存されているデータサブジェクト情報へのアクセス
* Target に保存されているデータサブジェクト情報の削除

詳しくは、次を参照してください。

* [AdobePrivacy Serviceの概要 ](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?lang=ja)
* [Privacy ServiceAPI ガイド ](https://experienceleague.adobe.com/docs/experience-platform/privacy/api/overview.html?lang=ja)
* [Privacy ServiceUI の概要 ](https://experienceleague.adobe.com/docs/experience-platform/privacy/ui/overview.html?lang=ja)

## カリフォルニア州消費者プライバシー法（CCPA）の概要

カリフォルニア州消費者プライバシー法（CCPA）は、カリフォルニア州の消費者に個人情報に関する新しい権利を提供し、カリフォルニア州でビジネスを行う特定の事業者に対してデータ保護の責任を課します。CCPA は 2020年1月1日（PT）に施行されました。

概要としては、この法律は、カリフォルニアの人々に次の権利を含む、いくつかの主要な権利を提供します。

* 要求情報（データアクセス）
* 個人情報の販売のオプトアウト（サードパーティとの情報の共有をオプトアウトするために広く定義された権利）
* 個人情報を削除させる
* 個人情報が公開または販売されたことを知る

昨年の欧州のプライバシー法（GDPR）の準備に取り組んでいたユーザーは、これらの権利の一部に精通しており、おこなった作業の多くは再利用できる可能性があります。

>[!NOTE]
>
>CCPA に適用されるデータへのアクセスと削除は、GDPR の場合と同じ手順に従います。

## Adobe TargetとAdobe Experience Platformのオプトイン

Target では、お客様の同意管理戦略を支援できるように、Adobe Experience Platformのタグを介してオプトイン機能がサポートされています。 オプトイン機能を使用すると、Target タグを実行する方法とタイミングを制御できます。Adobe Experience Platformから Target タグを事前承認するオプションもあります。 の Target at.js ライブラリでオプトインを使用する機能を有効にするには、`targetGlobalSettings` を使用して、`optinEnabled=true` 設定を追加する必要があります。 Adobe Experience Platformで、拡張機能インストール表示の「GDPR オプトイン」ドロップダウンリストから「有効」を選択します。 詳しくは、[Adobe Experience Platformを使用した Target の実装 ](/help/dev/implement/client-side/atjs/how-to-deployatjs/implement-target-using-adobe-launch.md) を参照してください。

次のコードスニペットに、`optinEnabled=true` 設定を有効にする方法を示します。

```
window.targetGlobalSettings = {
  optinEnabled: true
};
```

>[!NOTE]
>
>オプトイン機能は、at.js バージョン 1.7.0 および at.js 2.1.0 以降でサポートされます。オプトインは、at.js バージョン 2.0.0 および 2.0.1 ではサポートされていません。

Adobe Experience Platformを使用したオプトインの管理は、推奨されるアプローチです。 Adobe Experience Platformではオプトインをきめ細かく制御でき、Target による処理が許可されるまでページ内の特定の要素を非表示にすることができるので、お客様の同意戦略の一環として役立ちます。

オプトインを使用する場合に検討すべきシナリオには、以下の 3 つがあります。

1. **Target タグは、Adobe Experience Platform（または事前に承認された Target のデータ主体）を介して事前承認されています。** Target タグは、同意が得られず、期待どおりに機能しません。
1. **Target タグが事前に承認されていない状態で、`bodyHidingEnabled` が FALSE に設定されている：** Target タグは、お客様から同意が得られるまで実行されません。同意が得られるまでは、デフォルトコンテンツのみを使用できます。同意が得られると Target が呼び出されて、パーソナライズされたコンテンツがデータ主体（訪問者）に対して提供されるようになります。同意が得られるまではデフォルトコンテンツしか使用できないので、適切な戦略を採用することが重要です。例えば、スプラッシュページを使用してページの一部やパーソナライズされる可能性があるコンテンツを覆い隠すことなどを検討してください。これにより、データ主体（訪問者）のエクスペリエンスの一貫性を維持することができます。
1. **Target タグが事前に承認されていない状態で、`bodyHidingEnabled` が TRUE に設定されている：** Target タグは、お客様から同意が得られるまで実行されません。同意が得られるまでは、デフォルトコンテンツのみを使用できます。ただし、`bodyHidingEnabled` が true に設定されているので、Target タグが実行されるまで（またはデータ主体がオプトインを拒否するまで）ページ上で非表示になるコンテンツは `bodyHiddenStyle` によって決定されます。データ主体がオプトインを拒否した場合は、デフォルトコンテンツが表示されます。デフォルトでは、`bodyHiddenStyle` が `body { opacity:0;}` に設定されているので、HTML body タグは非表示になります。以下に、アドビが推奨するページ設定を示します。この設定では、ページのコンテンツを 1 つのコンテナに配置し、同意管理ダイアログを別のコンテナに配置することで、同意管理ダイアログ以外のページ本文全体を非表示にしています。このように Target を設定すると、ページコンテンツのコンテナのみが非表示になります。詳しくは、[Privacy Service の概要](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?)を参照してください。

   次に、3 つ目のシナリオで推奨されるページ設定を示します。

   ```
   <html> 
   <head> 
   //visitor, at.js 
   </head> 
   
   <body> 
   <div id = "consentManagerDialog"> 
   
   //consent manager html dialog goes here 
   </div> 
   
   <div id="pageContent"> 
   // page content goes here 
   </div> 
   
   </body> 
   </html> 
   ```

   `bodyHiddenStyle` は次のように設定されているものとします。

   ```
   #pageContent { opacity:0;}
   ```

## プライバシーとデータ保護規制 FAQ

欧州連合（EU）の一般データ保護規則（GDPR）、カリフォルニア州消費者プライバシー法（CCPA）および Target に特有のその他の国際的なプライバシー要件に関するよくある質問です。

### これらの規制に対するAdobeポリシーはどのようなものですか？

Adobeは、データ処理者としての義務を既に果たしているか、または履行中です。 アドビでは、認定済みのセキュリティとプライバシー制御の強固な基盤を計画的に保持しており、2018年5月の期限に先立って、製品の機能強化を行いました。大規模法人のお客様には、これらの機能強化を実装し、必要な方針や手順を更新するという責任があります。

### データ管理者となる私の会社は、使用するAdobe Experience Cloudの各ソリューションにおいて GDPR または CCPA 要求を送信する必要がありますか？

いいえ、Adobeでは、データ管理者が GDPR および CCPA の要件を満たすのに役立つ一元化された手段を提供します。 データ管理者は、各ソリューションで直接作業する必要はありません。

Target を含むExperience Cloudソリューションに関する GDPR および CCPA 要求は、一元化されたAdobeAPI を介して行われます。現在、この API は GDPR API と呼ばれています。 この API は、データ管理者のExperience Cloudソリューションスイートを介してリクエストを完了します。

### データ主体/ユーザーのリクエストに応じてAdobeで顧客が削除できる情報には何がありますか？

個々の訪問者に関する Target 内の情報は、Target 訪問者プロファイルに格納されています。顧客は Target を使用して、訪問者プロファイル内の特定 ID に関連づけられたすべてのデータを削除できます。 プロファイルデータターゲットストアの例については、[ 訪問者プロファイル ](https://experienceleague.adobe.com/docs/target/using/audiences/create-audiences/categories-audiences/visitor-profile.html) を参照してください。

個人を特定しない集約化されたあるいは匿名化されたデータ（レポートデータなど）または特定の個人に関連しないデータ（コンテンツデータなど）は、ユーザーからの削除要請の範疇外となります。

90 日間非アクティブであった Target の訪問者プロファイルは、特に作業をおこなわなくてもデフォルト設定で削除されます。

### Target に対する GDPR または CCPA のアクセスおよび削除要求を顧客が行うには、どのような ID を利用できますか？

顧客プロファイルを見つけるために Target がサポートするのは次の ID タイプです。

| ユーザー ID | 名前空間 ID タイプ | 名前空間 ID | 定義 |
|--- |--- |--- |--- |
| Experience Cloud ID（ECID） | Standard | 4 | Adobe Experience Cloud ID （旧称：訪問者 ID またはExperience CloudID）。 JavaScript API を利用して ID を見つけることができます（詳細は下記を参照してください）。 |
| TnT ID/Cookie ID （TNTID） | Standard | 9 | 訪問者のブラウザーで cookie として設定される Target 識別子。 JavaScript API を利用して ID を見つけることができます（詳細は下記を参照してください）。 |
| サードパーティ ID/CRM ID （THIRDPARTYID） | Target-Specific | 該当なし | 顧客の CRM やその他の一意の ID を Target に提供している場合。 |

>[!NOTE]
>
>Target では、ファーストパーティとサードパーティのクロスドメイン Cookie が両方ともサポートされていますが、GDPR および CCPA を確実に順守するには、ファーストパーティの Target Cookie のみを使用することをお勧めします。

### Target はどのように同意の管理をおこないますか？

GDPR および CCPA では、いつ同意の取得が必要となるかに関する変更はありませんが、同意を得る方法が変更されます。それぞれのお客様の同意戦略は、データ収集と利用の手法およびプライバシーポリシーに左右されます。同意管理はではサポートされておらず、GDPR および CCPA の場合は Target を介して同意を管理することはできません。

アドビは現在、同意管理ソリューションは提供していませんが、市場では新たな要件のいくつかに対処するための様々なツールが開発されています。同意管理者を含む、プライバシーツール全般について詳しくは、[International Association of Privacy Professionals（iapp）](https://iapp.org/media/pdf/resource_center/Tech-Vendor-Directory-1.4.1-electronic.pdf)Web サイトの *2017 Privacy Tech Vendor Report* を参照してください。

Target では、お客様の同意管理戦略を支援できるように、Adobe Experience Platformを介してオプトイン機能がサポートされています。 オプトイン機能を使用すると、Target タグを実行する方法とタイミングを制御できます。Adobe Experience Platformから Target タグを事前承認するオプションもあります。 Adobe Experience Platformを使用したオプトインの管理は、推奨されるアプローチです。 Adobe Experience Platformではオプトインをきめ細かく制御でき、Target による処理が許可されるまでページ内の特定の要素を非表示にすることができるので、お客様の同意戦略の一環として役立ちます。

GDPR、CCPA およびAdobe Experience Platformについて詳しくは、[Adobeプライバシーに関するJavaScript ライブラリと一般データ保護規則 ](https://experienceleague.adobe.com/docs/experience-platform/privacy/home.html?) を参照してください。 また、前述の *Adobe Target と Adobe Experience Platform のオプトイン*&#x200B;の節も参照してください。

### `AdobePrivacy.js` は情報を GDPR API に送信しますか？

AdobePrivacy.js は *この情報を API に送信しません*。 これは、お客様が行う必要があります。このライブラリは、特定の訪問者のブラウザーに保存されている ID のみを提供します。

### `removeIdentities` は何を削除するのですか？

`removeIdentities` が削除するのは、ブラウザーからの識別子&#x200B;*のみ*&#x200B;で、Adobe ソリューションが実装したものかどうかによって決まります。

例えば、Target は ID を格納している Cookie を削除しますが、Adobe Audience Manager（AAM）は、サードパーティ Cookie に格納されている demdex ID を削除しません。

### Target の GDPR または CCPA 要求には、どのような情報を含める必要がありますか？

セントラルPrivacy Serviceからの要件に加えて、Target の有効な GDPR または CCPA メッセージには次のものが含まれます。

```
{ 
    "jobId":"12345AD43E", 
    ... 
    "products":["Target",...], 
    "companyContexts":[ 
        { 
            "namespace":"imsOrgID", 
            "value":"123456789@AdobeOrg" 
        }, 
        ... 
    ], 
    "userContexts":[ 
        { 
            "namespace":"ECID", 
            "namespaceId":4, 
            "type":"standard", 
            "value":"53792210477379708453829363835595041181" 
        } 
        And/OR: 
        { 
            "namespace":"TNTID", 
            "namespaceId":9, 
            "type":"standard", 
            "value":"1234567890" 
        } 
        And/OR: 
        { 
            "namespace":"THIRDPARTYID", 
            "type":"target", 
            "value":"thirdPartyIdName" 
        }, 
        ... 
    ] 
}
```

### GDPR API を介した Target からの想定される応答には、どのような種類がありますか？

| 要求のステータス | Target の応答メッセージ | シナリオ |
|--- |--- |--- |
| 処理中 | 処理中 | Target は GDPR または CCPA 要求を受信し、処理中です。 |
| 完了 | 該当なし - 会社のコンテキストは適用できません | GDPR または CCPA 要求内の IMS ID は、Target クライアントにマップされていません。<br />一部の会社は複数の IMS ID を持っています。Target がプロビジョニングされている IMS ID を送信します。 |
| 完了 | 該当なし - ユーザーのコンテキストがありません | GDPR または CCPA 要求で提供された特定の訪問者またはデータ主体の ID は、Target プロファイルストアにありません。<br /> この結果は、Target でサポートされていない名前空間 ID タイプを送信しようとした場合も返されます（サポートされている ID については、上記を参照してください）。 |
| エラー | エラーメッセージ（詳細はエラーのタイプによって異なります） | 要求されたデータ主体のプロファイルの取得または削除中にエラーが発生しました。<br />アクセス要求のために Azure にアップロード中にエラーが発生しました。 |

### Target は、アクセスの要請に対して GDPR API にどのような応答を送信しますか？

データアクセスの要請への応答には、該当する訪問者の Target プロファイルの概要が含まれます。この戻り値はExperience CloudGDPR API に送信され、その結果、データ管理者に応答が送信されます。

Target のアクセス API 応答は次の例のようになります。

```
{ 
    "jobId":"12345AD43E", 
    ... 
    "products":["Target",...], 
    "companyContexts":[ 
        { 
            "namespace":"imsOrgID", 
            "value":"123456789@AdobeOrg" 
        }, 
        ... 
    ], 
    "userContexts":[ 
        { 
            ~"namespace":"ECID", 
            "namespaceId":4, 
            "type":"standard", 
            "value":"53792210477379708453829363835595041181" 
        } 
        And/OR: 
        { 
            ~"namespace":"tntId", 
            "namespaceId":9, 
            "type":"standard", 
            "value":"1234567890" 
        } 
        And/OR: 
        { 
            "namespace":"thirdPartyId", 
            "type":"target", 
            "value":"thirdPartyIdName" 
        }, 
        ... 
    ] 
} 
```

| フィールド | 説明 |
|--- |--- |
| jobId | Central GDPR API からの GDPR または CCPA のジョブ ID を示します。 |
| imsOrgID | お客様の会社の一意識別子を提供します。 |
| namespace | データソースとも呼ばれます。このトピックの「Target で顧客が GDPR または CCPA のアクセスおよび削除要求をおこなうには、どのような ID を利用できますか？」を参照してください。 |
| type | GDPR または CCPA データアクセスを要求した ID のタイプ。Target はいくつかの ID タイプを受け入れます。一部は標準的なものですが、その他は Target に特有のものです。 このトピックの「Target で顧客が GDPR または CCPA のアクセスおよび削除要求をおこなうには、どのような ID を利用できますか？」を参照してください。 |
| value | 名前空間／データソースの ID。指定できる値については、「Target で顧客が GDPR または CCPA のアクセスおよび削除要求をおこなうには、どのような ID を利用できますか？」を参照してください。 |
| 統合コード | Integration code は、データソースの分かりやすい名前で、データソース ID を使用するより簡単にデータソースの追跡を行うことができます。 |

プロファイルを識別するために複数の値が提示される場合、有効な識別子それぞれに 1 つのプロファイルのファイルがあるということです。1 つ以上のプロファイルファイルは、ターゲットプロファイル JSON 応答の形式で GDPR Central API を介して一元化された GDPR Azure Blob に送信されます。

サンプルの Target プロファイル JSON は次の例のようになります。

```
{"profileAttributes": 
 
"Sample_Parameter":{"value":"Gold Loyalty Status","modifiedAt":"2018-04-11T21:44:14.000-04:00"}, 
 
"user.ReturnTimeOfDay":{"value":"44.0","modifiedAt":"2018-04-11T21:44:14.000-04:00"}, 
 
"firstSessionStart":{"value":"1523497450602","modifiedAt":"2018-04-11T21:44:10.000-04:00"}, 
 
"user.sessionCountScript":{"value":"1","modifiedAt":"2018-04-11T21:44:14.000-04:00"} 
   } 
} 
```

下の表は、上の例のプロファイル JSON フィールドの説明を含みます。

| フィールド | 説明 |
|--- |--- |
| Sample_Parameter | Target プロファイル内の多数の情報が、データ管理者によってアップロードされるか、直接提供されます。この例では、プロファイル更新 API を利用して Target プロファイルにパラメーターがアップロードされました。詳しくは、[データを Target に送信する方法](/help/dev/before-implement/methods-to-get-data-into-target/methods-to-get-data-into-target.md)を参照してください。 |
| user.ReturnTimeOfDay | この標準フィールドには、ユーザーの最新の再来訪の時刻が含まれます。 |
| firstSessionStart | この標準フィールドには、ユーザーの最初のセッションが開始された時刻が含まれます。 |
| user.sessionCountScript | Target プロファイル内の多数の情報が、データ管理者によってアップロードされるか、直接提供されます。この例では、プロファイルスクリプトは、この訪問者がデータ管理者のサイトに対して実行したセッション数を増分しています。 詳しくは、[プロファイルスクリプト属性](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/profile-parameters.html)を参照してください。 |

>[!NOTE]
>
>このコードサンプルは、ターゲットプロファイル JSON を参照してください。 Target プロファイル内の多くのフィールドは標準的なものではありません。何が返ってくるかは、その特定の訪問者のプロファイルに含まれる情報に左右されます。

### Target は IP の不明化に対応していますか？

GDPR または CCPA 実装戦略の一環として IP を使用する場合、Target では IP の不明化に対応しています。 詳しくは、[プライバシー](privacy.md/#replacement-of-last-octet-of-ip-addresses)を参照してください。

### データがサードパーティに共有または販売されるのを防ぐために何らかの処理を行う必要がありますか？

Target では、顧客が Target のデータを直接をサードパーティに共有または販売することを許可していません。そのため、Target に対する販売のオプトアウトはありません。
