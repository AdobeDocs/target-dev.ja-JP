---
keywords: at.js，ブラウザーユーザーエージェント，ユーザーエージェント，クライアントヒント，ユーザーエージェント
description: Adobe Targetがユーザーエージェントとクライアントヒントを使用して、訪問者をセグメント化とパーソナライゼーションの対象として認定する方法について説明します。
title: User Agent と Client Hints
feature: at.js
exl-id: e0d87d95-ee95-4ca9-8632-222ae1fb9a91
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '1208'
ht-degree: 74%

---

# User-agent と Client Hints

Adobe Targetは、ユーザーエージェントを使用して、訪問者をセグメント化とパーソナライゼーションの対象に認定します。

>[!NOTE]
>
>この記事の情報は、[at.js バージョン 2.9.0](target-atjs-versions.md)（またはそれ以降）に適用されます。

Web ブラウザーがサーバーにリクエストをおこなうたびに、ブラウザーと、ブラウザーが実行される環境に関する情報がリクエストのヘッダーに含まれます。 インターネットの初期の頃から、このデータは、user-agent と呼ばれる単一の文字列で集計されています。

以下のテキストに、Safari ブラウザーを使用する Mac OS X ベースのコンピューターの user-agent の例を示します。

```
Mozilla/5.0 (Linux; Android 12; SM-S908E) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.41 Mobile Safari/537.36 
```

リクエストを受け取ったサーバーは、この user-agent から、ブラウザーおよびオペレーティングシステムに関する以下の情報を識別できます。

| 情報 | 詳細 |
| --- | --- |
| ソフトウェア名 | Chrome |
| ソフトウェアのバージョン | 101 |
| 完全なソフトウェアのバージョン | 101.0.4951.41 |
| レイアウトエンジン名 | AppleWebKit |
| レイアウトエンジンのバージョン | 537.36 |
| オペレーティングシステム | Android |
| オペレーティングシステムのバージョン | Android 12（Snow Cone） |
| デバイス | SM-S908E（Samsung Galaxy S22 Ultra） |

user-agent 文字列に含まれるブラウザーおよびデバイス情報の量は、年々増加しています。

## User-agent の使用例

ユーザーエージェントは、長い間、マーケティングチームや開発者チームに、ブラウザー、オペレーティングシステム、デバイスでのサイトコンテンツの表示方法や、ユーザーの Web サイトとのやり取りに関する重要なインサイトを提供してきました。 また、User-agents は、スパムのブロックやサイトをクロールするボットのフィルタリングなど、様々な追加の目的にも使用されます。

しかし、近年、一部のサイト所有者やマーケティングベンダーは、リクエストヘッダーに含まれる他の情報と共に user-agent を使用して、ユーザーが知らないうちにユーザーを特定する手段として使用できる、デジタル指紋を作成しています。サイト所有者にとって user-agent が重要な目的を果たすにもかかわらず、ブラウザー開発者は、サイト訪問者に関する潜在的なプライバシーの問題を制限するために、user-agents の動作方法の変更を決定しました。

ブラウザ開発者は、この課題の解決策として User-Agent Client Hints を作成しました。 クライアントヒントを使用すると、サイトで必要なブラウザー、オペレーティングシステム、デバイス情報を収集できるほか、フィンガープリントなどのコーバートトラッキング方法に対する保護も強化できます。

## クライアントヒント

User-Agent Client Hints は、web サイト所有者に、user-agent で利用できるのとほぼ同じ量の情報にアクセスする機能を、よりプライバシーが保護された方法で提供します。モダンブラウザーが web サーバーに user-agent を送信する場合、それが必要かどうかにかかわらず、すべてのリクエストで user-agent 全体が送信されます。一方、Client Hints は、サーバーがクライアントについて知りたい追加情報をブラウザーに問い合わせる必要があるモデルを強制します。このリクエストを受け取ったブラウザーは、独自のポリシーやユーザー設定を適用して、どのデータが返されるかを決定できます。すべてのリクエストに対してデフォルトで user-agent 全体を公開する代わりに、明示的で監査可能な方法でアクセスが管理されるようになりました。

User-Agent Client Hints は、Chrome バージョン 89 以降で利用できます。Chromium ベースのブラウザー（Microsoft Edge、Opera、Brave、Chrome Android、Opera Android および Samsung Internet）の最近のバージョンも、Client Hints API をサポートしています。

ブラウザーから web サーバーに送信される最初のリクエストのヘッダーに含まれる Client Hints には、ブラウザーのブランド、ブラウザーのメジャーバージョン、クライアントがモバイルデバイスであるかどうかのインジケーターが含まれます。データの各部分には、1 つのユーザーエージェント文字列にグループ化されるのではなく、独自のヘッダー値があります。

例えば、次にクライアントヒントを示します。

```
Sec-CH-UA: "Chromium";v="101", "Google Chrome";v="101", " Not;A Brand";v="99" 
Sec-CH-UA-Mobile: ?0 
Sec-CH-UA-Platform: "macOS"
```

これは同じブラウザーのユーザーエージェントです。

```
Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.54 Safari/537.36 
```

情報は似ていますが、サーバーへの最初のリクエストには、user-agent 文字列で利用可能なもののサブセットのみを含む Client Hints が含まれています。リクエストに含まれていないのは、OS アーキテクチャ、完全な OS のバージョン、レイアウトエンジン名、レイアウトエンジンのバージョンおよび完全なブラウザーのバージョンです。ただし、その後のリクエストでは、Client Hints API を使用すると、web サーバーは、デバイスに関する追加の高エントロピーの詳細を依頼できます。これらの高エントロピー値がリクエストされた場合、ブラウザーのポリシーやユーザー設定によっては、ブラウザーの応答にその情報が含まれることがあります。

以下に、高エントロピー値がリクエストされた場合に Client Hints API によって返される JSON オブジェクトの例を示します。

```
{ 

    "architecture":"x86", 
    "bitness":"64", 
    "brands":[ 
        { 
            "brand":" Not A;Brand", 
            "version":"99" 
        }, 
        { 
            "brand":"Chromium", 
            "version":"100" 
        }, 
        { 
            "brand":"Google Chrome", 
            "version":"100" 
        } 
    ], 
    "fullVersionList":[ 
        { 
            "brand":" Not A;Brand", 
            "version":"99.0.0.0" 
        }, 
        { 
            "brand":"Chromium", 
            "version":"100.0.4896.127" 
        }, 
        { 
            "brand":"Google Chrome", 
            "version":"100.0.4896.127" 
        } 
    ], 
    "mobile":false, 
    "model":"", 
    "platformVersion":"12.2.1" 
} 
```

高エントロピー値には、デフォルトの Client Hints 情報では利用できない、いくつかの追加情報が含まれます。以下の表に、デフォルトのリクエストと高エントロピーの User-Agent Client Hints 情報で利用できるデータの比較を示します。

| HTTP ヘッダー | JavaScript | User-agent | Client hint | 高エントロピーの Client hint |
| --- | --- | --- | --- | --- |
| Sec-CH-UA | brands | ○ | ○ | × |
| Sec-CH-UA-Platform | platform | ○ | ○ | × |
| Sec-CH-UA-Mobile | mobile | ○ | ○ | × |
| Sec-CH-UA-Platform-Version | platformVersion | ○ | × | ○ |
| Sec-CH-UA-Arch | architecture | ○ | × | ○ |
| Sec-CH-UA-Model | model | ○ | × | ○ |
| Sec-CH-UA-Bitness | ビット数 | ○ | × | ○ |
| Sec-CH-UA-Full-Version-List | fullVersionList | ○ | × | ○ |

## Client Hints への移行

現在、Chromium ベースのブラウザーは、web サーバーに対するリクエストのヘッダーで、Client Hints と共に user-agent を送信し続けています。ただし、2022年4月から2023年2月までは、user-agent 文字列に含まれるデータ量が削減されます。Safari や Firefox などの他のブラウザーは、引き続き user-agent 文字列を利用しますが、そこに含まれる情報量は削減される予定です。

## クライアントヒントが必要な Target の使用例

以下に、Client Hints を必要とする Target の使用例を示します。

### オーディエンス属性

Target オーディエンスを使用し、次のオーディエンス属性のいずれかを使用する場合、Target では、正しいセグメント化を実行するためにクライアントヒントが必要です。

* ブラウザー
* オペレーティングシステム
* モバイル

### プロファイルスクリプト

プロファイルスクリプトを使用して、（user-agent を参照する） `user.browser` 属性を参照する場合、1 つ以上の Client Hints も確認するようにプロファイルスクリプトを更新する必要がある可能性があります。関数 `user.clientHint('sec-ch-ua-xxxxx')` を使用して、任意の Client Hints にアクセスすることもできます。Client Hint ヘッダー名は、すべて小文字である必要があります。

以下の例に、プロファイルスクリプトで Windows OS を適切に検出する方法を示します。

```
"return (((user.browser != null) && (user.browser.indexOf(\"Windows\") > -1)) || " + 
      "((user.clientHint('sec-ch-ua-platform') != null) && 
(user.clientHint('sec-ch-ua-platform') === 'Windows')));" 
```

次に、クライアントヒントと対応するプロファイルスクリプト使用セマンティクスの表を示します。

| クライアントヒントヘッダー | エントロピー | オーディエンス属性 | プロファイルスクリプトの使用状況 |
| --- | --- | --- | --- |
| [Sec-CH-UA](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA) | 低 | ブラウザー | `user.clientHint('sec-ch-ua')` |
| [Sec-CH-UA-Arch](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Arch) | 高 | プロファイルスクリプトを使用したユーザーへの公開 | `user.clientHint('sec-ch-ua-arch')` |
| [Sec-CH-UA-Bitness](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Bitness) | 高 | プロファイルスクリプトを使用したユーザーへの公開 | `user.clientHint('sec-ch-ua-bitness')` |
| [Sec-CH-UA-Full-Version-List](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Full-Version-List) | 高 | ブラウザー | `user.clientHint('sec-ch-ua-full-version-list')` |
| [Sec-CH-UA-Mobile](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Mobile) | 低 | モバイル | `user.clientHint('sec-ch-ua-mobile')` |
| [Sec-CH-UA-Model](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Model) | 高 | モバイル | `user.clientHint('sec-ch-ua-model')` |
| [Sec-CH-UA-Platform](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Platform) | 低 | オペレーティングシステム | `user.clientHint('sec-ch-ua-platform')` |
| [Sec-CH-UA-Platform-Version](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Sec-CH-UA-Platform-Version) | 高 | プロファイルスクリプトを使用したユーザーへの公開 | `user.clientHint('sec-ch-ua-platform-version')` |

## クライアントヒントをAdobe Targetに渡す方法

以下の節では、Target 実装に応じて、クライアントヒントを渡す方法の詳細について説明します。

### at.js バージョン 2.9.0（またはそれ以降）

at.js 2.9.0 以降、ユーザーエージェントクライアントヒントは、ブラウザーから自動的に収集され、 `getOffer/getOffers()` が呼び出されます。 デフォルトでは、at.js は、「低エントロピー」の Client Hints のみを収集します。前の節で「高エントロピー」に分類されたデータに基づいてオーディエンスセグメンテーションを実行したり、プロファイルスクリプトを使用したりする場合、`targetGlobalSettings` を使用してブラウザーから「高エントロピー」の Client Hints を収集するように at.js を設定する必要があります。

```
window.targetGlobalSettings = { allowHighEntropyClientHints: true };
```

### サーバーサイド SDK

サーバー側 SDK を使用してクライアントヒントを渡す方法について詳しくは、 [クライアントヒント](../../server-side/sdk-guides/core-principles/audience-targeting.md#client-hints) （サーバー側実装ドキュメント）を参照してください。
