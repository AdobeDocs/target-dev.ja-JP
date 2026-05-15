---
keywords: クライアント ケア；cname；証明書プログラム；正規名；cookie；証明書；amc;adobe管理証明書；digicert；ドメイン制御検証；dcv
description: 広告ブロックの問題を処理するために、 [!DNL Adobe]  クライアントケアと連携して、 [!DNL Adobe Target] でCNAME （正規名）サポートを実装します。
title: TargetでCNAMEを使用するにはどうすればよいですか？
feature: Privacy & Security
role: Developer
exl-id: bf533771-6d46-48ba-964c-3ad9ce9f7352
TQID: https://experienceleague.adobe.com/gTS60hypD2WGc2fJh-sUkq2-pkzt2KnM4CzSQ050L40
product_v2:
  - id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
feature_v2:
  - id: c93393a4-e558-47e1-992e-c91ed4d480ce
subfeature_v2:
  - id: fd0ff162-b6d3-4a11-8aeb-e165a01c0f0a
role_v2:
  - id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
topic_v2:
  - id: b5ce8718-c3af-4fdb-a1a9-fca32f83a87c
  - id: d095671a-1355-40aa-8b5f-06c33c68080b
  - id: f4e6943a-c91a-4134-a2c7-f4f20cfff2f0
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 1313
ht-degree: 1%

---

# CNAMEと[!DNL Target]

[!DNL Adobe Target]でCNAME （正規の名前）のサポートを実装するために[!DNL Adobe] Client Careで作業する手順を説明します。 CNAMEを使用して、広告ブロックの問題またはITP関連（インテリジェント トラッキング防止） Cookie ポリシーを処理します。 CNAMEを使用すると、呼び出しは、[!DNL Adobe]が所有するドメインではなく、顧客が所有するドメインに対して行われます。

## [!DNL Target]でのCNAME サポートのリクエスト

1. SSL証明書に必要なホスト名のリストを決定します（以下のFAQを参照）。
1. [このフォーム &#x200B;](/help/dev/implement/assets/FPC_Request_Form.xlsx)に入力し、CNAME サポートをリクエストする [!DNL Adobe]  クライアントケアチケットを[開いたときにフォームを含めます](https://experienceleague.adobe.com/en/docs/target/using/cmp-resources-and-contact-information#reference_ACA3391A00EF467B87930A450050077C):

   * [!DNL Adobe Target] クライアント コード：
   * SSL証明書ホスト名（例：`target.example.com target.example.org`）:
   * SSL証明書の購入者（[!DNL Adobe]を強くお勧めします。FAQを参照）: Adobe/お客様
   * お客様が「自分の証明書を持参」（BYOC）とも呼ばれる証明書を購入する場合は、以下の追加情報を入力します。

      * 証明書の整理（例：会社の例）:
      * 証明書の組織単位（オプション、例：マーケティング）:
      * 証明書国（例：米国）:
      * 証明書の状態/地域（例：カリフォルニア）:
      * 証明書都市（例：サンノゼ）:

1. ホスト名リクエストごとに、Adobeが実装を作成し、作成するCNAME レコード名を返します。このレコードには、`tt.omtrdc.net`でサフィックスされたランダムな文字列が含まれます

   例えば、`target.example.com`のリクエストを行った場合、`abcdefgh.tt.omtrdc.net`の形式でCNAMEを返送します。 DNS CNAME レコードは次のようになります。

   ```
   target.example.com.  IN  CNAME  abcdefgh.tt.omtrdc.net.
   ```

   >[!IMPORTANT]
   >
   >この手順が完了するまで、[!DNL Adobe]の認証局であるDigiCertは証明書を発行できません。 したがって、この手順が完了するまで、[!DNL Adobe]はCNAME実装の要求を満たすことができません。

1. [!DNL Adobe]が証明書を購入する場合、[!DNL Adobe]はDigiCertと連携して、[!DNL Adobe]の実稼動サーバーに証明書を購入してデプロイします。

   お客様が証明書（BYOC）を購入している場合、[!DNL Adobe] Client Careは証明書の署名要求（CSR）を送信します。 選択した認証局を通じて証明書を購入する場合は、CSRを使用します。 証明書が発行されたら、証明書のコピーと中間証明書を[!DNL Adobe] Client Careに送信してデプロイメントします。

   実装の準備ができたら、[!DNL Adobe] クライアント ケアから通知が受け取られます。

1. `serverDomain` （[documentation](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#serverDomain)）を新しいCNAME ホスト名に更新し、at.js設定で`overrideMboxEdgeServer`を`false` （[documentation](/help/dev/implement/client-side/atjs/atjs-functions/targetglobalsettings.md#overridemboxedgeserver)）に設定します。

## よくある質問

次の情報は、[!DNL Target]でのCNAME サポートのリクエストと実装に関するよくある質問に回答します。

### 自分の証明書（自分の証明書またはBYOCを持参）を提供できますか？

独自の証明書を指定できます。 ただし、[!DNL Adobe]はこの方法を強くお勧めします。 SSL証明書のライフサイクルの管理は、[!DNL Adobe]が証明書を購入して管理している場合は、[!DNL Adobe]とユーザーの両方にとって簡単です。 SSL証明書の有効期間は、時間が短くなります（証明書の有効期間に関する次の節を参照）。 したがって、[!DNL Adobe] クライアント ケアは、新しい証明書をタイムリーに取得するために、毎回あなたに連絡する必要があります。 証明書の有効期間がわずか47日に短縮されると、この問題は課題であることが証明されます。 ブラウザーが接続を拒否するため、証明書の有効期限が切れると、[!DNL Target]の実装が危険にさらされます。

>[!IMPORTANT]
>
>[!DNL Target]独自の証明書を持ち込むCNAME実装をリクエストする場合は、有効期限が切れるたびに[!DNL Adobe] クライアントケアに新しい証明書を提供する責任があります。 [!DNL Adobe]が更新された証明書をデプロイする前にCNAME証明書の有効期限を許可すると、特定の[!DNL Target]実装が停止します。

### 新しいSSL証明書が期限切れになるまでにどのくらいの時間がかかりますか？

すべての証明書の有効期間は、証明機関による主要な取り組みの一環として短くなります。 DigiCert、[!DNL Adobe]の証明書プロバイダーの場合、次のスケジュールが適用されます。

2026年3月15日まで、TLS証明書の最大有効期間は398日です。
2026年3月15日の時点で、TLS証明書の最大有効期間は200日になります。
2027年3月15日の時点で、TLS証明書の最大有効期間は100日になります。
2029年3月15日現在、TLS証明書の最大有効期間は47日です。
詳しくは、証明書の有効期間の短縮に関する[DigiCertの記事を参照してください](https://www.digicert.com/blog/tls-certificate-lifetimes-will-officially-reduce-to-47-days)

### どのホスト名を選択すればよいですか？ ドメインごとにいくつのホスト名を選択すればよいですか？

[!DNL Target]件のCNAME実装では、SSL証明書および顧客のDNSで、ドメインごとに1つのホスト名のみを必要とします。 [!DNL Adobe]は、ドメインごとに1つのホスト名を推奨しています。 一部の顧客は、独自の目的のためにドメインごとに多くのホスト名を必要とします（ステージングのテストなど）。これはサポートされています。

ほとんどの顧客は、`target.example.com`のようなホスト名を選択します。 [!DNL Adobe]さんはこの方法を推奨していますが、選択は最終的にあなたのものです。 既存のDNS レコードのホスト名を要求しないでください。 これにより、競合が発生し、[!DNL Target] CNAME リクエストの解決に時間がかかります。

### 既に[!DNL Adobe Analytics]のCNAME実装がありますが、同じ証明書またはホスト名を使用できますか？

いいえ、[!DNL Target]には別のホスト名と証明書が必要です。

### 現在の[!DNL Target]の実装はITP 2.xの影響を受けますか？

Apple Intelligent Tracking Prevention （ITP）バージョン 2.3では、CNAME クローキング緩和機能が導入されました。この機能は、[!DNL Adobe Target]個のCNAME実装を検出でき、Cookieの有効期限を7日に短縮します。 現在[!DNL Target]には、ITPのCNAME クローキングの緩和策はありません。 ITPについて詳しくは、[Apple Intelligent Tracking Prevention （ITP） 2.x](/help/dev/before-implement/privacy/apple-itp-2x.md)を参照してください。

### CNAME実装がデプロイされると、どのようなサービスの中断が予想されますか？

証明書をデプロイしても、サービスの中断はありません（証明書の更新を含む）。

ただし、実装コード [!DNL Target]のホスト名（at.jsの`serverDomain`）を新しいCNAME ホスト名（`target.example.com`）に変更すると、web ブラウザーは再訪問者を新規訪問者として扱います。 以前のCookieが古いホスト名（`clientcode.tt.omtrdc.net`）でアクセスできないため、再訪問者のプロファイルデータが失われます。 ブラウザーセキュリティモデルにより、以前のCookieにアクセスできません。 この中断は、新しいCNAMEへの最初のカットオーバーでのみ発生します。 ホスト名が変更されないため、証明書の更新は同じ効果を持ちません。

### CNAME実装にはどのようなキータイプと証明書の署名アルゴリズムが使用されますか？

すべての証明書はRSA SHA-256で、キーはデフォルトでRSA 2048 ビットです。 2048 ビットを超えるキーサイズは、カスタマーケアを通じて明示的に要求する必要があります。

### CNAME実装がトラフィックの準備ができていることを検証するにはどうすればよいですか？

次の一連のコマンドを使用します（macOSまたはLinux コマンドラインターミナルでは、bashとcurl >=7.49を使用します）。

1. このbash関数をコピーして端末に貼り付けるか、bash起動スクリプトファイル（通常は`~/.bash_profile`または`~/.bashrc`）に関数を貼り付けて、端末セッション間で関数を利用できるようにします。

```
function adobeTargetCnameValidation {
  local hostname="$1"
  
  if [ -z "$hostname" ]; then
    echo "ERROR: no hostname specified"
    return 1
  fi
  
  local service="Adobe Target CNAME implementation"
  local edges="41 42 44 45 46 47 48"
  local edgeDomain="tt.omtrdc.net"
  local edgeFormat="mboxedge%d%s.$edgeDomain"
  local poolDomain="pool.data.adobedc.net"
  local shards=5
  local shardsFoundCount=0
  local shardsFound=""
  local shardsFoundOutput=""
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
  local curlVersion="$(curl --version | head -1 | cut -d' ' -f2)"
  local curlVersionRequired=7.49
  local edgeCount="$(wc -w <<< "$edges" | tr -d ' ')"
  local cnameExists=""
  local endToEndTestSucceeded=""
  
  for region in IRL1 IND1 SIN OR SYD VA TYO; do
    local currShard="${region}-${poolDomain}"
    local curlResult="$(curl -vsm20 --connect-to "$hostname:443:$currShard:443" "$url" 2>&1)"
    
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
  
  local dnsOutput="$(dig -t CNAME +short "$hostname" 2>&1)"
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
  fi
  
  for region in IRL1 IND1 SIN OR SYD VA TYO; do
    local curlResult="$(curl -vsm20 --connect-to "$hostname:443:${region}-pool.data.adobedc.net:443" "https://$hostname$curlEndpoint" 2>&1)"
    
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
  done
  
  if [ "$shardsFoundCount" -ge "$edgeCount" ]; then
    echo -n "$success $hostname passes shard validation for the following $shardsFoundCount edge shards:"
    echo -e "$shardsFoundOutput"
    echo
    
    if [ -n "$cnameExists" ] && [ -n "$endToEndTestSucceeded" ]; then
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
    echo ""
  fi
  
  echo
  echo "$horizontalRule"
  echo
}  
```

1. このコマンドを貼り付けます（`target.example.com`をホスト名に置き換えます）。

   ```
   adobeTargetCnameValidation target.example.com
   ```

   実装の準備ができたら、次のような出力が表示されます。 重要なのは、すべての検証ステータス行に`🚫`ではなく`✅`が表示されることです。 各[!DNL Target] エッジ CNAME シャードには、`CN=target.example.com`が表示されます。これは、要求された証明書のプライマリホスト名と一致します（証明書の追加のSAN ホスト名はこの出力に表示されません）。

```
$ adobeTargetCnameValidation target.example.com

==========================================================

Adobe Target CNAME implementation validation for hostname target.example.com:
✅ target.example.com passes DNS CNAME validation
✅ target.example.com passes TLS and HTTP response validation for region IRL1
✅ target.example.com passes TLS and HTTP response validation for region IND1
✅ target.example.com passes TLS and HTTP response validation for region SIN
✅ target.example.com passes TLS and HTTP response validation for region OR
✅ target.example.com passes TLS and HTTP response validation for region SYD
✅ target.example.com passes TLS and HTTP response validation for region VA
✅ target.example.com passes TLS and HTTP response validation for region TYO
✅ target.example.com passes shard validation for the following 7 edge shards:

===== ✅ target.example.com [edge shard: IRL1-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: IND1-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: SIN-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: OR-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: SYD-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: VA-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

===== ✅ target.example.com [edge shard: TYO-pool.data.adobedc.net] =====
*  expire date: Feb 20 23:59:59 2026 GMT
*  issuer: C=US; O=DigiCert Inc; CN=DigiCert Global G2 TLS RSA SHA256 2020 CA1
*  subject: C=US; ST=California; L=San Jose; O=Adobe Systems Incorporated; CN=target.example.com

==========================================================  

For additional TLS/SSL validation, see SSL Shopper:

    🔎  https://www.sslshopper.com/ssl-checker.html#hostname=target.example.com  

To check DNS propagation around the world, see whatsmydns.net:

    🔎  DNS A records:     https://whatsmydns.net/#A/target.example.com
    🔎  DNS CNAME record:  https://whatsmydns.net/#CNAME/target.example.com 
```

>[!NOTE]
>
>この検証コマンドがDNS検証で失敗したが、必要なDNS変更を既に行っている場合は、DNS更新が完全に反映されるのを待つ必要がある場合があります。 DNS レコードには、これらのレコードのDNS応答のキャッシュ有効期限を決定する[TTL （有効期間） &#x200B;](https://en.wikipedia.org/wiki/Time_to_live#DNS_records)が関連付けられています。 その結果、少なくともTTLに対応できる限り待つ必要があるかもしれません。 `dig target.example.com` コマンドまたは[G Suite Toolbox](https://toolbox.googleapps.com/apps/dig/#CNAME)を使用して、特定のTTLを検索できます。 世界中のDNSの伝播を確認するには、[whatsmydns.net](https://whatsmydns.net/#CNAME)を参照してください。

### CNAME でのオプトアウトリンクの使用方法

CNAMEを使用している場合、オプトアウトリンクには「client=`clientcode`」パラメーターを含める必要があります。例：
`https://my.cname.domain/optout?client=clientcode`.

`clientcode`をクライアントコードに置き換え、[&#x200B; オプトアウト URL](/help/dev/before-implement/privacy/privacy.md)にリンクするテキストまたは画像を追加します。

## 既知の制限事項

* CNAMEとat.js 1.xがある場合、サードパーティ Cookieに基づいているため、QA モードはスティッキーではありません。 この問題を回避するには、移動先の各URLにプレビューパラメーターを追加します。 CNAMEとat.js 2.xがある場合、QA モードはスティッキーです。
* CNAMEを使用する場合、[!DNL Target]呼び出しのcookie ヘッダーのサイズが増加する可能性が高くなります。 [!DNL Adobe]では、Cookie サイズを8 KB未満に保つことをお勧めします。
