---
title: mbox3rdPartyId のリアルタイムプロファイル同期
description: ' [!DNL Adobe Experience Platform Web SDK] で mbox3rdPartyId を使用する方法を説明します。'
keywords: パーソナライゼーション；Target;Adobe Target;renderDecisions;sendEvent;mbox3rdPartyId;
feature: AEP Web SDK
source-git-commit: 27759841c8d6a4ac4cc72e735f1dc6512c71aea5
workflow-type: tm+mt
source-wordcount: '147'
ht-degree: 16%

---

# mbox3rdPartyId とは

[!DNL Adobe Target] の `mbox3rdPartyId` は、会社のロイヤルティプログラムのメンバーシップ ID などの、会社の訪問者 ID です。

訪問者が会社のサイトにログインすると、会社は通常、訪問者のアカウント、ロイヤルティカード、メンバーシップ番号、またはその会社のその他の該当する識別子に関連付けられた ID を作成します。 [詳細情報](https://experienceleague.adobe.com/docs/target/using/audiences/visitor-profiles/3rd-party-id.html?lang=ja#)

## `mbox3rdPartyId` での [!DNL Platform Web SDK] の使用方法

### 手順 1:`Target Third Party ID Namespace` を設定する

mbox サードパーティ ID として使用する ID 名前空間を使用して、`Target Third Party ID Namespace` データストリーム [ の ](https://experienceleague.adobe.com/ja/docs/experience-platform/datastreams/overview) を設定します。 [ID 名前空間の詳細情報 ](https://experienceleague.adobe.com/docs/experience-platform/identity/namespaces.html?lang=ja)

![Target サードパーティ ID 名前空間フィールドを示すExperience Platform UI。](/help/dev/implement/client-side/aep-web-sdk/assets/mbox3rdpartyid.png)

### 手順 2:`mbox3rdpartyId` を [!DNL Target] に送信する

手順 1 で設定した ID 名前空間を使用して、`mbox3rdpartyId` コマンドで [!DNL Target] に `sendEvent` を送信します。
[ID の送信の詳細情報 ](../../identity/overview.md#syncing-identities)

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
