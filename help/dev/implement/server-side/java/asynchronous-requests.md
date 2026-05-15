---
title: ' [!DNL Adobe Target] Java SDKでの非同期リクエストの使用方法'
description: ' [!DNL Target] Java SDKが非同期リクエストをサポートする方法を説明します。これにより、効果的な目標時間を0に短縮できます。'
feature: APIs/SDKs
exl-id: e11f8d16-76f6-4d39-822a-34a1cf7f623f
TQID: https://experienceleague.adobe.com/Y9oTl8aU4-4HpMajdmy5KfvAwEFOpV0y9vUg7BdBRk8
product_v2: id: e43347a8-f2c5-4aa4-8623-6f13875d7e3a
role_v2: id: ff6a42d2-313e-452e-93a6-792e4fad9ff8
source-git-commit: 07d73101a14b986fa9b016350c1ddeac0df4fdc2
workflow-type: tm+mt
source-wordcount: 130
ht-degree: 3%

---

# 非同期要求（Java）

## 説明

サーバーサイド統合の利点の1つは、並列処理を使用して、サーバーサイドで利用可能な巨大な帯域幅とコンピューティングリソースを活用できることです。[!DNL Target] Java SDKは非同期リクエストをサポートしており、これにより効果的なターゲットタイムをゼロにすることができます。

## サポートされるメソッド

### メソッド

```javascript {line-numbers="true"}
CompletableFuture<TargetDeliveryResponse> getOffersAsync(TargetDeliveryRequest request);
CompletableFuture<ResponseStatus> sendNotificationsAsync(TargetDeliveryRequest request);
CompletableFuture<Attributes> getAttributesAsync(TargetDeliveryRequest targetRequest, String ...mboxes);
```

## 例

サンプル `Spring` アプリケーション コントローラーは次のようになります。

### サンプルコントローラー

```javascript {line-numbers="true"}
@RestController
public class TargetRestController {

    @Autowired
    private TargetClient targetJavaClient;

    @GetMapping("/mboxTargetOnlyAsync")
        public TargetDeliveryResponse mboxTargetOnlyAsync(
                @RequestParam(name = "mbox", defaultValue = "server-side-mbox") String mbox,
                HttpServletRequest request, HttpServletResponse response) {
            ExecuteRequest executeRequest = new ExecuteRequest()
                    .mboxes(getMboxRequests(mbox));

            TargetDeliveryRequest targetDeliveryRequest = TargetDeliveryRequest.builder()
                    .context(getContext(request))
                    .execute(executeRequest)
                    .cookies(getTargetCookies(request.getCookies()))
                    .build();
            CompletableFuture<TargetDeliveryResponse> targetResponseAsync =
                    targetJavaClient.getOffersAsync(targetDeliveryRequest);
            targetResponseAsync.thenAccept(tr -> setCookies(tr.getCookies(), response));
            simulateIO();
            TargetDeliveryResponse targetResponse = targetResponseAsync.join();
            return targetResponse;
        }

    /**
     * Function for simulating network calls like other microservices and database calls
     */
    private void simulateIO() {
        try {
            Thread.sleep(200L);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

}
```

この例では、[SDK](initialize-sdk.md)をスプリング Beanとして初期化しており、[ ユーティリティメソッド ](utility-methods.md)が使用可能であると仮定します。

[!DNL Target] リクエストは`simulateIO`の前に実行され、実行されるまでにターゲット結果も準備が整っている必要があります。 そうでなくても、ほとんどの場合、大幅な節約になります。
