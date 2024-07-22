---
keywords: 実装，実装，許可リスト，ホワイトリスト，許可リスト, 許可リスト, edge, edge, $9
description: 許可リスト [!DNL Adobe Target]  エッジ（エンドユーザーの応答時間を最適化する地理的に分散されたサービングノード）に役立つホストのリストを表示します。
title: Edgeノードの許可リストを設定する方法 [!DNL Target]
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
source-git-commit: 49b6572c0d414ab304712691c97794bb0b1e3781
workflow-type: tm+mt
source-wordcount: '476'
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
>詳しくは、*Adobe Analytics テクニカルノート [ ドキュメントの ](https://experienceleague.adobe.com/docs/analytics/technotes/ip-addresses.html?lang=en#all-adobe-analytics-ip-address-blocks){target=_blank} すべてのAdobe Analytics IP アドレスブロック* を参照してください。
>
>インフラストラクチャ [!DNL Adobe Target] 更新中であり、アドレスを許可リストに加えるしたいお客様は両方の IP セットを使用する必要があります。 失敗すると、エクスペリエンスを取得するための Target API 呼び出しが、許可リストを使用するように設定されたファイアウォールの背後にあるネットワークから発信される、サーバーサイド実装またはハイブリッド実装を使用しているお客様に影響を与えます。
>
>以下の両方の表に記載されているEdge4 *x* のすべてのアドレスは、2023 年 8 月 9 日（PT）に更新される予定です。

## [!DNL Target] エッジのネットワーク アドレス変換（NAT） IP アドレス

[!DNL Target] エッジのエグレス IP アドレスのリスト。 サービスに連絡する予定がある場合 [!DNL Target]、これらの IP を許可リストに加えるします。

| Edgeの場所 | エグレス IP アドレス |
| --- | --- |
| Edge41 （ムンバイ） | 3.6.2.221<br />13.235.112.4 <br />52.66.66.192 |
| Edge42 （東京） | 52.69.55.232<br />43.206.61.43 <br />13.113.73.214 |
| Edge44 （米国東海岸） | 54.164.192.223<br />52.86.86.203 <br />54.88.167.98 |
| Edge45 （米国西海岸） | 52.40.124.129<br />54.148.219.69 <br />54.189.208.212 |
| Edge46 （シドニー） | 54.253.144.4<br />54.66.198.142 <br />13.211.218.51 |
| Edge47 （アイルランド） | 52.208.136.136<br />54.170.28.19 <br />99.80.111.82 |
| Edge48 （シンガポール） | 3.1.141.36<br />18.143.112.116 <br />52.76.61.44 |

## [!DNL Target] エッジ IP アドレス

[!DNL Target] Edge の IP アドレスのリスト。 [!DNL Target] Edge に対して API 呼び出しを行う場合は、これらの IP を許可リストに加えるします。

このリストは、ロードバランサーがトラフィックプロファイルに基づいてスケールアップまたはスケールダウンするため、頻繁に変更されます。

| Edgeの場所 | ドメイン | IP アドレス |
| --- | --- | --- |
|  | `CLIENTCODE.tt.omtrdc.net`<br /> （CLIENTCODE は [!DNL Target] クライアント ID） |  |
| Edge41 （ムンバイ） | `mboxedge41.tt.omtrdc.net` | 15.206.104.6<br />3.109.14.178 <br />13.234.139.131 |
| Edge42 （東京） | `mboxedge42.tt.omtrdc.net` | 52.194.84.34<br />3.115.158.39 <br />18.180.123.21 |
| Edge44 （米国東海岸） | `mboxedge44.tt.omtrdc.net` | 54.205.210.54<br />23.20.189.8 <br />35.169.173.155 |
| Edge45 （米国西海岸） | `mboxedge45.tt.omtrdc.net` | 35.161.163.45<br />44.230.114.101 <br />35.161.120.22 |
| Edge46 （シドニー） | `mboxedge46.tt.omtrdc.net` | 3.104.142.61<br />52.62.4.152 <br />54.253.105.140 |
| Edge47 （アイルランド） | `mboxedge47.tt.omtrdc.net` | 18.203.168.186<br />54.228.83.91 <br />54.217.181.83 |
| Edge48 （シンガポール） | `mboxedge48.tt.omtrdc.net` | 54.179.6.70<br />13.215.150.94 <br />18.136.47.70 |

ロードバランサーがトラフィックプロファイルの変更を検出すると、スケールアップまたはスケールダウンします。 Elastic Load Balancing のスケーリングに要する時間は、検出された変更に応じて 1～7 分の範囲です。 ロードバランサーは拡張を行うと、新しい IP アドレスのリストを使用して DNS レコードを更新します。 Elastic Load Balancing では、増加した容量を確実に活用するために、60 秒の DNS レコードに対して TTL 設定を使用します。
