---
keywords: 実装、実装、ホワイトリスト、ホワイトリスト、許可リスト、エッジ、エッジ、9 ドル
description: ホストのリストを表示して、 [!DNL Adobe Target] edge （最適な応答時間を確保する地理的に分散されたサービスノード）を許可リストに加えるできます。
title: 'Edge ノードを許可リストに加えるするにはどうすればよいですか？ [!DNL Target] '
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
TQID: https://experienceleague.adobe.com/-XCVJpuvQ1xV9vQBZbomDKU3F-60b5FS-LU8lIBp4GQ
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2: id: a94ced60-8199-4549-b453-ede2acb4101e
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: d095671a-1355-40aa-8b5f-06c33c68080bid: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 373
ht-degree: 0%

---

# [!DNL Target]個のエッジ ノードを許可リストに加える

[!DNL Adobe Target] エッジを許可リストに加えるするのに役立つ情報と最新のホストのリスト。

エッジとは、コンテンツをリクエストするエンドユーザーの場所に関係なく、最適な応答時間を確保する、地理的に分散したサービスアーキテクチャのことです。 各エッジノードには、ユーザーのコンテンツリクエストに応答し、そのリクエストで分析データを追跡するために必要なすべての情報が含まれています。 ユーザーリクエストは、最も近いエッジノードにルーティングされます。 詳細については、[ エッジ ネットワーク ](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#concept_0AE2ED8E9DE64288A8B30FCBF1040934)を参照してください。

必要に応じて、[!DNL Target]個のエッジノードを許可リストに加えるできます。

>[!IMPORTANT]
>
>記事で説明した[!DNL Target] エッジと[!DNL Target] エッジ IP アドレスのNetwork Address Translation （NAT） IP アドレスを許可リストに加えるすることに加えて、[!DNL Adobe Analytics] IP アドレスブロックをすべて許可リストに加えるする必要があります。
>
>詳しくは、*Adobe Analytics テクニカルノート*&#x200B;のドキュメントの[すべてのAdobe Analytics IP アドレスブロック ](https://experienceleague.adobe.com/docs/analytics/technotes/ip-addresses.html?lang=en#all-adobe-analytics-ip-address-blocks){target=_blank}を参照してください。
>
>[!DNL Adobe Target]個のインフラストラクチャを更新中です。アドレスを許可リストに加えるするお客様は、両方のIP セットを使用する必要があります。 これを行わないと、エクスペリエンスの取得を行うためのTarget API呼び出しが、ファイアウォールの背後にあるネットワーク内から発生するサーバーサイド実装またはハイブリッド実装を使用しているお客様に影響が及びます。この実装は、ファイアウォールを使用するように設定されています。

[!DNL Experience Edge Connector]を介した[!DNL Target]への中断のないアクセスを確保するために、お客様はネットワーク設定を更新して、プロキシサービスへのトラフィックを許可できます。

## プロキシサービスの概要

* **サービス エンドポイント**: `https://tnt-web-proxy.adobe.io`。
* **インフラストラクチャ**: [!DNL Adobe] Ethos プラットフォームでホストされています。
* **メモ**：このサービスでは、レイテンシーベースのDNS ルーティングが使用されており、静的IP アドレスには依存しません。

## CNAME ターゲット

プロキシサービスは、CNAME レコードを使用して、複数の地域をまたいでトラフィックを動的にルーティングします。 現在の目標は次のとおりです。

| Edge場所 | エグレス IP アドレス |
| --- | --- |
| 地域 | CNAME ターゲット |
| 欧州（eu-west-1） | `ethos.pub.ethos11-prod-nld2.ethos.adobe.net` |
| 米国東部（us-east-2） | `ethos.pub.ethos11-prod-va7.ethos.adobe.net` |
| 米国東部（us-east-1） | `ethos.pub.ethos11-prod-aus5.ethos.adobe.net` |

## 推奨されるエントリの許可リストに加える

信頼性の高い接続性を確保するには、次のホスト名を許可リストに加えるします。

* `ethos.pub.ethos11-prod-nld2.ethos.adobe.net`
* `ethos.pub.ethos11-prod-va7.ethos.adobe.net`
* `ethos.pub.ethos11-prod-aus5.ethos.adobe.net`

## オプション：IP検出

ネットワークポリシーでIP ベースの許可リストに加えるが必要な場合は、次のツールを使用して、プロキシサービスに関連付けられている現在のパブリック IP アドレスを表示できます。

* `DNSChecker – A Record Lookup for tnt-web-proxy.adobe.io`