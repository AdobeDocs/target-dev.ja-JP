---
title: ' [!DNL Adobe Target] Java SDK での非同期要求の使用方法'
description: Java SDK が  [!DNL Target]  非同期リクエストをサポートする方法を説明します。これにより、有効なターゲット時間をゼロに減らすことができます。
feature: APIs/SDKs
exl-id: e11f8d16-76f6-4d39-822a-34a1cf7f623f
source-git-commit: e5bae1ac9485c3e1d7c55e6386f332755196ffab
workflow-type: tm+mt
source-wordcount: '130'
ht-degree: 3%

---

# 非同期要求（Java）

## 説明

サーバーサイド統合の利点の 1 つは、並列処理を使用することで、サーバーサイドで利用できる膨大な帯域幅とコンピューティングリソースを活用できる点です。 Java SDK[!DNL Target]、非同期リクエストをサポートしており、有効ターゲット時間をゼロに減らすことができます。

## サポートされるメソッド

### メソッド

```javascript {line-numbers="true"}
CompletableFuture<TargetDeliveryResponse> getOffersAsync(TargetDeliveryRequest request);
CompletableFuture<ResponseStatus> sendNotificationsAsync(TargetDeliveryRequest request);
CompletableFuture<Attributes> getAttributesAsync(TargetDeliveryRequest targetRequest, String ...mboxes);
```

## 例

`Spring` アプリケーションコントローラーの例は次のようになります。

### サンプル コントローラ

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

この例では、SDK を [ 初期化 ](initialize-sdk.md) して Spring Bean とし、[ ユーティリティメソッド ](utility-methods.md) を使用できることを前提としています。

[!DNL Target] リクエストは `simulateIO` の前に実行され、実行される時点で、ターゲット結果も準備が整っています。 そうでない場合でも、ほとんどの場合、大幅に節約できます。
