---
keywords: 実装, mbox.js 非 JavaScript, リダイレクター, クリックあたりのコスト, クリックあたりの売上高
description: 電子メール実装でのリダイレクターの使用方法を説明します。 [!DNL Adobe Target] アクティビティ。
title: リダイレクターの使用方法
feature: Implement Email
exl-id: 072368ff-9f17-4709-ac2d-c9e1f0d888bb
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '693'
ht-degree: 66%

---

# リダイレクターの使用

リダイレクターは、テストで mbox を使用する場合と同様に使用します。

リダイレクターは、リダイレクター mbox（リダイレクター）をアカウントにロードする特殊なリダイレクターの URL を使用して作成されます。このリダイレクターは、テストで mbox を使用する場合と同様に使用します。リダイレクターの URL を広告ネットワークに広告の表示先リンクとして送信します。

リダイレクターの使用目的は次のとおりです。

* ディスプレイ広告からサイトへのクリックの追跡
* 複数の広告ネットワーク上のディスプレイ広告に対するクリックを追跡するための、一元管理された単一のレポートの作成
* ディスプレイ広告の表示先の変更

  例えば、同じバナーはホームページ、カテゴリページおよび製品紹介ページにランディングします。

* 最もコンバージョン率の高いランディングページの発見

適切な設定の判断方法については、[JavaScript ベース以外の実装](/help/dev/implement/email/overview.md).

## リダイレクターの作成

リダイレクターを使用するには、まずリダイレクターを作成する必要があります。

1. 広告の様々な表示先（デフォルトの表示先を含む）を決定します。
1. リダイレクターの URL を作成します。

   ```
   https://<your_testandtarget_clientcode>.tt.omtrdc.net/​m2/yourclientcode/ubox
   /​page?mbox=redirectorlink_456
   &mboxDefault=http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2Fusualdestination%2Ehtm
   ```

   * `yourclientcode` はお客様のクライアントコードです。クライアントコードはすべて小文字で、特殊文字は含まれません。

     クライアントコードは、 **[!UICONTROL 管理]** > **[!UICONTROL 実装]** ページ [!DNL Target] インターフェイス。

   * `redirectorlink_456` は、キャンペーンおよびテストで使用するためにアカウントで表示するリダイレクター mbox の名前です。

     リダイレクターの機能は他の mbox とは異なりますが、他の mbox と同様にアカウントで表示されます。アカウント内の標準タイプの mbox と区別しやすいようなリダイレクターの名前を指定してください。mbox の名前には、&#39;redirectorlink&#39; を先頭に付けることをお勧めします。

   * `http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2Fusualdestination%2Ehtm` はデフォルトの宛先です。

     URL エンコードをおこない、絶対参照にする必要があります。以下を使用すると、 [HTMLURL エンコーディングのリファレンス](https://www.w3schools.com/tags/ref_urlencode.asp) を使用して、URL を簡単にエンコードできます。

   >[!WARNING]
   >
   >リダイレクターを使用すると、オープンリダイレクトの脆弱性のリスクにさらされる可能性があることに注意してください。 サードパーティによるリダイレクターリンクの不正使用を防ぐため、Adobeでは、「認証済みホスト」を使用してデフォルトのリダイレクト URL ドメインをするこ許可リストに加えるとをお勧めします。 [!DNL Target] は、ホストを使用して、リダイレクトを許可するドメインを許可リストに登録します。詳しくは、 [mbox 呼び許可リストに加える出しを送信する権限のあるホストを指定するの作成 [!DNL Target]](https://experienceleague.adobe.com/docs/target/using/administer/hosts.html#allowlist) in *ホスト*.

1. リダイレクターを検証します。
   1. *セキュリティのベストプラクティス*：前述のように、リダイレクターで使用されるドメインが許可リストに加えるされていることを確認します。 許可リストに加えるされていないドメインを使用する場合、Adobeは、悪意のあるアクターがリダイレクターを使用して悪意のあるドメインにリダイレクトするのを防ぐために、そのドメインへの呼び出しをブロックします。
   2. リダイレクターの URL をブラウザーに挿入して表示を更新します。
   3. アカウントにログインし、mbox のリストを更新して、新しいリダイレクターが mbox として表示されることを確認します。
1. 1 つの広告に対してさまざまな表示先をテストする場合、各バージョンごとに[リダイレクトオファー](https://experienceleague.adobe.com/docs/target/using/experiences/vec/redirect-offer.html)を作成します。
1. キャンペーンを作成します。

   目的に合わせた適切な設定については、「[JavaScript ベース以外の実装](/help/dev/implement/email/overview.md)」を参照してください。
1. キャンペーンで QA を完了します。

   リダイレクターの URL を含む `<a href>` を使用してダミーページを作成します。例：

   ```
   <a href=https://<your_clientcode>.tt.omtrdc.net/​m2/yourclientcode/ubox/​page?mbox=
   
   redirectorlink_456&mboxDefault=http%3A%2F%2Fwww%2Eyourcompany%2Ecom%2F​usualdestination%2Ehtm>
   ```

1. あらゆる環境のすべてのタイプのブラウザーで、エクスペリエンス、デフォルトコンテンツおよびレポートがすべて期待どおりに機能することを確認します。

   >[!NOTE]
   >
   >リダイレクターは、オファープレビューまたは mbox の閲覧ではサポートされていません。ブラウザーで直接エクスペリエンスのプレビューを確認してください。また、 `mboxDebug` はリダイレクターでは機能しません。

1. リダイレクターの完全な URL をディスプレイ広告ネットワークに広告の表示先として送信します。

## リダイレクターを使用してクリックあたりのコストとクリックあたりの売上高を渡す

リダイレクターを使用してクリックあたりのコストとクリックあたりの売上高を渡す方法について説明します。

### クリックあたりのコストを渡す

リダイレクターを使用して、クリックあたりのコストを引き渡すことができます。

>[!NOTE]
>
>ベストプラクティスは、 **[!UICONTROL 訪問あたりのスコア]** エンゲージメント指標。

`&mboxPageValue=-value` を URL に追加します。負の値であることに注意してください。

例：クリックあたり 10 セントのコストの場合

```
https://<your_clientcode>.tt.omtrdc.net/​m2/yourclientcode/ubox/​page?mbox=redirectorlink_456
&mboxPageValue=-0.1&mboxDefault=​https://www.yourcompany.com/usualdestination.htm
```

### クリックあたりの売上高を渡す

リダイレクターを使用して、クリックあたりの売上高を引き渡すことができます。

>[!NOTE]
>
>ベストプラクティスは、 **[!UICONTROL 訪問あたりのスコア]** エンゲージメント指標。

`&mboxPageValue=value` を URL に追加します。

例：クリックあたり 10 セントの売上高の場合

```
https://<​your_clientcode>​​​​.tt​​.omtrdc​.net/​​m2/​yourclientcode/​ubox/​​​page?mbox=redirectorlink_456
&mboxPageValue=0.1​&mbox​Default=​​http%3A%2F%2Fwww%2E​yourcompany%2Ecom​%2Fusualdestination%2Ehtm
```
