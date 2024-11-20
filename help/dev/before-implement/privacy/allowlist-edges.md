---
keywords: 実装，実装，許可リスト，ホワイトリスト，許可リスト, 許可リスト, edge, edge, $9
description: 許可リスト [!DNL Adobe Target]  エッジ（エンドユーザーの応答時間を最適化する地理的に分散されたサービングノード）に役立つホストのリストを表示します。
title: Edgeノードの許可リストを設定する方法 [!DNL Target]
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
source-git-commit: 1583cfe8ea1009fd1df5c5a0e5f3f95f72daf2b9
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
| Edge41 （ムンバイ） | 3.6.2.221<P>13.235.112.4 <P>52.66.66.192 |
| Edge42 （東京） | 52.69.55.232<P>43.206.61.43 <P>13.113.73.214 |
| Edge44 （米国東海岸） | 54.164.192.223<P>52.86.86.203 <P>54.88.167.98 |
| Edge45 （米国西海岸） | 52.40.124.129<P>54.148.219.69 <P>54.189.208.212 |
| Edge46 （シドニー） | 54.253.144.4<P>54.66.198.142 <P>13.211.218.51 |
| Edge47 （アイルランド） | 52.208.136.136<P>54.170.28.19 <P>99.80.111.82 |
| Edge48 （シンガポール） | 3.1.141.36<P>18.143.112.116 <P>52.76.61.44 |

## [!DNL Target] エッジ IP アドレス

[!DNL Target] Edge の IP アドレスのリスト。 [!DNL Target] Edge に対して API 呼び出しを行う場合は、これらの IP を許可リストに加えるします。

このリストは、ロードバランサーがトラフィックプロファイルに基づいてスケールアップまたはスケールダウンするため、頻繁に変更されます。

| Edgeの場所 | ドメイン | IP アドレス |
| --- | --- | --- |
|  | `CLIENTCODE.tt.omtrdc.net`<br /> （CLIENTCODE は [!DNL Target] クライアント ID） |  |
| Edge41 （ムンバイ） | `mboxedge41.tt.omtrdc.net` | 3.6.2.221<P>52.66.66.192<P>13.235.112.4 |
| Edge42 （東京） | `mboxedge42.tt.omtrdc.net` | 43.206.61.43<P>13.113.73.214<P>52.69.55.232 |
| Edge44 （米国東海岸） | `mboxedge44.tt.omtrdc.net` | 54.88.167.98<P>54.164.192.223<P>52.86.86.203 |
| Edge45 （米国西海岸） | `mboxedge45.tt.omtrdc.net` | 52.40.124.129<P>54.148.219.69<P>54.189.208.212 |
| Edge46 （シドニー） | `mboxedge46.tt.omtrdc.net` | 54.66.198.142<P>54.253.144.4<P>13.211.218.51 |
| Edge47 （アイルランド） | `mboxedge47.tt.omtrdc.net` | 54.170.28.19<P>52.208.136.136<P>99.80.111.82 |
| Edge48 （シンガポール） | `mboxedge48.tt.omtrdc.net` | 52.76.61.44<P>3.1.141.36<P>18.143.112.116 |

ロードバランサーがトラフィックプロファイルの変更を検出すると、スケールアップまたはスケールダウンします。 Elastic Load Balancing のスケーリングに要する時間は、検出された変更に応じて 1～7 分の範囲です。 ロードバランサーは拡張を行うと、新しい IP アドレスのリストを使用して DNS レコードを更新します。 Elastic Load Balancing では、増加した容量を確実に活用するために、60 秒の DNS レコードに対して TTL 設定を使用します。
