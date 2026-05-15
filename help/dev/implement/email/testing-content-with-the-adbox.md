---
keywords: 実装、非JavaScript、mbox、adbox
description: ' [!DNL Adobe Target]を使用してオフサイト実装で画像を配信するには、AdBoxを使用します。 AdBoxはmboxに似ていますが、JavaScriptではなくURLによって制御されます。'
title: 画像用のAdboxを作成するにはどうすればよいですか？
feature: Implement Email
exl-id: ad1eb6c4-7a16-4054-ae76-57971261e931
TQID: https://experienceleague.adobe.com/OPo9T2Eb7afF8Ir8PAlY62OX83zhxtruUMKWCfUthxY
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: c94a34eb-b51c-4dd1-a6a4-46b0d84ccccd
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 335
ht-degree: 68%

---

# 画像用 adbox の作成

AdBoxを使用して、[!DNL Adobe Target]を使用してオフサイト実装で画像を配信します。

adbox は mbox と似ていますが、JavaScript ではなく URL によって制御されます。 adbox は、「広告」mbox（adbox）をアカウントに読み込む特殊な adbox 用の URL を使用して作成されます。 アクティビティでは、この adbox を mbox の代わりに使用します。 電子メールなどの JavaScript を使用しない実装では、画像の直接参照ではなく adbox URL を使用します。

適切な設定を選択する方法については、[JavaScript以外のベースの実装](/help/dev/implement/email/overview.md)を参照してください。

1. 次の adbox URL を作成します。

   ```
   https://myClientCode.tt.omtrdc.net/m2/myClientCode/ubox/
   image?mbox=emailHeroImage123_320x200&
   mboxDefault=http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2Fimg%2Flogo%2Egif
   ```

   * `myClientCode` はお客様のクライアントコードです。 クライアントコードはすべて小文字で、特殊文字は含まれません。

     クライアントコードは、[!DNL Target] インターフェイスの&#x200B;**[!UICONTROL Administation]** > **[!UICONTROL Implementation]** ページの上部にあります。

   * `image` は呼び出しのタイプです。 この場合は画像です。

   * `emailHeroImage123_320x200` は adbox の名前です。

   * `http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2Fimg%2Flogo%2Egif` は mbox のデフォルトコンテンツです。 画像にする必要があります。

     URL エンコードをおこない、絶対参照にする必要があります。 [HTML URL エンコーディング参照](https://www.w3schools.com/tags/ref_urlencode.asp)を使用すると、URLをすばやくエンコードできます。

1. 各代替画像の[リダイレクトオファー](https://experienceleague.adobe.com/docs/target/using/experiences/offers/offer-redirect.html)を作成します。

   >[!NOTE]
   >
   >adbox は、リダイレクトオファーまたはデフォルトコンテンツオファーを使用して読み込む必要があります。 その他のオファータイプは使用できません。 adbox は URL なので、受け取った URL しか表示できません。つまり、リダイレクトオファーのみが機能します。

1. アクティビティを作成します。

   目的に合わせた適切な設定については、「[JavaScript ベース以外の実装](/help/dev/implement/email/overview.md)」を参照してください。

1. アクティビティで QA を完了します。

   ダミーページを作成し、あらゆる環境のすべてのタイプのブラウザーで、エクスペリエンス、デフォルトコンテンツおよびレポートがすべて適切に機能するか確認することをお勧めします。

1. アクティビティを起動します。
