---
title: 機能フラグとオンデバイス判定を使用して A/B テストを実行します。
description: オンデバイス判定を使用して、機能フラグを指定して A/B テストを実行します。
feature: APIs/SDKs
exl-id: abf66e00-742d-4d40-9b6e-9bd71638c31a
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '726'
ht-degree: 0%

---

# 機能フラグを使用した A/B テストの実行

## 手順の概要

1. 組織の [!UICONTROL on-device decisioning] を有効にする
1. [!UICONTROL A/B Test] アクティビティの作成
1. A と B の定義
1. オーディエンスを追加
1. トラフィック配分の設定
1. トラフィック配分をバリエーションに設定
1. レポートの設定
1. KPI を追跡するための指標の追加
1. 機能フラグを使用して A/B テストを実行するコードの実装
1. 機能フラグを使用した A/B テストのアクティブ化

>[!NOTE]
>
>ホームページの秋をテーマにしたデザインの修正がユーザーに好評かどうかを判断するとします。 [!DNL Adobe Target] で A/B 実験を実行して、テストを行うことにします。 また、ユーザーエクスペリエンスがマイナスになったり遅くなったりしても結果がゆがんだりしないように、実験が優れたパフォーマンスで提供されるようにします。

## 1.組織の [!UICONTROL on-device decisioning] を有効にする

オンデバイス判定を有効にすることで、A/B アクティビティがほぼゼロの待ち時間で実行されるようになります。 この機能を有効にするには、[!DNL Adobe Target] で **[!UICONTROL Administration]**/**[!UICONTROL Implementation]**/**[!UICONTROL Account details]** に移動し、「**[!UICONTROL On-Device Decisioning]**」トグルを有効にします。

&lt;!— image-odd4.png を挿入 – >
![alt 画像 ](assets/asset-odd-toggle.png)

>[!NOTE]
>
>オンデバイス判定の切り替えを有効または無効にするには、管理者または承認者 [ ユーザーの役割 ](https://experienceleague.adobe.com/docs/target/using/administer/manage-users/user-management.html) が必要です。

「**[!UICONTROL On-Device Decisioning]**」切替スイッチを有効 [!DNL Adobe Target] すると、クライアントのルールアーティファクトの生成が開始されます。

## 2. [!UICONTROL A/B Test] アクティビティの作成

[!DNL Adobe Target] で、**[!UICONTROL Activities]** ページに移動し、**[!UICONTROL Create Activity]**/**[!UICONTROL A/B test]** を選択します。

![alt 画像 ](assets/asset-ab.png)

**[!UICONTROL Create A/B Test Activity]** モーダルでは、デフォルトの **[!UICONTROL Web]** オプションを選択したままにし（1）、experience composer として **[!UICONTROL Form]** を選択し（2）、「**[!UICONTROL Property Restrictions]** なし」の **[!UICONTROL Default Workspace]** を選択し（3）、「**[!UICONTROL Next]**」（4）をクリックします。

![alt 画像 ](assets/asset-form.png)

## 3. A と B を定義する

1. アクティビティ作成の **[!UICONTROL Experiences]** の手順で、アクティビティの名前を指定し（1）、「エクスペリ **[!UICONTROL Add Experience]** ンス」（2） ボタンをクリックして、2 つ目のエクスペリエンスとして「エクスペリエンス B」を追加します。 A/B テストを実行するアプリケーション内の場所の名前（3）を入力します。 次の例では、ホームページは、エクスペリエンス A に対して定義された場所です（また、エクスペリエンス B に対して定義された場所でもあります）。

   エクスペリエンス A は、現在のホームページデザインであるコントロールを定義します。

   ![alt 画像 ](assets/asset-exp-a.png)

   エクスペリエンス B は、新しくデザインされたホームページを表すチャレンジャーを定義します。 クリックすると、既定のコンテンツ （1）を変更できます。

   ![alt 画像 ](assets/asset-exp-b.png)

1. エクスペリエンス B で、をクリックし、以下に示す（1）を選択して、コンテンツを **[!UICONTROL Default Content]** から再設計されたコンテンツに変更 **[!UICONTROL Create JSON Offer]** ます。

   ![alt 画像 ](assets/asset-offer.png)

1. ビジネスロジックが、実稼動環境の現在のホームページではなく、新しく再設計されたホームページをレンダリングできるように、フラグとして利用される属性を使用して JSON を定義します。


   >[!NOTE]
   >
   >[!DNL Adobe Target] がユーザーをバケット化してエクスペリエンス B （再設計されたホームページ）を表示すると、例で定義されている属性を含んだ JSON が返されます。 コードでは、属性値を確認して、再設計されたホームページをレンダリングするためにビジネスロジックを実行するかどうかを決定する必要があります。 この JSON 応答では、名前、値、属性の数を定義できます。

   ![alt 画像 ](assets/asset-homepage.png)

## 4. オーディエンスの追加

最初に、常連客を対象に再設計をテストするとします。常連客は、ログインしているかどうかに基づいて特定できます。

1. **[!UICONTROL Targeting]** の手順で、をクリックして、**[!UICONTROL All Visitors]** オーディエンスを置き換えます（下図を参照）。

   ![alt 画像 ](assets/asset-all-audiences.png)

1. **[!UICONTROL Create Audience]** モーダルで、`logged-in = true` の場所にカスタムルールを定義します。 ログインしているユーザーのグループを定義します。 このオーディエンスをアクティビティで使用します。

   ![alt 画像 ](assets/asset-audience.png)

## 5. トラフィック配分の設定

新しいホームページのデザインをテストする、ログインユーザーの割合を定義します。 つまり、このテストをロールアウトするユーザーの割合を指定します。 この例では、すべてのログインユーザーにこのテストをデプロイするには、トラフィックの割り当てを 100% に保ちます。

![alt 画像 ](assets/asset-allocation.png)

## 6. トラフィック配分をバリエーションに設定する

ホームページの現在のデザインまたはまったく新しいデザインを表示する、ログインユーザーの割合を定義します。 この例では、エクスペリエンス A と B の間でトラフィック配分を 50/50 に分割します。

![alt 画像 ](assets/asset-traffic-distribution.png)

## 7. レポートの設定

**[!UICONTROL Goals & Settings]** の手順で、[!DNL Adobe Target] UI でアクティビティの結果を表示する **[!UICONTROL Reporting Source]** として **[!UICONTROL Adobe Target]** を選択するか、Adobe Analytics UI で表示する **[!UICONTROL Adobe Analytics]** を選択します。

![alt 画像 ](assets/asset-reporting.png)

## 8. KPI を追跡するための指標を追加する

A/B テストを測定する **[!UICONTROL Goal Metric]** を選択します。 この例では、コンバージョンの成功は、ユーザーがページの下部に達したかどうかに基づいて行われ、エンゲージメントを示します。 したがって、ユーザー **[!UICONTROL Conversion]** ページの下部という名前の場所を表示したかどうかに基づいてステータスが決定されます。

## 9.機能フラグを使用した A/B テストをアプリケーションに実行するコードを実装する

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const options = {
  client: "testClient",
  organizationId: "ABCDEF012345677890ABCDEF0@AdobeOrg",
  decisioningMethod: "on-device",
  events: {
    clientReady: targetClientReady
  }
};
const targetClient = TargetClient.create(options);

function targetClientReady() {
  return targetClient.getAttributes(["homepage"]).then(function(attributes) {
    const flag = attributes.getValue("homepage", "feature-flag");
    // ...
  });
}
```

>[!TAB Java]

```java {line-numbers="true"}
import com.adobe.target.edge.client.ClientConfig;
import com.adobe.target.edge.client.TargetClient;
import com.adobe.target.delivery.v1.model.ChannelType;
import com.adobe.target.delivery.v1.model.Context;
import com.adobe.target.delivery.v1.model.ExecuteRequest;
import com.adobe.target.delivery.v1.model.MboxRequest;
import com.adobe.target.edge.client.entities.TargetDeliveryRequest;
import com.adobe.target.edge.client.model.TargetDeliveryResponse;

ClientConfig config = ClientConfig.builder()
    .client("testClient")
    .organizationId("ABCDEF012345677890ABCDEF0@AdobeOrg")
    .build();
TargetClient targetClient = TargetClient.create(config);
MboxRequest mbox = new MboxRequest().name("homepage").index(0);
TargetDeliveryRequest request = TargetDeliveryRequest.builder()
    .context(new Context().channel(ChannelType.WEB))
    .execute(new ExecuteRequest().mboxes(Arrays.asList(mbox)))
    .build();
Attributes attributes = targetClient.getAttributes(request, "homepage");
String flag = attributes.getString("homepage", "feature-flag");
```

>[!ENDTABS]

## 10.機能フラグを使用して A/B テストをアクティブ化する

![alt 画像 ](assets/asset-activate.png)
