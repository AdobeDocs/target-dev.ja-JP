---
keywords: 実装，実装，ホワイトリスト，ホワイトリスト，許可リストに加える許可リスト，エッジ，エッジ， $9
description: ホストのリストを表示して、に役立て許可リストに加えるる [!DNL Adobe Target] エッジ（エンドユーザーの応答時間を最適化する、地理的に分散された配信ノード）
title: 方法をす許可リストに加えるる [!DNL Target] エッジノード？
feature: Privacy & Security
exl-id: a7e5d2fc-da8e-414d-a3da-2441ea21503d
source-git-commit: 49b6572c0d414ab304712691c97794bb0b1e3781
workflow-type: tm+mt
source-wordcount: '476'
ht-degree: 0%

---

# 許可リストに加える [!DNL Target] エッジノード

に役立つ、ホストの情報と最新のリス許可リストに加えるト [!DNL Adobe Target] エッジ。

エッジとは、コンテンツを要求するエンドユーザーが、その場所に関係なく、最適な応答時間を確保できる、地理的に分散された配信アーキテクチャです。 各エッジノードには、ユーザーのコンテンツリクエストに応答し、そのリクエストに関する分析データを追跡するために必要なすべての情報が含まれています。 ユーザーリクエストは最も近いエッジノードにルーティングされます。 詳しくは、 [エッジネットワーク](https://experienceleague.adobe.com/docs/target/using/introduction/how-target-works.html#concept_0AE2ED8E9DE64288A8B30FCBF1040934).

次をできま許可リストに加えるす [!DNL Target] 必要に応じて、エッジノードに設定します。

>[!IMPORTANT]
>
>次の Network Address Translation（NAT；ネットワークアドレス変換）IP アドレスを許可リストに加えるする以外に、 [!DNL Target] エッジと [!DNL Target] エッジの IP アドレスについては、この記事で説明しています。すべての IP アドレスをする必要がありま許可リストに加えるす。 [!DNL Adobe Analytics] IP アドレスブロック。
>
>詳しくは、 [すべてのAdobe Analytics IP アドレスブロック](https://experienceleague.adobe.com/docs/analytics/technotes/ip-addresses.html?lang=en#all-adobe-analytics-ip-address-blocks){target=_blank} （内） *Adobe Analyticsテクニカルノート* ドキュメント。
>
>[!DNL Adobe Target] インフラストラクチャが更新中で、アドレスをするお客様は許可リストに加える、両方の IP セットを使用する必要があります。 これをおこなわないと、エクスペリエンスを取得するための Target API 呼び出しが、サーバーを使用するように設定されたファイアウォールの背後にあるネットワーク内から発生する、サーバー側またはハイブリッド実装を使用するお客様に影響しま許可リストに加えるす。
>
>すべての Edge4 *x* 以下の表に記載されているアドレスは、2023 年 8 月 9 日に更新される予定です。

## Network Address Translation（NAT；ネットワークアドレス変換）：の IP アドレス [!DNL Target] エッジ

のエグレス IP アドレスのリスト [!DNL Target] エッジ。 これ許可リストに加えるらの IP をする ( [!DNL Target] サービスに問い合わせてください。

| エッジの位置 | エグレス IP アドレス |
| --- | --- |
| Edge41（ムンバイ） | 3.6.2.221<br />13.235.112.4 <br />52.66.66.192 |
| Edge42（東京） | 52.69.55.232<br />43.206.61.43 <br />13.113.73.214 |
| Edge44（米国東海岸） | 54.164.192.223<br />52.86.86.203 <br />54.88.167.98 |
| Edge45（米国西海岸） | 52.40.124.129<br />54.148.219.69 <br />54.189.208.212 |
| Edge46（シドニー） | 54.253.144.4<br />54.66.198.142 <br />13.211.218.51 |
| Edge47（アイルランド） | 52.208.136.136<br />54.170.28.19 <br />99.80.111.82 |
| Edge48（シンガポール） | 3.1.141.36<br />18.143.112.116 <br />52.76.61.44 |

## [!DNL Target] エッジ IP アドレス

次の IP アドレスのリスト [!DNL Target] エッジ。 API 呼び出しを許可リストに加える実行する場合は、これらの IP をします。 [!DNL Target] エッジ。

ロードバランサーがトラフィックプロファイルに基づいてスケールアップおよびスケールダウンするにつれ、このリストは頻繁に変更されます。

| エッジの位置 | ドメイン | IP アドレス |
| --- | --- | --- |
|  | `CLIENTCODE.tt.omtrdc.net`<br />(CLIENTCODE は [!DNL Target] クライアント ID) |  |
| Edge41（ムンバイ） | `mboxedge41.tt.omtrdc.net` | 15.206.104.6<br />3.109.14.178 <br />13.234.139.131 |
| Edge42（東京） | `mboxedge42.tt.omtrdc.net` | 52.194.84.34<br />3.115.158.39 <br />18.180.123.21 |
| Edge44（米国東海岸） | `mboxedge44.tt.omtrdc.net` | 54.205.210.54<br />23.20.189.8 <br />35.169.173.155 |
| Edge45（米国西海岸） | `mboxedge45.tt.omtrdc.net` | 35.161.163.45<br />44.230.114.101 <br />35.161.120.22 |
| Edge46（シドニー） | `mboxedge46.tt.omtrdc.net` | 3.104.142.61<br />52.62.4.152 <br />54.253.105.140 |
| Edge47（アイルランド） | `mboxedge47.tt.omtrdc.net` | 18.203.168.186<br />54.228.83.91 <br />54.217.181.83 |
| Edge48（シンガポール） | `mboxedge48.tt.omtrdc.net` | 54.179.6.70<br />13.215.150.94 <br />18.136.47.70 |

ロードバランサーがトラフィックプロファイルの変更を検出すると、スケールアップまたはスケールダウンします。 Elastic Load Balancing のスケールに必要な時間は、検出された変更に応じて、1 ～ 7 分の範囲です。 ロードバランサーの規模を調整すると、DNS レコードが新しい IP アドレスのリストで更新されます。 Elastic Load Balancing では、容量の増加を確実に活用するために、60 秒の DNS レコードに対して TTL 設定が使用されます。
