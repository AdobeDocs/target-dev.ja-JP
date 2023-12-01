---
keywords: 実装，実装，設定，設定，単一プロファイルの更新
description: データをに取り込む [!DNL Target] 単一のプロファイル更新 API を使用する。
title: データをに取り込む方法 [!DNL Target] 単一プロファイル更新 API を使用する場合
feature: Implementation
exl-id: e6c394cb-74a3-4991-b656-5ae601f2d5e2
source-git-commit: af9db32d59bdf32f2b9fade267922803250377dd
workflow-type: tm+mt
source-wordcount: '198'
ht-degree: 9%

---

# 単一プロファイル更新 API

ほぼ同じ [!UICONTROL プロファイル一括更新 API] in [!DNL Adobe Target]に変更された場合でも、API 呼び出しの行で、1 人の訪問者プロファイルが.csv ファイルではなく一度に更新されます。

## 形式

訪問者は、 [!DNL Target] `mboxPC` 値または `mbox3rdPartyId` の値です。 The [!UICONTROL Experience CloudID] (ECID) はサポートされていません。

## 使用例

何らかのオフラインアクションを実行する 1 人の訪問者のプロファイルを更新する必要がある場合。 アクションには、コールセンターへのリーチ、ローンの資金提供、店舗内のロイヤルティカードを使用したキオスクへのアクセスなどが含まれます。

## 方法の利点

* プロファイル属性の数に上限がありません。*
* サイトを介して送信されるプロファイル属性は、API を介して更新することも、逆の方法で更新することもできます。

## 注意事項

* 24 時間あたりの API 呼び出し数の上限は 1,000,000 件（100 万件）です。
* プロファイルのみが更新されます。潜在的なユーザーのプロファイルを作成できません [!DNL Target] はまだ見ていません。
* 通常、更新は 1 時間以内におこなわれますが、反映されるまでに 24 時間かかる場合があります。

## コードの例

GET および POST に対応しています。

```
https://CLIENT.tt.omtrdc.net/m2/client/profile/update?mboxPC=1368007744041-575948.01_00&profile.attr1=0&profile.attr2=1...
```

## リソース

* [[!DNL Adobe Target Profile APIs overview]](/help/dev/administer/profile-api/profile-api-overview.md)
* [[!DNL Adobe Target Single Profile Update API]](/help/dev/administer/profile-api/profile-single-api.md)
* [[!DNL Adobe Target Bulk Profile Update API]](/help/dev/administer/profile-api/profile-bulk-api.md)
