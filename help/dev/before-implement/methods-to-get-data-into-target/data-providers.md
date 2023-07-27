---
keywords: 実装，実装，設定，設定，データプロバイダー
description: データをに取り込む [!DNL Target] データプロバイダーを使用する。
title: データをに取り込む方法 [!DNL Target] データプロバイダーを使用する場合
feature: Implementation
exl-id: 9971bd96-f736-4965-afe2-b4901c12d006
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '298'
ht-degree: 53%

---

# データプロバイダー

データプロバイダーは、サードパーティからにデータを簡単に渡すことができる機能です。 [!DNL Adobe Target].

>[!NOTE]
>
>データプロバイダーには at.js 1.3 以降が必要です。

## 形式

`window.targetGlobalSettings.dataProviders` 設定は、データプロバイダーの配列です。

各データプロバイダーの構造について詳しくは、[データプロバイダー](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)を参照してください。

## 使用例

サードパーティから、気象予報サービス、DMP、自社の Web サービスなどのデータを収集します。このデータを利用して、オーディエンスやターゲットコンテンツを構築したり、訪問者プロファイルを充実させることができます。

## 方法の利点

この設定を使用すると、Demandbase、BlueKai、カスタムサービスなどのサードパーティのデータプロバイダーからデータを収集し、そのデータをに渡すことができます。 [!DNL Target] を mbox パラメーターとしてグローバル mbox リクエストに追加します。

非同期および同期リクエストを介した複数のプロバイダーからのデータ収集もサポートしています。

この手法では、デフォルトのページコンテンツのちらつきを制御しながら、プロバイダーごとに個別のタイムアウトを指定し、ページのパフォーマンスへの影響を抑制することが簡単にできます。

## 注意事項

データプロバイダーが `window.targetGlobalSettings.dataProviders` が非同期の場合は、同時に実行されます。 訪問者 API リクエストは、に追加された関数と同時に実行されます。 `window.targetGlobalSettings.dataProviders` 待ち時間を最小限に抑える。

at.js はデータをキャッシュしません。 データプロバイダーが 1 回だけデータを取得する場合は、データをキャッシュし、そのプロバイダーの関数が呼び出されたら、2 回目の呼び出しでキャッシュデータを配信できるようにする必要があります。

## コードの例

[データプロバイダー](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)にはいくつかの例が記載されています。

## 関連情報へのリンク

ドキュメント：[データプロバイダー](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)

## トレーニングビデオ：

* [Adobe Target でのデータプロバイダーの使用](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html)
* [Adobe Target でのデータプロバイダーの実装](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html)
