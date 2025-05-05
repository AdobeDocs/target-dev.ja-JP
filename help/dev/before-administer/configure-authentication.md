---
title: ' [!DNL Adobe Target] API 用の認証の設定方法'
description: ' [!DNL Adobe Target] API を正常にやり取りするために必要な認証トークンを生成するにはどうすればよいですか？'
feature: APIs/SDKs, Administration & Configuration
exl-id: fc67363c-6527-40aa-aff1-350b5af884ab
source-git-commit: 2fba03b3882fd23a16342eaab9406ae4491c9044
workflow-type: tm+mt
source-wordcount: '1785'
ht-degree: 0%

---

# [!DNL Adobe Target] API の認証の設定

[!DNL Recommendations Admin] API を含む [!DNL Adobe Target] 管理 API は、許可されたユーザーのみが [!DNL Adobe Target] ーザーにアクセスするために使用できるように、認証で保護されています。 [Adobe Developer Console](https://developer.adobe.com/console/home) を使用して、[!DNL Adobe Target] を含むすべての [!DNL Adobe Experience Cloud solutions] に対してこの認証を管理します。

>[!IMPORTANT]
>
>この記事で説明するサービスアカウント（JWT）資格情報は、新しい OAuth サーバー間資格情報に置き換えて、非推奨になります。
>
>サービスアカウント（JWT）資格情報は、2025 年 1 月 1 日（PT）まで引き続き機能します。 新しい OAuth サーバー間資格情報を使用するには、2025 年 1 月 1 日（PT）までにアプリケーションまたは統合を移行する必要があります。
>
>統合を移行するための詳細と手順については、*Developer Console[ ドキュメントのサービスアカウント（JWT）資格情報から OAuth サーバー間資格情報への移行 ](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/migration/){target=_blank} を参照してください*。
>
>新しい OAuth 資格情報の設定について詳しくは、*Developer Console[ ドキュメントの ](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/implementation/){target=_blank}OAuth サーバー間資格情報の実装* を参照してください。

[!DNL Adobe Target] API を正常に操作するために必要なレガシー JWT 認証トークンを生成するための準備手順を以下に示します。

1. プロジェクト（旧称：統合）を [!DNL Adobe Developer Console] で作成します。
1. プロジェクトの詳細をPostmanに書き出します。
1. ベアラアクセストークンを生成します。
1. ベアラアクセストークンのテスト

## 前提条件

| リソース | 詳細 |
| --- | --- |
| Postman | これらの手順を正常に完了するには、お使いのオペレーティングシステムに対応する [&#128279;](https://www.postman.com/downloads/)0&rbrace;Postman アプリ &rbrace; を取得します。 Postman basic は無料でアカウントを作成できます。 Postmanは、[!DNL Adobe Target] API を一般的に使用するために必要なものではありませんが、API ワークフローを容易にし、API の実行や [!DNL Adobe Target] の動作の学習に役立つ複数のPostman コレクションを提供しています。 このガイドの残りの部分では、Postmanに関する実務知識を前提としています。 サポートについては、[Postmanのドキュメント ](https://learning.getpostman.com/) を参照してください。 |
| 参照 | このガイドの残りの部分では、次のリソースに慣れていることを前提としています。<ul><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[Target 管理者およびプロファイル API のドキュメント ](../administer/admin-api/admin-api-overview-new.md)</li><li>[Recommendations API ドキュメント ](https://developer.adobe.com/target/administer/recommendations-api/)</li></ul> |

## Adobe I/Oプロジェクトの作成

この節では、[!DNL Adobe Developer Console] にアクセスして、[!DNL Adobe Target] 用のプロジェクトを作成します。 詳しくは、[ プロジェクトに関するドキュメント ](https://developer.adobe.com/developer-console/docs/guides/projects/) を参照してください。

&lt;! – （1. [ 認証に関するドキュメント ](https://developer.adobe.com/developer-console/docs/guides/authentication/) に従って、秘密鍵と公開証明書を生成します。// [//]:#（[Adobe IO の設定方法：認証 – 手順 ](https://helpx.adobe.com/marketing-cloud-core/kb/adobe-io-authentication-step-by-step.html) の **手順 1** で説明します。 手順 1 を完了したら、このガイドに戻り、次の手順 2 に進みます。//このステップの結果は、`private.key` ファイルと `certificate_pub.crt` ファイルの作成になります。 これら 2 つのファイルを生成したら、このガイドに戻ります。） – >

1. [Adobe Admin Console](https://adminconsole.adobe.com/) で、お使いの [!DNL Adobe] ユーザーアカウントに [!DNL Target] に対する [ 製品管理者 ](https://helpx.adobe.com/jp/enterprise/using/admin-roles.html) レベルおよび [ 開発者 ](https://helpx.adobe.com/jp/enterprise/using/manage-developers.html) レベルのアクセス権の両方が付与されていることを確認します。

1. [Adobe Developer Console](https://developer.adobe.com/console/home) で、この統合を作成する [!UICONTROL Experience Cloud Organization] を選択します。 （1 つの [!UICONTROL Experience Cloud Organization] ージにのみアクセスできる可能性があります）。

   ![configure-io-target-createproject2.png](assets/configure-io-target-createproject2.png)

1. **[!UICONTROL Create new project]** をクリックします。

   ![configure-io-target-createproject3.png](assets/configure-io-target-createproject3.png)

1. 「**[!UICONTROL Add API]**」をクリックして REST API をプロジェクトに追加し、[!DNL Adobe] サービスおよび製品にアクセスします。

   ![API を追加 ](assets/configure-io-target-createproject4.png)

1. 統合する [!DNL Adobe] サービスとして **[!DNL Adobe Target]** を選択します。 表示される「**[!UICONTROL Next]**」ボタンをクリックします。

   ![configure-io-target-createproject5](assets/configure-io-target-createproject5.png)

1. [!DNL Target] 用に作成するサービス アカウント統合に公開キーと秘密キーを関連付けるためのオプションを選択してください。 この例では、「**[!UICONTROL Option 1: Generate a key pair]**」を選択し、「**[!UICONTROL Generate keypair]**」をクリックします。

   ![configure-io-target-createproject6](assets/configure-io-target-createproject6.png)

1. 指示に従って、自動的にダウンロードされた設定ファイル（`config`）をメモします。これには、秘密鍵が含まれています。 **[!UICONTROL Next]** をクリックします。

   ![configure-io-target-createproject7](assets/configure-io-target-createproject7.png)

1. ファイルシステムで、`config` の場所を確認します。これは、前の手順で作成した圧縮された設定ファイルです。 ここでも、この `config` ファイルには秘密鍵が含まれています。これは後で必要になります。 ファイルシステム内の正確な場所は、ここで示した場所とは異なる場合があります。

   ![configure-io-target-createproject8](assets/configure-io-target-createproject8.png)

1. Adobe Developer Consoleに戻り、Adobe Recommendationsを使用しているプロパティに対応する [ 製品プロファイル ](https://helpx.adobe.com/jp/enterprise/using/manage-products-and-profiles.html) を選択します。 （プロパティを使用しない場合は、「デフォルトのWorkspace」オプションを選択します）。 **[!UICONTROL Save configured API]** をクリックします。

   ![configure-io-target-createproject9](assets/configure-io-target-createproject9.png)

1. 「**[!UICONTROL Create Integration]**」をクリックします。 API が正常に設定されたことを示す一時的なメッセージが表示されます。
1. 最後の手順として、プロジェクトの名前を元の `Project 1` よりも意味のある名前に変更します。 それには、ナビゲーションパスを表示として使用してプロジェクトに移動し、「**[!UICONTROL Edit project]**」をクリックして **[!UICONTROL Edit Project]** モーダルにアクセスし、プロジェクトの名前を変更します。

   ![configure-io-target-createproject11](assets/configure-io-target-createproject11.png)

>[!NOTE]
>
>この例では、プロジェクトに「[!DNL Target] Integration」という名前を付けます。 プロジェクトを単なる [!DNL Adobe Target] 以上の用途で使用することが予想される場合は、それに応じて名前を付けた方がよいでしょう。 例えば、Adobe Experience Cloudの他のソリューションと共に使用される可能性があるので、「AdobeAPI」または「Experience CloudAPI」という名前を付けることができます。

## プロジェクト詳細の書き出し

これで、[!DNL Target] へのアクセスに使用できるAdobeプロジェクトが作成されました。次は、そのプロジェクトの詳細とAdobe API リクエストを送信する必要があります。 これらの詳細は、複数の [!DNL Target] API を含む複数のAdobeAPI とやり取りするために必要です。 例えば、統合の詳細には、[!DNL Target] 管理 API で必要な認証および認証情報が含まれます。 したがって、Postmanで API を使用するには、これらの詳細をPostmanに取り込む必要があります。

Postmanでは多くの方法でプロジェクトの詳細を指定できますが、この節では、いくつかの事前定義済みの機能とコレクションを利用します。 まず（この節では）、統合の詳細をPostman環境に書き出します。 次に（次の節で）、必要なAdobeリソースへのアクセス権を付与するベアラアクセストークンを生成します。

>[!NOTE]
>
>[!DNL Target] を含む任意のExperience Cloudソリューションに適用できるビデオ手順については、[Experience Platform API でのPostmanの使用 ](https://experienceleague.adobe.com/docs/platform-learn/tutorials/platform-api-authentication.html?lang=ja) を参照してください。 次の節は、[!DNL Target] API に関連しています。1. Experience PlatformAPI を作成してPostman 2 に書き出します。 Postmanでアクセストークンを生成します。 これらの手順は、以下でも説明します。

1. 引き続き [Adobe Developer Console](https://developer.adobe.com/console/home) で、新しいプロジェクトの **[!UICONTROL Service Account (JWT)]** 資格情報を表示するために移動します。 図に示すように、左側のナビゲーションまたは **[!UICONTROL Credentials]** のセクションを使用します。

   ![JWT1](assets/configure-io-target-jwt1.png)

   ま **[!UICONTROL Credential details]**、**[!UICONTROL Public key(s)]**、**[!UICONTROL Client ID]**、およびサービスアカウントに関連するその他の情報を表示することができます。

   ![JWT1a](assets/configure-io-target-jwt1a.png)

1. クリックして、**[!DNL Adobe Target]** API に関する情報に移動します。 図に示すように、左側のナビゲーションまたは **接続された製品とサービス** セクションを使用します。

   ![JWT2](assets/configure-io-target-jwt2.png)

1. **[!UICONTROL Download for Postman]**/**[!UICONTROL Service Account (JWT)]** をクリックして、Postman環境の認証情報を取得する JSON ファイルを作成します。

   ![JWT3](assets/configure-io-target-jwt3.png)

   ファイルシステムに JSON ファイルが存在することを確認します。

   ![JWT3a](assets/configure-io-target-jwt3a.png)

1. Postmanで、歯車アイコンをクリックして環境を管理し、「**[!UICONTROL Import]**」をクリックして JSON ファイル（environment）を読み込みます。

   ![JWT4](assets/configure-io-target-jwt4.png)

1. ファイルを選択し、「**[!UICONTROL Open]**」をクリックします。

   ![JWT5](assets/configure-io-target-jwt5.png)

1. Postman **環境の管理** モーダルで、新しく読み込んだ環境の名前をクリックして検査します。 （お使いの環境名は、ここに示すものとは異なる場合があります。 必要に応じて名前を編集します。 必ずしも [!DNL Adobe] プロジェクトの名前と一致する必要はありません）。

   ![JWT6](assets/configure-io-target-jwt6.png)

1. メモ `CLIENT_SECRET` および `API_KEY` （および他の変数）は、Adobe Developer Consoleで定義された統合から取得され、値が事前入力されています。 （Postmanの `CLIENT_SECRET` 変数は、Developer Consoleに表示される `CLIENT SECRET` Adobeの資格情報と一致する必要があります。また、Postmanの `API_KEY` は、Developer Consoleの `CLIENT ID` と同様に一致する必要があります）。 これに対して、メモ `PRIVATE_KEY`、`JWT_TOKEN`、`ACCESS_TOKEN` は空白です。 まず、`PRIVATE_KEY` の値を指定します。

   ![JWT7](assets/configure-io-target-jwt7.png)

1. ファイルシステムから `config` ファイルを開き、`private` key ファイルを開きます。

   ![JWT8](assets/configure-io-target-jwt8.png)

1. `private` キーファイルのコンテンツ全体を選択してコピーします。

   ![JWT9](assets/configure-io-target-jwt9.png)

1. Postmanで、秘密鍵の値を「**[!UICONTROL INITIAL VALUE]**」フィールドと「**[!UICONTROL CURRENT VALUE]**」フィールドに貼り付けます。

   ![JWT10](assets/configure-io-target-jwt10.png)

1. 「**[!UICONTROL Update]**」をクリックして、環境モーダルを閉じます。

## ベアラアクセストークンの生成

この節では、[!DNL Adobe Target] API とのインタラクションを認証するために必要なベアラアクセストークンを生成します。 ベアラーアクセストークンを生成するには、（前の節で確立された）Adobeの詳細を [ 統合Identity Management サービス （IMS） ](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/AuthenticationGuide.md) に送信する必要があります。 これを行うには、いくつかの異なる方法がありますが、このガイドでは、プロセスを直接的かつ簡単にする事前定義済みの IMS 呼び出しを含むPostman コレクションを利用します。 コレクションを読み込むと、必要に応じて再利用でき、[!DNL Adobe Target] ーザーだけでなく他のAdobeAPI にも新しいトークンを生成できます。

1. [AdobeIdentity Management Service API サンプル呼び出し ](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims) に移動します。

   ![token1](assets/configure-io-target-generatetoken1.png)

1. **[!UICONTROL Adobe I/O Access Token Generation Postman collection]** をクリックします。

   ![token2](assets/configure-io-target-generatetoken2.png)

1. **[!UICONTROL Raw]** をクリックし、結果の JSON をクリップボードにコピーすることで、このコレクションの生の JSON を取得します。 （または、生の JSON を.json ファイルとして保存できます。）

   ![token3](assets/configure-io-target-generatetoken3.png)

1. Postmanで、クリップボードに生の JSON を貼り付けて送信することで、コレクションを読み込みます。 （または、保存した.json ファイルをアップロードできます）。 **[!UICONTROL Continue]** をクリックします。

   ![token4](assets/configure-io-target-generatetoken4.png)

1. Adobe I/Oアクセストークン生成Postman コレクションの **[!UICONTROL IMS: JWT Generate + Auth via User Token]** リクエストを選択し、環境が選択されていることを確認し、「**[!UICONTROL Send]**」をクリックしてトークンを生成します。

   ![token5](assets/configure-io-target-generatetoken5.png)

   >[!NOTE]
   >
   >このベアラアクセストークンは 24 時間有効です。 新しいトークンを生成する必要がある場合は常に、リクエストを再度送信します。

1. 環境の管理モーダルを再度開き、環境を選択します。

   ![token6](assets/configure-io-target-jwt11.png)

1. `ACCESS_TOKEN` と `JWT_TOKEN` の値が入力されていることに注意してください。

   ![token7](assets/configure-io-target-generatetoken7.png)

質問：JSON web トークン（JWT）とベアラーアクセストークンを生成するには、Adobe I/Oアクセストークン生成Postman コレクションを使用する必要がありますか？

答え：いいえ。 Adobe I/Oアクセストークン生成Postman コレクションは、Postmanで JWT およびベアラーアクセストークンをより簡単に生成できる便利な機能として使用できます。 または、Adobe Developer Console内の機能を使用して、ベアラアクセストークンを手動で生成することもできます。

## ベアラアクセストークンのテスト

この演習では、[!DNL Target] アカウントからアクティビティのリストを取得する API リクエストを送信して、新しいベアラーアクセストークンを使用します。 応答が成功すると、API を使用するために、[!DNL Adobe] プロジェクトと認証が期待どおりに動作していることが示されます。

1. [[!DNL Adobe Target] Admin APIs Postman Collection](https://developers.adobetarget.com/api/#admin-postman-collection) を読み込みます。 コレクションがPostmanに読み込まれるまで、すべてのプロンプトに従います。

   ![testtoken1](assets/configure-io-target-testtoken0.png)

1. コレクションを展開し、**[!UICONTROL List activities]** リクエストをメモします。

   ![testtoken1](assets/configure-io-target-testtoken1.png)

1. `{{access_token}}` などの変数は、最初は未解決です。 この問題は、いくつかの方法で解決できます。例えば、`{{access_token}}` という新しいコレクション変数を定義できます。ただし、このガイドでは、代わりに、API リクエストを変更して、以前に使用していたPostman環境を活用します。 これにより、Adobeは、環境 API 間で共通のすべての変数を一元的かつ一貫して統合したものとして引き続き機能できます。

   ![testtoken2](assets/configure-io-target-testtoken2.png)

1. `{{access_token}}` を `{{ACCESS_TOKEN}}` に置き換えるためにと入力します。

   ![testtoken3](assets/configure-io-target-testtoken3.png)

1. `{{api_key}}` を `{{API_KEY}}` に置き換えるためにと入力します。

   ![testtoken4](assets/configure-io-target-testtoken4.png)

1. `{{tenant}}` を `{{TENANT_ID}}` に置き換えるためにと入力します。 メモ `{{TENANT_ID}}` はまだ認識されていません。

   ![testtoken4](assets/configure-io-target-testtoken4a.png)

1. 環境を管理モーダルを開き、環境を選択します。

   ![JWT11](assets/configure-io-target-jwt11.png)

1. を入力して、新しい `{{TENANT_ID}}` 環境変数を追加します。 テナント ID 値をコピーして、新しい `TENANT_ID` 環境変数の **[!UICONTROL INITIAL VALUE]** および **[!UICONTROL CURRENT VALUE]** フィールドに貼り付けます。

   ![testtoken5](assets/configure-io-target-testtoken5.png)

   >[!NOTE]
   >
   >テナント ID が [!DNL Target] `clientcode` と異なります。 [!DNL Target] にログインすると、URL 内にテナント ID が存在します。 テナント ID を取得するには、Adobe Experience Cloudにログインし、[!DNL Target] を開いて「Target」カードをクリックします。 URL サブドメインに示されているように、テナント ID の値を使用します。 例えば、[!DNL Adobe Target] にログインしたときの URL が `<https://mycompany.experiencecloud.adobe.com/...>` の場合、テナント ID は「mycompany」です。

1. 正しい環境を選択したことを確認したら、リクエストを送信します。 アクティビティのリストを含んだ応答が届きます。

   ![testtoken6](assets/configure-io-target-testtoken6.png)

これでAdobe認証を確認したので、それを使用して [!DNL Adobe Target] API （および他のAdobeAPI）とやり取りできます。 例えば、[Recommendations API を使用 ](recs-api/overview.md) してレコメンデーションを作成または管理したり、[Target 配信 API](/help/dev/implement/delivery-api/overview.md) と共に使用したりできます。
