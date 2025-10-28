---
keywords: 実装，実装，許可リスト，ホワイトリスト，許可リスト, 許可リスト, edge, edge, $9
description: 許可リスト [!DNL Adobe Target]  エッジ（エンドユーザーの応答時間を最適化する地理的に分散されたサービングノード）に役立つホストのリストを表示します。
title: 'Edgeノードの許可リストを設定する方法 [!DNL Target] '
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
source-git-commit: 662d415bc3c216bcd038f07dcaa0fd83f6518690
workflow-type: tm+mt
source-wordcount: '343'
ht-degree: 0%

---

# エッジノード [!DNL Target]許可リスト

エッジの許可リストに役立つホストの情報と最新のリスト [!DNL Adobe Target] 表示されます。

エッジは、コンテンツをリクエストするエンドユーザーの場所に関係なく、最適な応答時間を確保する、地理的に分散された配信アーキテクチャです。 各エッジノードには、ユーザーのコンテンツリクエストに応答し、そのリクエストに関する分析データを追跡するために必要な情報がすべて含まれています。 ユーザーリクエストは、最寄りのエッジノードにルーティングされます。 詳しくは、[Edge ネットワーク ](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#concept_0AE2ED8E9DE64288A8B30FCBF1040934) を参照してください。

必要に応じて、[!DNL Target] のエッジノードを許可リストに加えるできます。

>[!IMPORTANT]
>
>この記事で取り上げた [!DNL Target] エッジおよび [!DNL Target] エッジ IP アドレスの Network Address Translation （NAT; ネットワーク アドレス変換） IP アドレスを許可リストに加えるするだけでなく、すべての [!DNL Adobe Analytics] IP アドレス ブロックを許可リストに加えるする必要もあります。
>
>詳しくは、[Adobe Analytics テクニカルノート ](https://experienceleague.adobe.com/docs/analytics/technotes/ip-addresses.html?lang=en#all-adobe-analytics-ip-address-blocks){target=_blank} ドキュメントの *すべてのAdobe Analytics IP アドレスブロック* を参照してください。
>
>インフラストラクチャ [!DNL Adobe Target] 更新中であり、アドレスを許可リストに加えるしたいお客様は両方の IP セットを使用する必要があります。 失敗すると、エクスペリエンスを取得するための Target API 呼び出しが、許可リストを使用するように設定されたファイアウォールの背後にあるネットワークから発信される、サーバーサイド実装またはハイブリッド実装を使用しているお客様に影響を与えます。

[!DNL Target] を介した [!DNL Experience Edge Connector] への中断のないアクセスを確保するために、プロキシサービスへのトラフィックを許可するようにネットワーク設定を更新できます。

## プロキシサービスの概要

* **サービス エンドポイント**: `https://tnt-web-proxy.adobe.io`。
* **インフラストラクチャ**:[!DNL Adobe] Ethos プラットフォームでホストされています。
* **注意**：このサービスは、待ち時間ベースの DNS ルーティングを使用しており、静的 IP アドレスには依存しません。

## CNAME ターゲット

プロキシサービスは、CNAME レコードを使用して、複数の地域にわたってトラフィックを動的にルーティングします。 現在のターゲットは次のとおりです。

| Edgeの場所 | エグレス IP アドレス |
| --- | --- |
| 地域 | CNAME ターゲット |
| ヨーロッパ （eu-west-1） | `ethos.pub.ethos11-prod-nld2.ethos.adobe.net` |
| 米国東部（us-east-2） | `ethos.pub.ethos11-prod-va7.ethos.adobe.net` |
| 米国東部（us-east-1） | `ethos.pub.ethos11-prod-aus5.ethos.adobe.net` |

## 推奨される許可リスト項目

確実な接続性を確保するには、次のホスト名を許可リストに加えるします。

* `ethos.pub.ethos11-prod-nld2.ethos.adobe.net`
* `ethos.pub.ethos11-prod-va7.ethos.adobe.net`
* `ethos.pub.ethos11-prod-aus5.ethos.adobe.net`

## オプション：IP 検出

ネットワークポリシーで IP ベースの許可リストに加えるが必要な場合は、このツールを使用して、プロキシサービスに関連付けられている現在のパブリック IP アドレスを確認できます。

* `DNSChecker – A Record Lookup for tnt-web-proxy.adobe.io`