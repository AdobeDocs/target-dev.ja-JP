---
keywords: 実装，実装，設定，設定，データプロバイダー
description: データプロバイダ  [!DNL Target]  を使用してデータをに取り込みます。
title: データプロバイダーを使用してデータをに取  [!DNL Target]  込むにはどうすればよいですか？
feature: Implementation
exl-id: 9971bd96-f736-4965-afe2-b4901c12d006
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '287'
ht-degree: 55%

---

# データプロバイダー

データプロバイダーは、サードパーティから [!DNL Adobe Target] にデータを簡単に渡すことができる機能です。

>[!NOTE]
>
>データプロバイダーには at.js 1.3 以降が必要です。

## 形式

`window.targetGlobalSettings.dataProviders` 設定は、データプロバイダーの配列です。

各データプロバイダーの構造について詳しくは、[データプロバイダー](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)を参照してください。

## 使用例

サードパーティから、気象予報サービス、DMP、自社の Web サービスなどのデータを収集します。このデータを利用して、オーディエンスやターゲットコンテンツを構築したり、訪問者プロファイルを充実させることができます。

## メソッドの利点

この設定を使用すると、お客様は Demandbase、BlueKai、カスタムサービスなどのサードパーティデータプロバイダーからデータを収集し、そのデータをグローバル mbox リクエストの mbox パラメーターとして [!DNL Target] に渡すことができます。

非同期および同期リクエストを介した複数のプロバイダーからのデータ収集もサポートしています。

この手法では、デフォルトのページコンテンツのちらつきを制御しながら、プロバイダーごとに個別のタイムアウトを指定し、ページのパフォーマンスへの影響を抑制することが簡単にできます。

## 注意事項

`window.targetGlobalSettings.dataProviders` に追加されたデータプロバイダーが非同期の場合は、並行して実行されます。 訪問者 API リクエストは、`window.targetGlobalSettings.dataProviders` に追加された関数と並行して実行され、待機時間が最小限に抑えられます。

at.js はデータをキャッシュしようとしません。 データプロバイダーが 1 回だけデータを取得する場合は、データをキャッシュし、そのプロバイダーの関数が呼び出されたら、2 回目の呼び出しでキャッシュデータを配信できるようにする必要があります。

## コードの例

[データプロバイダー](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)にはいくつかの例が記載されています。

## 関連情報へのリンク

ドキュメント：[データプロバイダー](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)

## トレーニングビデオ：

* [Adobe Target でのデータプロバイダーの使用](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html?lang=ja)
* [Adobe Target でのデータプロバイダーの実装](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html?lang=ja)
