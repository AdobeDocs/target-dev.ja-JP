---
title: 機能テストのロールアウトの管理
description: '[!UICONTROL  オンデバイス判定]を使用して、機能テストのロールアウトを管理する方法について説明します。'
feature: APIs/SDKs
exl-id: caa91728-6ac0-4583-a594-0c8fe616342d
TQID: https://experienceleague.adobe.com/soG8leVV3R4Y4FSns5oIJ43oziIhtOb2zJ5bkFYxeo0
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2: id: c93393a4-e558-47e1-992e-c91ed4d480ce
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2: id: aa2f3246-cb95-4b30-8899-fdf7d73550ccid: b5ce8718-c3af-4fdb-a1a9-fca32f83a87cid: e0eb8757-182f-49f3-94a4-1587d16f5094id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 596
ht-degree: 1%

---

# 機能テストのロールアウトの管理

## 手順の概要

1. 組織の[!UICONTROL  オンデバイス決定]を有効にする
1. [!UICONTROL A/B テスト ] アクティビティの作成
1. 機能とロールアウト設定の定義
1. アプリケーションに機能を実装してレンダリングする
1. アプリケーションにイベントのトラッキングを実装する
1. A/B アクティビティのアクティベート
1. 必要に応じて、ロールアウトとトラフィック配分を調整します

## &#x200B;1. 組織の[!UICONTROL  オンデバイス決定]を有効にする

オンデバイス判定を有効にすると、A/B アクティビティがほぼゼロの遅延で実行されます。 この機能を有効にするには、[!DNL Adobe Target]で&#x200B;**[!UICONTROL 管理]** > **[!UICONTROL 実装]** > **[!UICONTROL アカウントの詳細]**&#x200B;に移動し、**[!UICONTROL オンデバイス決定]** トグルを有効にします。

![alt画像](assets/asset-odd-toggle.png)

>[!NOTE]
>
>[!UICONTROL  オンデバイス決定] トグルを有効または無効にするには、管理者または承認者[ ユーザーの役割](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html)が必要です。

[!UICONTROL  オンデバイス決定] トグルを有効にすると、[!DNL Adobe Target]は、クライアントに対して&#x200B;*ルールアーティファクト*&#x200B;の生成を開始します。

## &#x200B;2. [!UICONTROL A/B テスト ] アクティビティの作成

1. [!DNL Adobe Target]で、**[!UICONTROL アクティビティ]** ページに移動し、**[!UICONTROL アクティビティの作成]** > **[!UICONTROL A/B テスト]**&#x200B;を選択します。

   ![alt画像](assets/asset-ab.png)

1. **[!UICONTROL A/B テスト アクティビティの作成]** モーダルで、デフォルトの&#x200B;**[!UICONTROL Web]** オプションを選択したままにし（1）、エクスペリエンス コンポーザーとして&#x200B;**[!UICONTROL Form]**&#x200B;を選択し（2）、**[!UICONTROL Default Workspace]**&#x200B;を&#x200B;**[!UICONTROL プロパティ制限なし]** （3）で選択し、**[!UICONTROL 次へ]** （4）をクリックします。

   ![alt画像](assets/asset-form.png)

## &#x200B;3. 機能とロールアウト設定の定義

アクティビティ作成の&#x200B;**[!UICONTROL エクスペリエンス]**&#x200B;手順で、アクティビティの名前を指定します（1）。 機能のロールアウトを管理するアプリケーション内の場所（2）の名前を入力します。 例えば、`ondevice-rollout`または`homepage-addtocart-rollout`は、機能ロールアウトの管理先を示す場所名です。 次の例では、`ondevice-rollout`はエクスペリエンス Aに定義された場所です。オプションで、オーディエンスの絞り込み（4）を追加して、アクティビティへの選定を制限できます。

![alt画像](assets/asset-location-rollout.png)

1. 同じページの「**[!UICONTROL コンテンツ]**」セクションで、図に示すように、ドロップダウン（1）で「**[!UICONTROL JSON オファーを作成]**」を選択します。

   ![alt画像](assets/asset-offer.png)

1. 表示される「**[!UICONTROL JSON データ]**」テキストボックスに、有効なJSON オブジェクト（2）を使用して、Experience A （1）でこのアクティビティでロールアウトする機能の機能フラグ変数を入力します。

   ![alt画像](assets/asset-json-a-rollout.png)

1. 「**[!UICONTROL 次へ]** （1）」をクリックして、アクティビティ作成の&#x200B;**[!UICONTROL ターゲティング]** ステップに進みます。

   ![alt画像](assets/asset-next-2-t-rollout.png)

1. **[!UICONTROL ターゲティング]**&#x200B;の手順では、簡単にするために&#x200B;**[!UICONTROL すべての訪問者]** オーディエンス（1）を維持します。 しかし、トラフィック配分（2）を10%に調整します。 これにより、機能はサイト訪問者の10%に制限されます。 「次へ（3）」をクリックして、**[!UICONTROL 目標と設定]** ステップに進みます。

   ![alt画像](assets/asset-next-2-g-rollout.png)

1. **[!UICONTROL 目標と設定]** ステップで、**[!UICONTROL レポート用Source]**&#x200B;として&#x200B;**[!UICONTROL Adobe Target]** （1）を選択し、[!DNL Adobe Target] UIでアクティビティの結果を表示します。

1. アクティビティを測定するには、**[!UICONTROL 目標指標]**&#x200B;を選択します。 この例では、コンバージョンの成功は、ユーザーがorderConfirm （2）の場所に到達したかどうかに示されるように、ユーザーが商品を購入したかどうかに基づいています。

1. 「**[!UICONTROL 保存して閉じる]**」（3）をクリックして、アクティビティを保存します。

   ![alt画像](assets/asset-conv-rollout.png)

## &#x200B;4. アプリケーションに機能を実装してレンダリングする

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
targetClient.getAttributes(["ondevice-rollout"]).then(function(attributes) {
      const featureFlags = attributes.asObject("ondevice-rollout");

      // Your flag variables are now available in the featureFlags object variable.
      //If you failed to qualify for the Activity, you will have an empty object.
      console.log(featureFlags);
    });
```

>[!TAB Java]

```java {line-numbers="true"}
    Attributes attrs = targetJavaClient.getAttributes(targetDeliveryRequest, "ondevice-rollout");
    Map<String, Object> featureFlags = attrs.toMboxMap("ondevice-rollout");
​
    // Your flag variables are now available in the featureFlags object variable.
    //If you failed to qualify for the Activity, you will have an empty object.
    System.out.println(featureFlags);
```

>[!ENDTABS]

## &#x200B;5. アプリケーションにイベントのトラッキングを実装する

機能フラグ変数をアプリケーションで使用できるようにした後、この変数を使用して、既にアプリケーションの一部となっている機能を有効にすることができます。 訪問者がアクティビティに適格でない場合は、オーディエンスとして定義された10% バケットの一部として訪問者が含まれていないことを意味します。

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
//... Code removed for brevity

if(featureFlags.enable == "yes") { //Fell within 10% traffic
    console.log("Render Feature");
}
else {
    console.log("Disable Feature");
}

// alternatively, the getValue method could be used on the Attributes object.

if(attributes.getValue("ondevice-rollout", "enable") === "yes") { //Fell within 10% traffic
    console.log("Render Feature");
}
else {
    console.log("Disable Feature");
}
```

>[!TAB Java]

```java {line-numbers="true"}
//... Code removed for brevity
​
if("yes".equals(String.valueOf(featureFlags.get("enable")))) { //Fell within 10% traffic
    System.out.println("Render Feature");
}
else {
    System.out.println("Disable Feature");
}
​
// alternatively, the getString method could be used on the Attributes object.
​
if("yes".equals(attrs.getString("ondevice-rollout", "enable"))) { //Fell within 10% traffic
    System.out.println("Render Feature");
}
else {
    System.out.println("Disable Feature");
}
```

>[!ENDTABS]

## &#x200B;6. ロールアウトアクティビティのアクティブ化

![alt画像](assets/asset-activate-rollout.png)

## &#x200B;7. 必要に応じて、ロールアウトとトラフィック配分を調整します

アクティビティをアクティベートしたら、いつでも編集して、必要に応じてトラフィック配分を増減できます。

最初のロールアウトの成功により、トラフィック割り当てが10%から50%に増加しました。

![alt画像](assets/asset-adjust-rollout.png)
