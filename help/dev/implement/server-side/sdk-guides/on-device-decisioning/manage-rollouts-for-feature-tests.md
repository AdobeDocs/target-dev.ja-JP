---
title: 機能テストのロールアウトの管理
description: '[!UICONTROL on-device decisioning] を使用して機能テストのロールアウトを管理する方法を説明します。'
feature: APIs/SDKs
exl-id: caa91728-6ac0-4583-a594-0c8fe616342d
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '513'
ht-degree: 0%

---

# 機能テストのロールアウトの管理

## 手順の概要

1. 組織の [!UICONTROL on-device decisioning] を有効にする
1. [!UICONTROL A/B Test] アクティビティの作成
1. 機能の定義とロールアウト設定
1. アプリケーションでの機能の実装とレンダリング
1. アプリケーションにイベントのトラッキングを実装
1. A/B アクティビティのアクティベート
1. 必要に応じてロールアウトとトラフィック割り当てを調整する

## 1.組織の [!UICONTROL on-device decisioning] を有効にする

オンデバイス判定を有効にすることで、A/B アクティビティがほぼゼロの待ち時間で実行されるようになります。 この機能を有効にするには、[!DNL Adobe Target] で **[!UICONTROL Administration]**/**[!UICONTROL Implementation]**/**[!UICONTROL Account details]** に移動し、「**[!UICONTROL On-Device Decisioning]**」トグルを有効にします。

![alt 画像 ](assets/asset-odd-toggle.png)

>[!NOTE]
>
>[!UICONTROL On-Device Decisioning] の切り替えを有効または無効にするには、管理者または承認者 [ ユーザーの役割 ](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html?lang=ja) が必要です。

「[!UICONTROL On-Device Decisioning]」切替スイッチを有効 [!DNL Adobe Target] すると、クライアントの *ルールアーティファクト* の生成を開始します。

## 2. [!UICONTROL A/B Test] アクティビティの作成

1. [!DNL Adobe Target] で、**[!UICONTROL Activities]** ページに移動し、**[!UICONTROL Create Activity]**/**[!UICONTROL A/B test]** を選択します。

   ![alt 画像 ](assets/asset-ab.png)

1. **[!UICONTROL Create A/B Test Activity]** モーダルでは、デフォルトの **[!UICONTROL Web]** オプションを選択した状態（1）のままにし、experience composer として **[!UICONTROL Form]** を選択し（2）、**[!UICONTROL No Property Restrictions]** を使用して **[!UICONTROL Default Workspace]** を選択し（3）、**[!UICONTROL Next]** をクリックします（4）。

   ![alt 画像 ](assets/asset-form.png)

## 3.機能の定義とロールアウト設定

アクティビティ作成の **[!UICONTROL Experiences]** の手順で、アクティビティの名前を指定します（1）。 機能のロールアウトを管理するアプリケーション内の場所（2）の名前を入力します。 例えば、`ondevice-rollout` や `homepage-addtocart-rollout` は、機能ロールアウトを管理する宛先を示すロケーション名です。 次の例で、`ondevice-rollout` はエクスペリエンス A に対して定義された場所です。オプションで、オーディエンスの絞り込み（4）を追加して、アクティビティの選定を制限できます。

![alt 画像 ](assets/asset-location-rollout.png)

1. 同じページの「**[!UICONTROL Content]**」セクションで、次に示すように、ドロップダウンの「**[!UICONTROL Create JSON Offer]** （1）」を選択します。

   ![alt 画像 ](assets/asset-offer.png)

1. 表示される **[!UICONTROL JSON Data]** テキストボックスで、有効な JSON オブジェクト（2）を使用して、Experience A （1）でこのアクティビティとともにロールアウトする機能の機能フラグ変数を入力します。

   ![alt 画像 ](assets/asset-json-a-rollout.png)

1. **[!UICONTROL Next]** （1）をクリックして、アクティビティ作成の **[!UICONTROL Targeting]** のステップに進みます。

   ![alt 画像 ](assets/asset-next-2-t-rollout.png)

1. **[!UICONTROL Targeting]** の手順では、簡単にするために、**[!UICONTROL All Visitors]** オーディエンス（1）を残します。 ただし、トラフィック配分（2）を 10% に調整します。 これにより、機能がサイト訪問者の 10% のみに制限されます。 次へ（3）をクリックして、**[!UICONTROL Goals & Settings]** のステップに進みます。

   ![alt 画像 ](assets/asset-next-2-g-rollout.png)

1. **[!UICONTROL Goals & Settings]** の手順で、[!DNL Adobe Target] UI でアクティビティの結果を表示する **[!UICONTROL Reporting Source]** として **[!UICONTROL Adobe Target]** （1）を選択します。

1. アクティビティを測定する **[!UICONTROL Goal Metric]** を選択します。 この例では、ユーザーが orderConfirm （2）の場所に到達したかどうかで示されるように、ユーザーがアイテムを購入したかどうかに基づいてコンバージョンが成功します。

1. **[!UICONTROL Save & Close]** （3）をクリックして、アクティビティを保存します。

   ![alt 画像 ](assets/asset-conv-rollout.png)

## 4. アプリケーションで機能を実装しレンダリングする

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

## 5. アプリケーションにイベントのトラッキングを実装する

機能フラグ変数をアプリケーションで使用できるようにすると、既にアプリケーションの一部となっている任意の機能を有効にすることができます。 訪問者がアクティビティの対象とならない場合は、オーディエンスとして定義された 10% バケットに含まれていなかったことを意味します。

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

## 6. ロールアウトアクティビティの有効化

![alt 画像 ](assets/asset-activate-rollout.png)

## 7.必要に応じたロールアウトとトラフィック割り当ての調整

アクティビティをアクティブ化したら、いつでも編集して、必要に応じてトラフィックの割り当てを増減します。

最初のロールアウトが正常に行われるので、トラフィック割り当てを 10% から 50% に増やします。

![alt 画像 ](assets/asset-adjust-rollout.png)
