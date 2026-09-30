---
title: mbox3rdPartyId のリアルタイムプロファイル同期
description: '[!DNL Adobe Experience Platform Web SDK]でmbox3rdPartyIdを使用する方法を説明します。'
keywords: personalization;target;adobe target;renderDecisions;sendEvent;mbox3rdPartyId;
feature: AEP Web SDK
exl-id: 1c5067ef-38b3-4bf1-bd39-ea0f2cbd1074
TQID: 'https://experienceleague.adobe.com/Ej2sYVnBD9orRTlsMQG85JJV7dvn-9gnABDa0b8uBlM'
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
    internal-label: Target
feature_v2:
  - id: adee20bd-51f4-461d-b9db-d215f8756eeb
    internal-label: Audiences
  - id: b050e0cd-2ddd-42cd-a71b-5d9e1fdf75e0
    internal-label: APIs and SDKs
subfeature_v2:
  - id: a1f3c920-a3a8-4506-8067-53189547b5e6
    internal-label: AEP Web SDK
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
    internal-label: Developer
topic_v2:
  - id: e0eb8757-182f-49f3-94a4-1587d16f5094
    internal-label: Personalization
source-git-commit: 5d119ccf18b09b3ba864a69642458597f65c754f
workflow-type: tm+mt
source-wordcount: '181'
ht-degree: 32%
---
# mbox3rdPartyIdの使用

[!DNL Adobe Target] の `mbox3rdPartyId` は、会社のロイヤルティプログラムのメンバーシップ ID などの、会社の訪問者 ID です。

訪問者が会社のサイトにログインすると、通常、会社は訪問者のアカウント、ロイヤルティカード、メンバーシップ番号またはその会社で適用できるその他の識別子に結び付けられる ID を作成します。 [詳細情報](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html?lang=ja)

## `mbox3rdPartyId`を[!DNL Platform Web SDK]と共に使用する方法

### 手順1: `Target Third Party ID Namespace`を設定する

[&#x200B; データストリーム &#x200B;](https://experienceleague.adobe.com/ja/docs/experience-platform/datastreams/overview)の`Target Third Party ID Namespace`を、mbox サードパーティ IDとして使用するID名前空間を使用して設定します。 [ID名前空間の詳細](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja)

![Target サードパーティ ID名前空間フィールドを示すExperience Platform UI。](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

### 手順2: `mbox3rdpartyId`を[!DNL Target]に送信する

手順1で設定したID名前空間を使用して、`sendEvent` コマンドで`mbox3rdpartyId`を[!DNL Target]に送信します。
[IDの送信について詳しく見る](/help/dev/implement/client-side/aep-web-sdk/using-mbox-3rdpartyid.md)

```javascript
alloy("sendEvent", {
  xdm: {
    "identityMap": {
      "ID_NAMESPACE": [ // Replace `ID_NAMESPACE` with the namespace you have configured in Step 1.
        {
          "id": "1234",
          "authenticatedState": "authenticated"
        }
      ]
    }
  }
});
```
