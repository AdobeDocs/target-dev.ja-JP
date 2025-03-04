---
keywords: クライアントケア，cname，証明書プログラム，正規名，cookie，証明書，amc, adobe managed certificate, digicert, ドメイン制御検証，dcv, client care2
description: '[!UICONTROL Adobe Client Care] と連携して CNAME （正規名）のサポートを実装し、広告ブロ  [!DNL Adobe Target]  クの問題を処理します。'
title: Target での CNAME の使用方法
feature: Privacy & Security
exl-id: 5709df5b-6c21-4fea-b413-ca2e4912d6cb
source-git-commit: 31d7de17530c14a392cbeef777937c07a214e07a
workflow-type: tm+mt
source-wordcount: '1164'
ht-degree: 1%

---

# CNAME と Target

[!DNL Adobe Client Care] を使用して [!DNL Adobe Target] で CNAME （正規名）サポートを実装する手順。 CNAME を使用すると、広告ブロックの問題や ITP 関連の（インテリジェントトラッキング防止） cookie ポリシーを処理できます。 CNAME を使用すると、Adobeが所有するドメインではなく、顧客が所有するドメインに対して呼び出しが行われます。

## Target での CNAME サポートのリクエスト

1. SSL 証明書に必要なホスト名のリストを決定します（以下の FAQ を参照）。

1. ホスト名ごとに、通常の [!DNL Target] ホスト名 `clientcode.tt.omtrdc.net` を指す CNAME レコードを DNS に作成します。

   例えば、クライアントコードが「cnamecustomer」で、提案されたホスト名が `target.example.com` の場合、DNS CNAME レコードは次のようになります。

   ```
   target.example.com.  IN  CNAME  cnamecustomer.tt.omtrdc.net.
   ```

   >[!WARNING]
   >
   >Adobeの証明機関 DigiCert は、この手順が完了するまで証明書を発行できません。 そのため、Adobeは、この手順が完了するまで、CNAME 実装のリクエストを実行できません。

1. [ このフォームに入力し ](assets/FPC_Request_Form.xlsx)[CNAME サポートをリクエストするAdobe クライアントケアチケットを開く ](https://experienceleague.adobe.com/docs/target/using/cmp-resources-and-contact-information.html?#reference_ACA3391A00EF467B87930A450050077C) 際に含めます。

   * [!DNL Adobe Target] クライアントコード：
   * SSL 証明書のホスト名（例：`target.example.com target.example.org`）:
   * SSL 証明書購入者（Adobeを強くお勧めします。FAQ を参照してください）:Adobe/お客様
   * お客様が証明書（「Bring Your Own Certificate」（BYOC）とも呼ばれる）を購入する場合は、次の詳細を入力します。
      * 証明書組織（例：Example Company Inc）:
      * 証明書の組織単位（オプション、例：マーケティング）:
      * 証明書の国（例：米国）:
      * 証明書の州/地域（例：カリフォルニア）:
      * 証明書市区町村（例：サンノゼ）:

1. Adobeが証明書を購入する場合、Adobeは DigiCert と連携して証明書を購入し、Adobeの実稼働サーバーにデプロイします。

   お客様が証明書（BYOC）を購入すると、Adobe クライアントケアから証明書署名リクエスト（CSR）が送信されます。 選択した認証局を通じて証明書を購入する場合は、CSR を使用します。 証明書が発行されたら、証明書のコピーおよび中間の証明書をAdobe Client Care に送信してデプロイします。

   実装の準備が整うと、Adobe Client Care から通知が届きます。

1. `serverDomain` [ ドキュメント ](../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverdomain) を新しい CNAME ホスト名に更新し、at.js 設定の `false` [ ドキュメント ](../implement/client-side/atjs/atjs-functions/targetglobalsettings.md#overridemboxedgeserver) に `overrideMboxEdgeServer` 定します。

## よくある質問

次の情報では、Target での CNAME サポートのリクエストと実装に関するよくある質問に回答します。

### 自分の証明書（Bring Your Own Certificate または BYOC）を提供できますか？

独自の証明書を指定できます。 ただし、Adobeではこの方法をお勧めしません。 Adobeと、Adobeが証明書を購入して制御する場合の両方で、SSL 証明書のライフサイクルの管理が容易になります。 SSL 証明書は、毎年更新する必要があります。 したがって、Adobe Client Care は、新しい証明書をタイムリーに取得するために、毎年あなたに連絡する必要があります。 一部のお客様では、更新された証明書をタイムリーに生成することが困難な場合があります。 ブラウザーが接続を拒否するので、証明書の有効期限が切れると、[!DNL Target] の実装が危険になります。

>[!WARNING]
>
>独自の証明書 CNAME 導入をリクエストした場合、Adobe クライアントケアに毎年、更新された証明書を提供する責任が [!DNL Target] ります。 Adobeが証明書を更新してデプロイする前に CNAME 証明書の有効期限が切れると、特定の [!DNL Target] 実装が停止します。

### 新しい SSL 証明書が期限切れになるまでの期間はどのくらいですか？

Adobeで購入した証明書はすべて 1 年間有効です。 詳しくは、[1 年証明書に関する DigiCert の記事 ](https://www.digicert.com/blog/position-on-1-year-certificates) を参照してください。

### どのホスト名を選択すればよいですか？ ドメインごとに何個のホスト名を選択すればよいですか？

Target CNAME 実装では、SSL 証明書とお客様の DNS で、ドメインごとに 1 つのホスト名のみが必要です。 Adobeでは、ドメインごとに 1 つのホスト名を使用することをお勧めします。 一部のお客様は、独自の目的（ステージングでのテストなど）のために、ドメインごとに多くのホスト名を必要とします。これはサポートされています。

ほとんどのお客様は、`target.example.com` のようなホスト名を選択しています。 Adobeでは、この方法に従うことをお勧めしますが、最終的には選択はあなた次第です。 既存の DNS レコードのホスト名は要求しないでください。 これにより、競合が発生し、[!DNL Target] CNAME 要求の解決に時間がかかる可能性があります。

### Adobe Analytics用の CNAME 実装は既にありますが、同じ証明書またはホスト名を使用できますか？

いいえ、別 [!DNL Target] ホスト名と証明書が必要です。

### [!DNL Target] の現在の実装は ITP 2.x の影響を受けますか？

Apple Intelligent Tracking Prevention （ITP）バージョン 2.3 では、CNAME クローキング軽減機能が導入されました。この機能は、[!DNL Target] CNAME 実装を検出し、cookie の有効期限を 7 日に短縮できます。 現在、[!DNL Target] には ITP の CNAME クローク軽減策がありません。 ITP について詳しくは、[Apple インテリジェント トラッキング防止機能（ITP） 2.x](../before-implement/privacy/apple-itp-2x.md) を参照してください。

### CNAME 実装がデプロイされた場合、どのようなサービスの中断が予想されますか？

証明書がデプロイされる際（証明書の更新を含む）に、サービスの中断はありません。

ただし、[!DNL Target] 実装コードのホスト名（at.js で `serverDomain`）を新しい CNAME ホスト名（`target.example.com`）に変更すると、web ブラウザーは再訪問者を新しい訪問者として扱います。 古いホスト名（`clientcode.tt.omtrdc.net`）では以前の cookie にアクセスできないので、再訪問者のプロファイルデータは失われます。 ブラウザーのセキュリティモデルが原因で、以前の cookie にアクセスできません。 この問題は、新しい CNAME への最初のカットオーバーでのみ発生します。 ホスト名は変更されないので、証明書の更新による影響は同じではありません。

### CNAME 実装で使用されるキーの種類と証明書の署名アルゴリズムは何ですか？

すべての証明書は RSA SHA-256 で、キーはデフォルトで RSA 2048 ビットです。 2048 ビットを超えるキーサイズは、現在サポートされていません。

### CNAME 実装がトラフィックに対して準備されていることを検証するにはどうすればよいですか？

次の一連のコマンドを使用します（macOSまたは Linux コマンドラインターミナルで、bash と curl >=7.49 を使用）。

1. この bash 関数をコピーして端末に貼り付けるか、bash 起動スクリプトファイル（通常は `~/.bash_profile` または `~/.bashrc`）に貼り付けて、端末セッション全体で関数を使用できるようにします。

   ```
      function adobeTargetCnameValidation {
     local hostname="$1"
     if [ -z "$hostname" ]; then
       echo "ERROR: no hostname specified"
       return 1
     fi  local service="Adobe Target CNAME implementation"
     local edges="41 42 44 45 46 47 48"
     local edgeDomain="tt.omtrdc.net"
     local edgeFormat="mboxedge%d%s.$edgeDomain"
     local poolDomain="pool.data.adobedc.net"
     local shards=5
     local shardsFoundCount=0
     local shardsFound
     local shardsFoundOutput
     local curlRegex="subject:.*CN=|expire date:|issuer:"
     local curlValidation="SSL certificate verify ok"
     local curlResponseValidation='"OK"'
     local curlEndpoint="/uptime?mboxClient=uptime3"
     local url="https://$hostname$curlEndpoint"
     local sslShopperUrl="https://www.sslshopper.com/ssl-checker.html#hostname=$hostname"
     local success="✅"
     local failure="🚫"
     local info="🔎"
     local rule="="
     local horizontalRule="$(seq ${COLUMNS:-30} | xargs printf "$rule%.0s")"
     local miniRule="$(seq 5 | xargs printf "$rule%.0s")"
     local curlVersion="$(curl --version | head -1 | cut -d' ' -f2 )"
     local curlVersionRequired=">=7.49"
     local edgeCount="$(wc -w <<< "$edges" | tr -d ' ')"
     local shard
     local currShard
     local dnsOutput
     local cnameExists
     local endToEndTestSucceeded
     local curlResult  for region in IRL1 IND1 SIN OR SYD VA TYO; do
       currShard="${region}-${poolDomain}"
       curlResult="$(curl -vsm20 --connect-to "$hostname:443:$currShard:443" "$url" 2>&1)"
       if grep -q "$curlValidation" <<< "$curlResult"; then
         shardsFound+=" $currShard"
         if grep -q "$curlResponseValidation" <<< "$curlResult"; then
           shardsFoundCount=$((shardsFoundCount+1))
           shardsFoundOutput+="\n\n$miniRule $success $hostname [edge shard: $currShard] $miniRule\n"
         else
           shardsFoundOutput+="\n\n$miniRule $failure $hostname [edge shard: $currShard] $miniRule\n"
         fi
         shardsFoundOutput+="$(grep -E "$curlRegex" <<< "$curlResult" | sort)"
         if ! grep -q "$curlResponseValidation" <<< "$curlResult"; then
           shardsFoundOutput+="\nERROR: unexpected HTTP response from this shard using $url"
         fi
       fi
     done
     echo
     echo "$horizontalRule"
     echo
     echo "$service validation for hostname $hostname:"
     dnsOutput="$(dig -t CNAME +short "$hostname" 2>&1)"
     if grep -qFi ".$edgeDomain" <<< "$dnsOutput"; then
       echo "$success $hostname passes DNS CNAME validation"
       cnameExists=true
     else
       echo -n "$failure $hostname FAILED DNS CNAME validation -- "
       if [ -n "$dnsOutput" ]; then
         echo -e "$dnsOutput is not in the subdomain $edgeDomain"
       else
         echo "required DNS CNAME record pointing to <target-client-code>.$edgeDomain not found"
       fi
     fi  for region in IRL1 IND1 SIN OR SYD VA TYO; do
       curlResult="$(curl -vsm20 --connect-to "$hostname:443:${region}-pool.data.adobedc.net:443" "https://$hostname$curlEndpoint" 2>&1)"
       if grep -q "$curlValidation" <<< "$curlResult"; then
         if grep -q "$curlResponseValidation" <<< "$curlResult"; then
           echo -en "$success $hostname passes TLS and HTTP response validation for region $region"
           if [ -n "$cnameExists" ]; then
             echo
           else
             echo " -- the DNS CNAME is not pointing to the correct subdomain for ${service}s with Adobe-managed certificates" \
               "(bring-your-own-certificate implementations don't have this requirement), but this test passes as configured"
           fi
           endToEndTestSucceeded=true
         else
           echo -n "$failure $hostname FAILED HTTP response validation for region $region --" \
             "unexpected response from $url -- "
           if [ -n "$cnameExists" ]; then
             echo "DNS is NOT pointing to the correct shard, notify Adobe Client Care"
           else
             echo "the required DNS CNAME record is missing, see above"
           fi
         fi
       else
         echo -n "$failure $hostname FAILED TLS validation for region $region -- "
         if [ -n "$cnameExists" ]; then
           echo "DNS is likely NOT pointing to the correct shard or there's a validation issue with the certificate or" \
             "protocols, see curl output below and optionally SSL Shopper ($sslShopperUrl):"
           echo ""
           echo "$horizontalRule"
           echo "$curlResult" | sed 's/^/    /g'
           echo "$horizontalRule"
           echo ""
         else
           echo "the required DNS CNAME record is missing, see above"
         fi
       fi
     done  if [ "$shardsFoundCount" -ge "$edgeCount" ]; then
       echo -n "$success $hostname passes shard validation for the following $shardsFoundCount edge shards:"
       echo -e "$shardsFoundOutput"
       echo    if [ -n "$cnameExists" ] && [ -n "$endToEndTestSucceeded" ]; then
         echo "$horizontalRule"
         echo ""
         echo "  For additional TLS/SSL validation, see SSL Shopper:"
         echo ""
         echo "    $info  $sslShopperUrl"
         echo ""
         echo "  To check DNS propagation around the world, see whatsmydns.net:"
         echo ""
         echo "    $info  DNS A records:     https://whatsmydns.net/#A/$hostname"
         echo "    $info  DNS CNAME record:  https://whatsmydns.net/#CNAME/$hostname"
       fi
     else
       echo -n "$failure $hostname FAILED shard validation -- shards found: $shardsFoundCount," \
         "expected: $edgeCount"
       if bc -l <<< "$(cut -d. -f1,2 <<< "$curlVersion") $curlVersionRequired" 2>/dev/null | grep -q 0; then
         echo -n " -- insufficient curl version installed: $curlVersion, but this script requires curl version" \
           "$curlVersionRequired because it uses the curl --connect-to flag to bypass DNS and directly test" \
           "each Adobe Target edge shards' SNI configuration for $hostname"
       fi
       if [ -n "$shardsFoundOutput" ]; then
         echo -e ":\n$shardsFoundOutput"
       fi
       echo
     fi
     echo
     echo "$horizontalRule"
     echo
   }
   ```

1. 次のコマンドを貼り付けます（`target.example.com` をホスト名に置き換えます）。

   ```
   adobeTargetCnameValidation target.example.com
   ```

   実装の準備が整ったら、次のような出力が表示されます。 重要な部分は、すべての検証ステータス行に `🚫` ではなく `✅` が表示されることです。 各 Target Edge CNAME シャードは、要求された証明書のプライマリ ホスト名に一致する `CN=target.example.com` を表示する必要があります（この出力には、証明書の追加の SAN ホスト名は出力されません）。

   ```
      $ adobeTargetCnameValidation 
    target.example.com==========================================================Adobe Target CNAME implementation validation for hostname target.example.com:
    ✅ target.example.com passes DNS CNAME validation
    ✅ target.example.com passes TLS and HTTP response validation for region IRL1
    ✅ target.example.com passes TLS and HTTP response validation for region IND1
    ✅ target.example.com passes TLS and HTTP response validation for region SIN
    ✅ target.example.com passes TLS and HTTP response validation for region OR
    ✅ target.example.com passes TLS and HTTP response validation for region SYD
    ✅ target.example.com passes TLS and HTTP response validation for region VA
    ✅ target.example.com passes TLS and HTTP response validation for region TYO
    ✅ target.example.com passes shard validation for the following 7 edge shards:===== ✅ target.example.com [edge shard: IRL1-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: IND1-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: SIN-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: OR-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: SYD-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: VA-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com===== ✅ target.example.com [edge shard: TYO-pool.data.adobedc.net] =====
    *  expire date: Feb 20 23:59:59 2026 GMT
    *  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
    *  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com==========================================================  For additional TLS/SSL validation, see SSL Shopper:    🔎  https://www.sslshopper.com/ssl-checker.html#hostname=target.example.com  To check DNS propagation around the world, see whatsmydns.net:    🔎  DNS A records:     https://whatsmydns.net/#A/target.example.com
        🔎  DNS CNAME record:  https://whatsmydns.net/#CNAME/target.example.com 
   ```

>[!NOTE]
>
>この検証コマンドが DNS 検証時に失敗しても、必要な DNS 変更を既に行っている場合は、DNS 更新が完全に反映されるまで待つ必要がある可能性があります。 DNS レコードには、レコードの DNS 返信のキャッシュ有効期限を指示する [TTL （time-to-live） ](https://en.wikipedia.org/wiki/Time_to_live#DNS_records) が関連付けられています。 その結果、少なくとも TTL の期間は待機する必要がある場合があります。 `dig target.example.com` コマンドまたは [G Suite ツールボックス ](https://toolbox.googleapps.com/apps/dig/#CNAME) を使用して、特定の TTL を検索できます。 世界中での DNS の伝播を確認するには、[whatsmydns.net](https://whatsmydns.net/#CNAME) を参照してください。

### CNAME でのオプトアウトリンクの使用方法

CNAME を使用している場合、オプトアウトリンクには&quot;client=`clientcode` パラメーターを含める必要があります。次に例を示します。
`https://my.cname.domain/optout?client=clientcode`。

`clientcode` をお客様のクライアントコードに置き換えてから、この [ オプトアウト URL](privacy/privacy.md) にリンクさせるテキストまたは画像を追加します。

## 既知の制限事項

* CNAME および at.js 1.x はサードパーティの cookie に基づいているので、QA モードは動的に変更されます。 これを回避するには、移動先の各 URL にプレビューパラメーターを追加します。 CNAME および at.js 2.x を使用している場合、QA モードは sticky です。
* CNAME を使用すると、[!DNL Target] 呼び出しの cookie ヘッダーのサイズが増加する可能性が高くなります。 Adobeでは、cookie のサイズを 8 KB 未満に抑えることをお勧めします。
