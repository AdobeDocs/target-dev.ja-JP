---
title: 機能テストのロールアウトを管理
description: を使用して機能テストのロールアウトを管理する方法を説明します。 [!UICONTROL オンデバイス判定].
feature: APIs/SDKs
exl-id: caa91728-6ac0-4583-a594-0c8fe616342d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '556'
ht-degree: 0%

---

# 機能テストのロールアウトを管理

## 手順の概要

1. 有効にする [!UICONTROL オンデバイス判定] （組織の）
1. の作成 [!UICONTROL A/B テスト] アクティビティ
1. 機能とロールアウトの設定を定義する
1. アプリケーションで機能を実装してレンダリングする
1. アプリケーションでのイベントのトラッキングの実装
1. A/B アクティビティをアクティブ化
1. 必要に応じてロールアウトとトラフィックの割り当てを調整

## 1.有効にする [!UICONTROL オンデバイス判定] （組織の）

オンデバイス判定を有効にすると、A/B アクティビティがほぼゼロの待ち時間で実行されます。 この機能を有効にするには、次に移動します。 **[!UICONTROL 管理]** > **[!UICONTROL 実装]** > **[!UICONTROL アカウントの詳細]** in [!DNL Adobe Target]、を有効にします。 **[!UICONTROL オンデバイス判定]** 切り替え

![代替画像](assets/asset-odd-toggle.png)

>[!NOTE]
>
>管理者または承認者が必要です [ユーザーロール](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) 有効または無効にするには [!UICONTROL オンデバイス判定] 切り替え

を有効にした後 [!UICONTROL オンデバイス判定] トグル、 [!DNL Adobe Target] 生成を開始 *ルールアーティファクト* を設定します。

## 2. [!UICONTROL A/B テスト] アクティビティ

1. In [!DNL Adobe Target]をクリックし、 **[!UICONTROL アクティビティ]** ページ、「 」を選択します。 **[!UICONTROL アクティビティを作成]** > **[!UICONTROL A/B テスト]**.

   ![代替画像](assets/asset-ab.png)

1. Adobe Analytics の **[!UICONTROL A/B テストアクティビティの作成]** モーダルのままにします。デフォルトのままにします。 **[!UICONTROL Web]** オプションが選択されている (1)、 **[!UICONTROL フォーム]** experience composer (2) として、「 」を選択します。 **[!UICONTROL デフォルトのワークスペース]** 次を使用 **[!UICONTROL プロパティの制限がありません]** (3) をクリックし、 **[!UICONTROL 次へ]** (4)。

   ![代替画像](assets/asset-form.png)

## 3.機能とロールアウトの設定を定義する

Adobe Analytics の **[!UICONTROL エクスペリエンス]** アクティビティの作成手順。アクティビティの名前を指定します (1)。 機能のロールアウトを管理するアプリケーション内の場所 (2) の名前を入力します。 例：  `ondevice-rollout` または `homepage-addtocart-rollout` は、機能ロールアウトを管理する宛先を示す場所名です。 次の例では、 `ondevice-rollout` は、エクスペリエンス A に対して定義された場所です。オプションで、オーディエンスの絞り込み条件 (4) を追加して、認定をアクティビティに制限できます。

![代替画像](assets/asset-location-rollout.png)

1. Adobe Analytics の **[!UICONTROL コンテンツ]** セクションで、「 **[!UICONTROL JSON オファーを作成]** を (1) に設定します。

   ![代替画像](assets/asset-offer.png)

1. Adobe Analytics の **[!UICONTROL JSON データ]** 表示されるテキストボックスに、有効な JSON オブジェクト (2) を使用して、Experience A(1) でこのアクティビティでロールアウトする機能の機能フラグ変数を入力します。

   ![代替画像](assets/asset-json-a-rollout.png)

1. クリック **[!UICONTROL 次へ]** (1) に進む **[!UICONTROL ターゲット設定]** アクティビティ作成のステップ。

   ![代替画像](assets/asset-next-2-t-rollout.png)

1. Adobe Analytics の **[!UICONTROL ターゲット設定]** ステップ、 **[!UICONTROL すべての訪問者]** audience (1)。簡潔にします。 ただし、トラフィックの配分 (2) を 10%に調整します。 これにより、サイト訪問者の 10%にのみ機能が制限されます。 「次へ」(3) をクリックして、 **[!UICONTROL 目標と設定]** 手順

   ![代替画像](assets/asset-next-2-g-rollout.png)

1. Adobe Analytics の **[!UICONTROL 目標と設定]** ステップ、選択 **[!UICONTROL Adobe Target]** (1) **[!UICONTROL レポートソース]** アクティビティの結果を [!DNL Adobe Target] UI

1. を選択します。 **[!UICONTROL 目標指標]** を使用して、アクティビティを測定します。 この例では、ユーザーが orderConfirm(2) の場所に到達したかどうかで示されるように、コンバージョンが成功するのは、ユーザーが品目を購入したかどうかに基づきます。

1. クリック **[!UICONTROL 保存して閉じる]** (3) をクリックして、アクティビティを保存します。

   ![代替画像](assets/asset-conv-rollout.png)

## 4.アプリケーションで機能を実装してレンダリングする

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

## 5.アプリケーションでのイベントのトラッキングの実装

機能フラグ変数をアプリケーションで使用可能にした後、既にアプリケーションに含まれている任意の機能を有効にすることができます。 訪問者がアクティビティの対象にならない場合、その訪問者はオーディエンスとして定義された 10%バケットの一部に含まれていなかったことを意味します。

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

## 6.ロールアウトアクティビティをアクティブにする

![代替画像](assets/asset-activate-rollout.png)

## 7.必要に応じてロールアウトとトラフィックの配分を調整します

アクティビティを有効にしたら、必要に応じて、いつでもアクティビティを編集してトラフィックの配分を増減できます。

最初のロールアウトの成功により、トラフィックの配分を 10%から 50%に増やします。

![代替画像](assets/asset-adjust-rollout.png)
