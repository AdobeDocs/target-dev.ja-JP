---
keywords: 実装、実装、設定、セットアップ、データプロバイダー
description: データプロバイダーを使用して [!DNL Target] にデータを取り込みます。
title: データプロバイダーを使用して [!DNL Target] にデータを取り込むにはどうすればよいですか？
feature: Implementation
exl-id: 9971bd96-f736-4965-afe2-b4901c12d006
TQID: https://experienceleague.adobe.com/e8uMaGcACjHiaIT4WSlbKry82mhLHUDTSKmCuuhoWgw
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
  - id: f7c7de77-382f-4f48-8b36-61a170f06d3d
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 314
ht-degree: 50%

---

# データプロバイダー

データプロバイダーは、サードパーティから[!DNL Adobe Target]にデータを簡単に渡すことができる機能です。

>[!NOTE]
>
>データプロバイダーが必要とするat.js 1.3以降

## 形式

`window.targetGlobalSettings.dataProviders` 設定は、データプロバイダーの配列です。

各データプロバイダーの構造について詳しくは、[データプロバイダー](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)を参照してください。

## 使用例

サードパーティから、気象予報サービス、DMP、自社の Web サービスなどのデータを収集します。 このデータを利用して、オーディエンスやターゲットコンテンツを構築したり、訪問者プロファイルを充実させることができます。

## メソッドの利点

この設定を使用すると、顧客はDemandbase、BlueKai、カスタム サービスなどのサードパーティのデータ プロバイダーからデータを収集し、グローバル mbox リクエストのmbox パラメーターとして[!DNL Target]にデータを渡すことができます。

非同期および同期リクエストを介した複数のプロバイダーからのデータ収集もサポートしています。

この手法では、デフォルトのページコンテンツのちらつきを制御しながら、プロバイダーごとに個別のタイムアウトを指定し、ページのパフォーマンスへの影響を抑制することが簡単にできます。

## 注意事項

`window.targetGlobalSettings.dataProviders`に追加されたデータプロバイダーが非同期である場合、それらは並行して実行されます。 訪問者API リクエストは、最小待機時間を許可するために`window.targetGlobalSettings.dataProviders`に追加された関数と並行して実行されます。

at.jsはデータをキャッシュしようとしません。 データプロバイダーが 1 回だけデータを取得する場合は、データをキャッシュし、そのプロバイダーの関数が呼び出されたら、2 回目の呼び出しでキャッシュデータを配信できるようにする必要があります。

## コード例

[データプロバイダー](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)にはいくつかの例が記載されています。

## 関連情報へのリンク

ドキュメント：[データプロバイダー](../../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#data-providers)

## トレーニングビデオ：

* [Adobe Target でのデータプロバイダーの使用](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/use-data-providers-to-integrate-third-party-data.html)
* [Adobe Target でのデータプロバイダーの実装](https://experienceleague.adobe.com/docs/target-learn/tutorials/integrations/implement-data-providers-to-integrate-third-party-data.html)
