---
title: Adobe Target Single Profile Update API
description: 使用方法を学ぶ [!DNL Adobe Target] [!UICONTROL 単一プロファイル更新 API] 1 人の訪問者のプロファイルデータをに送信する [!DNL Target].
feature: APIs/SDKs
contributors: https://github.com/icaraps
source-git-commit: 81bff85a9d1fe28ca267c471a470da95568fd06d
workflow-type: tm+mt
source-wordcount: '349'
ht-degree: 4%

---

# [!DNL Adobe Target Single Profile Update API]

The [!DNL Adobe Target] [!UICONTROL 単一プロファイル更新 API] を使用すると、1 人のユーザーに対するプロファイルの更新を送信できます。 The [!UICONTROL 単一プロファイル更新 API] は、 [!UICONTROL プロファイル一括更新 API]に変更された場合でも、訪問者プロファイルは一度に 1 つずつ更新され、 .cvs ファイルではなく API 呼び出しでインラインで更新されます。

The [!UICONTROL 単一プロファイル更新 API] およびは、通常、実装されていないチャネルで発生するトランザクションに関して更新が必要な場合に使用されます [!DNL Target]. 例えば、何らかのオフラインアクションを実行する 1 人の訪問者のプロファイルを更新するとします。 アクションには、コールセンターへのリーチ、ローンの資金提供、店舗内のロイヤルティカードを使用したキオスクへのアクセスなどが含まれます。

のメリット [!UICONTROL 単一プロファイル更新 API] 次を含む：

* プロファイル属性の数に上限がありません。
* サイトを介して送信されるプロファイル属性は、API を介して更新することも、逆の方法で更新することもできます。

## 注意事項

* The [!UICONTROL 単一プロファイル更新 API] は、24 時間に周期的に実行される更新の実行数が 100 万件に制限されています。
* 通常、更新は 1 時間以内におこなわれますが、反映されるまでに 24 時間かかる場合があります。

  より多くの更新を送信する場合や、より短い期間で更新を処理する必要がある場合は、クライアント側の更新（推奨）または [!DNL Adobe Target] サーバーサイド [配信 API](/help/dev/implement/delivery-api/overview.md).

## 形式

形式でのプロファイルパラメーターの指定 `profile.paramName=value`.

のプロファイルを更新するには `pcId`、用途：

``````
https://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr=0&profile.attr2=1...
``````

のプロファイルを更新するには `mbox3rdPartyId`、用途：

``````
shell http://<your-client-code>.tt.omtrdc.net/m2/client/profile/update?mbox3rdPartyId=123456&profile.attr=0&profile.attr2=1...
``````

The [!UICONTROL 単一プロファイル更新 API] は、更新専用です。 何も見つからない場合、プロファイルは作成されません。

## メモ

* パラメーターと値は、UTF-8 を使用して URL エンコードする必要があります。
* パラメーターの形式は `profile.paramName`.
* すべての pcIds と mbox3rdPartyIds に対して、すべてのパラメーター値が存在する必要はありません。
* パラメーターと値は、大文字と小文字を区別します。
* GETとPOSTの両方がサポートされます。
* 制限の現在のサイズ制限は、GETの場合は 8 KB、POSTの場合は 60 KB です。

## 応答

上記のリクエストの応答例は、次のようになります。

`trueRequest successfully submitted`

この応答は、応答が送信され、まもなく処理されることを示します。