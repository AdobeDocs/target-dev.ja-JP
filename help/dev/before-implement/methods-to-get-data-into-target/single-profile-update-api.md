---
keywords: 実装，実装，設定，設定，単一プロファイルの更新
description: データをに取り込む [!DNL Target] 単一のプロファイル更新 API を使用する。
title: データをに取り込む方法 [!DNL Target] 単一プロファイル更新 API を使用する場合
feature: Implementation
exl-id: e6c394cb-74a3-4991-b656-5ae601f2d5e2
source-git-commit: 3ae2391dea9994c0ddc1df39d74cccf6e067c1a4
workflow-type: tm+mt
source-wordcount: '204'
ht-degree: 35%

---

# 単一プロファイル更新 API

ほぼ同じ [!UICONTROL プロファイル一括更新 API] in [!DNL Adobe Target]に変更された場合でも、API 呼び出しの行で、1 人の訪問者プロファイルが.csv ファイルではなく一度に更新されます。

## 形式

訪問者は、 の [!DNL Target]mboxPC または `mbox3rdPartyId` の値によって識別する必要があります。Experience Cloud ID（ECID）には対応していません。

## 使用例

何らかのオフラインアクションを実行する 1 人の訪問者のプロファイルを更新する必要がある場合。 アクションには、コールセンターへのリーチ、ローンの資金提供、店舗内のロイヤルティカードを使用したキオスクへのアクセスなどが含まれます。

## 方法の利点

プロファイル属性の数に上限がありません。

サイトを介して送信されるプロファイル属性を、API を介して更新できます（その逆も可能です）。

## 注意事項

24 時間ごとの API 呼び出しの上限は 100 万件です。

プロファイルのみが更新されます。潜在的なユーザーのプロファイルを作成できません [!DNL Target] はまだ見ていません。

通常、更新は 1 時間以内におこなわれますが、反映されるまでに 24 時間かかる場合があります。

## コードの例

GET および POST に対応しています。

```
https://CLIENT.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr1=0&profile.attr2=1...
```

## リソース

* [プロファイルの更新](https://developers.adobetarget.com/api/#updating-profiles)
