---
title: で非同期リクエストを使用する方法 [!DNL Adobe Target] Java SDK
description: 方法を学ぶ [!DNL Target] Java SDK は非同期リクエストをサポートするので、有効なターゲット時間をゼロに抑えることができます。
feature: APIs/SDKs
exl-id: e11f8d16-76f6-4d39-822a-34a1cf7f623f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 3%

---

# 非同期リクエスト (Java)

## 説明

サーバー側の統合の利点の 1 つは、並列処理を使用することで、サーバー側で利用可能な大量の帯域幅とコンピューティングリソースを活用できる点です。 [!DNL Target] Java SDK は非同期リクエストをサポートするので、有効なターゲット時間をゼロに抑えることができます。

## サポートされるメソッド

### メソッド

```javascript {line-numbers="true"}
CompletableFuture<TargetDeliveryResponse> getOffersAsync(TargetDeliveryRequest request);
CompletableFuture<ResponseStatus> sendNotificationsAsync(TargetDeliveryRequest request);
CompletableFuture<Attributes> getAttributesAsync(TargetDeliveryRequest targetRequest, String ...mboxes);
```

## 例

サンプル `Spring` アプリケーションコントローラーは次のようになります。

### サンプルコントローラ

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

この例では、 [SDK を初期化しました](initialize-sdk.md) 春の豆として、そしてあなたが持っている [ユーティリティメソッド](utility-methods.md) 使用可能

The [!DNL Target] リクエストが次より前に実行される： `simulateIO` また、実行されるまでには、ターゲット結果も準備が整っている必要があります。 そうでなくても、ほとんどの場合、大幅な節約になります。
