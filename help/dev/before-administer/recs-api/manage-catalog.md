---
title: API を使用してRecommendations カタログを管理する方法
description: Adobe Target API を使用してRecommendations カタログのエンティティを作成、更新、保存、取得および削除するために必要な手順です。
feature: APIs/SDKs, Recommendations, Administration & Configuration
kt: 3815
thumbnail: null
author: Judy Kim
exl-id: aea82607-cde4-456a-8dfb-2967badce455
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '857'
ht-degree: 0%

---

# API を使用したRecommendations カタログの管理

[Recommendations API を使用するための要件 ](/help/dev/before-administer/recs-api/overview.md#prerequisites) を満たしていることを確認しながら、JWT 認証フローを使用して [ アクセストークンを生成 ](/help/dev/before-administer/configure-authentication.md) し、[Adobe Developer Consoleで [!DNL Adobe Target] 管理 API を使用する方法を学びました ](https://developer.adobe.com/console/home)。

[Recommendations API](https://developer.adobe.com/target/administer/recommendations-api/) を使用して、Recommendations カタログのアイテムを追加、更新、削除できるようになりました。 その他のAdobe Target管理 API と同様に、Recommendations API には認証が必要です。

>[!NOTE]
>
>24 時間後に期限切れになるので、認証用にアクセストークンを更新する必要がある場合は、いつでも **[!UICONTROL IMS: JWT Generate + Auth via User Token]** リクエストを送信します。 手順については、[AdobeAPI 認証の設定 ](../configure-authentication.md) を参照してください。

![JWT3ff](assets/configure-io-target-jwt3ff.png)

続行する前に、[Recommendations Postman コレクション ](https://developer.adobe.com/target/administer/recommendations-api/#section/Postman) を取得します。

## エンティティ保存 API を使用した項目の作成と更新

商品ページで発生する CSV 商品フィードや Target リクエストではなく、API を使用してRecommendations商品データベースに入力するには、[Save Entities API](https://developer.adobe.com/target/administer/recommendations-api/#operation/saveEntities) を使用します。 このリクエストは、単一の Target 環境で項目を追加または更新します。 構文は次のとおりです。

```
POST https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities
```

例えば、保存エンティティを使用して、在庫や価格のしきい値など、特定のしきい値に達した場合に常にアイテムを更新し、アイテムにフラグを立て、アイテムが推奨されないようにすることができます。

1. **[!UICONTROL Target]**/**[!UICONTROL Setup]**/**[!UICONTROL Hosts]**/**[!UICONTROL CONTROL Environments]** に移動して、項目を追加または更新するターゲット環境 ID を取得します。

   ![SaveEntities1](assets/SaveEntities01.png)

1. `TENANT_ID` を確認し、前 `API_KEY` 確立したPostman環境変数を参照します。 比較のために以下の画像を使用してください。 必要に応じて、API リクエストのヘッダーとパスを変更して、以下の画像のヘッダーとパスに一致させます。

   ![SaveEntities3](assets/SaveEntities03.png)

1. JSON を **raw** コードとして **Body** に入力します。 `environment` 変数を使用して、環境 ID を必ず指定してください。 （以下の例では、環境 ID は 6781 です。）

   ![SaveEntities4.png](assets/SaveEntities04.png)

   以下は、Toaster Oven 製品の関連するエンティティ値に entity.id kit2001 を追加して、環境 6781 に送信する JSON のサンプルです。

   ```
       {
       "entities": [{
               "name": "Toaster Oven",
               "id": "kit2001",
               "environment": 6781,
               "categories": [
                   "housewares:appliances"
               ],
               "attributes": {
                   "inventory": 77,
                   "margin": 23,
                   "message": "crashing helicopter",
                   "pageUrl": "www.foobar.foo.com/helicopter.html",
                   "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                  "value": 19.2
               }
           }]
       }
   ```

1. 「**[!UICONTROL Send]**」をクリックします。 次の応答が届きます。

   ![SaveEntities5.png](assets/SaveEntities05.png)

   JSON オブジェクトは、複数の製品を送信するようにスケールできます。 例えば、この JSON は 2 つのエンティティを指定します。

   ```
       {
           "entities": [{
                   "name": "Toaster Oven",
                   "id": "kit2001",
                   "environment": 6781,
                   "categories": [
                       "housewares:appliances"
                   ],
                   "attributes": {
                       "inventory": 89,
                       "margin": 11,
                       "message": "Toaster Oven",
                       "pageUrl": "www.foobar.foo.com/helicopter.html",
                       "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                       "value": 102.5
                   }
               },
               {
                   "name": "Blender",
                   "id": "kit2002",
                   "environment": 6781,
                   "categories": [
                       "housewares:appliances"
                   ],
                   "attributes": {
                       "inventory": 36,
                       "margin": 5,
                       "message": "Blender",
                       "pageUrl": "www.foobar.foo.com/helicopter.html",
                       "thumbnailUrl": "www.foobar.foo.com/helicopter.jpg",
                       "value": 54.5
                   }
               }
           ]
       }
   ```

1. さあ、君の番だ。 **[!UICONTROL Save Entities]** API を使用して、次の項目をカタログに追加します。 上記のサンプル JSON を出発点として使用します。 （追加のエンティティを含めるには、JSON を拡張する必要があります）。

   ![SaveEntities6.png](assets/SaveEntities06.png)

これら最後の 2 つの項目は属していないようです。 **[!UICONTROL Get Entity]** API を使用して検査し、必要に応じて **[!UICONTROL Delete Entities]** API を使用して削除します。

## Get Entity API を使用した項目の詳細の取得

既存の項目の詳細を取得するには、[Get Entity API](https://developer.adobe.com/target/administer/recommendations-api/#operation/getEntity) を使用します。 構文は次のとおりです。

```
GET https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities/[entity.id]
```

エンティティの詳細は、一度に 1 つのエンティティに対してのみ取得できます。 「エンティティを取得」を使用して、カタログで更新が正常に行われたことを確認したり、カタログのコンテンツを監査したりできます。

1. API リクエストで、変数 `entityId` を使用してエンティティ ID を指定します。 次の例では、entityId=kit2004 を持つエンティティの詳細を返します。

   ![GetEntity1](assets/GetEntity1.png)

1. `TENANT_ID` を確認し、前 `API_KEY` 確立したPostman環境変数を参照します。 比較のために以下の画像を使用してください。 必要に応じて、API リクエストのヘッダーとパスを変更して、以下の画像のヘッダーとパスに一致させます。

   ![GetEntity2](assets/GetEntity2.png)

1. リクエストを送信します。

   ![GetEntity3](assets/GetEntity3.png)
上の例に示すように、「エンティティが見つかりませんでした」というエラーが表示された場合は、正しい Target 環境にリクエストを送信していることを確認します。



   >[!NOTE]
   >
   >環境が明示的に指定されていない場合、Get Entity は自分の [ デフォルト環境 ](https://experienceleague.adobe.com/docs/target/using/administer/environments.html) からのみエンティティの取得を試みます。 デフォルト環境以外の環境から取り込む場合は、環境 ID を指定する必要があります。

1. 必要に応じて、`environmentId` パラメーターを追加し、リクエストを再送信します。

   ![GetEntity4](assets/GetEntity4.png)

1. 別の **[!UICONTROL Get Entity]** リクエストを送信します。今回は、entityId=kit2005 を持つエンティティを調べます。

   ![GetEntity5](assets/GetEntity5.png)

これらのエンティティをカタログから削除する必要があると判断したとします。 **[!UICONTROL Delete Entities]** API を使用しましょう。

## エンティティ削除 API を使用した項目の削除

カタログから項目を削除するには、[ エンティティ削除 API](https://developer.adobe.com/target/administer/recommendations-api/#operation/deleteEntities) を使用します。 構文は次のとおりです。

```
DELETE https://mc.adobe.io/{{TENANT_ID}}/target/recs/entities?ids=[comma-delimited-entity-ids]&environment=[environmentId]
```

>[!WARNING]
>
>Delete Entities API は、指定した ID によって参照されているエンティティを削除します。 エンティティ ID が指定されていない場合、指定された環境内のすべてのエンティティが削除されます。 環境 ID が指定されていない場合、エンティティはすべての環境から削除されます。 これを使用する際は注意が必要です。

1. **[!UICONTROL Target]**/**[!UICONTROL Setup]**/**[!UICONTROL Hosts]**/**[!UICONTROL Environments]** に移動し、項目を削除するターゲット環境 ID を取得します。

   ![DeleteEntities1](assets/SaveEntities01.png)

1. API リクエストで、構文 `&ids=[comma-delimited-entity-ids]` （クエリパラメーター）を使用して、削除するエンティティのエンティティ ID を指定します。 複数のエンティティを削除する場合は、コンマを使用して ID を区切ります。

   ![DeleteEntities2](assets/DeleteEntities2.png)

1. 構文 `&environment=[environmentId]` を使用して環境 ID を指定します。指定しない場合は、すべての環境のエンティティが削除されます。

   ![DeleteEntities3](assets/DeleteEntities3.png)

1. `TENANT_ID` を確認し、前 `API_KEY` 確立したPostman環境変数を参照します。 比較のために以下の画像を使用してください。 必要に応じて、API リクエストのヘッダーとパスを変更して、以下の画像のヘッダーとパスに一致させます。

   ![DeleteEntities4](assets/DeleteEntities4.png)

1. リクエストを送信します。

   ![DeleteEntities5](assets/DeleteEntities5.png)

1. **[!UICONTROL Get Entity]** を使用して結果を確認します。これにより、削除されたエンティティが見つからないことが示されます。

   ![DeleteEntities6](assets/DeleteEntities6.png)

   ![DeleteEntities6](assets/DeleteEntities7.png)

おめでとうございます。 Recommendations API を使用して、カタログ内のエンティティの詳細を作成、更新、削除および取得できるようになりました。 次の節では、カスタム条件の管理方法について説明します。

&lt;!— [ 次の「カスタム条件の管理」 >](manage-custom-criteria.md) —>
