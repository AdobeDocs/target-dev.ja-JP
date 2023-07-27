---
title: イベントトラッキング
description: 用途 [!DNL Adobe Target]のイベントトラッキング機能を使用して、お客様のビジネスや使用例にとって最も重要な指標を効果的に測定できます。
exl-id: a47fa692-c633-4c53-82da-878b1e451a3f
feature: Implement Server-side
source-git-commit: 09a50aa67ccd5c687244a85caad24df56c0d78f5
workflow-type: tm+mt
source-wordcount: '527'
ht-degree: 3%

---

# イベントトラッキング

用途 [!DNL Adobe Target]のイベントトラッキング機能を使用して、お客様のビジネスや使用例にとって最も重要な指標を効果的に測定できます。 イベントのトラッキングは、勝者または敗者のバリエーションやエクスペリエンスがわかるので、実験やパーソナライゼーションアクティビティの成功を測定するための重要な要素です。 これを理解すると、ユーザーが製品に対してどのように関わり、または絶え間なく変化する状況で進化しているかを理解するのに役立ちます。

を通じてイベントを追跡するには [!DNL Adobe Target]の SDK では、次の 2 つの手順に従います。

1. SDK をインストールし、にイベントを送信するコードをデプロイする [!DNL Adobe Target].

1. の作成とアクティブ化 [!DNL Adobe Target] アクティビティの作成を指定します。

   ![代替画像](./assets/report-settings.png)

## 目標指標およびイベント

次の表に、 [!DNL Target] 使用するアクティビティ [!DNL Target]のレポート機能は次のとおりです。

| プライマリ目標 | イベント |
| --- | --- |
| コンバージョン | ページが表示された、mbox が表示された、mbox がクリックされた |
| 売上高 | mbox が表示され、mbox がクリックされた |
| エンゲージメント | ページビュー数、顧客スコア付け、サイト滞在時間 |

## インプレッションのトリガー方法

Target SDK は基になる [配信 API](/help/dev/implement/delivery-api/overview.md). 必要なパラメーターを持つ execute オブジェクトがリクエスト自体内にある場合、インプレッションは、条件を満たすアクティビティに対して自動的に増分されます。 インプレッションを自動的に増分する SDK メソッドは次のとおりです。

* getOffers（）
* getAttributes()

>[!NOTE]
>
>プリフェッチオブジェクトがリクエスト内で渡された場合、プリフェッチオブジェクト内の mbox を含むアクティビティに対しては、インプレッションは自動的には増分されません。

The `sendNotifications` メソッドを使用して、に手動でイベントを送信できます。 [!DNL Adobe Target] そしてトリガーと印象。

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
TargetClient.sendNotifications(options: Object): Promise
```

>[!TAB Java]

```java {line-numbers="true"}
ResponseStatus TargetClient.sendNotifications(TargetDeliveryRequest request)
```

>[!ENDTABS]

## サンプルコード

次のコードサンプルは、コンバージョン、売上高、エンゲージメントのどれであっても、すべての目標指標タイプに対して機能します。

### ページまたは mbox が表示された

このサンプルでは、最初に `getOffers`. その後、その mbox オファーに基づく通知を使用してリクエストを作成します。

通知 `type` プロパティはに設定されます。 `display`.

ページが表示されたことを示すには、通知ペイロードで address オブジェクトを指定することが重要です。 URL は適宜設定してください。

mbox の場合、通知オブジェクトの mbox プロパティを設定し、 `targetResult`.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const { v4: uuidv4 } = require("uuid");

const client = TargetClient.create({
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg",
  events: { clientReady: onTargetReady },
});

async function onTargetReady() {
  const targetResult = await client.getOffers({
    request: {
      targetRequest,
      prefetch: {
        mboxes: [
          {
            name: "homepage",
            index: 1
          }
        ]
      },
      sessionId: uuidv4()
    }
  });

  const { mboxes = [] } = targetResult.response.prefetch;

  const request = {
    context: { channel: "web" },
    notifications: mboxes.map(mbox => {
      const { options = [] } = mbox;

      return {
        id: targetResult.response.id,
        impressionId: uuidv4(),
        address: {
          url: "http://www.target-demo-site.com"
        },
        timestamp: new Date().getTime(),
        type: "display",
        mbox: {
          name: mbox.name
        },
        tokens: options.map(option => option.eventToken)
      };
    })
  };
  // send the notification event
  await client.sendNotifications({ request });
}
```

>[!TAB Java]

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
                .client("acmeclient")
                .organizationId("1234567890@AdobeOrg")
                .build();

TargetClient targetClient = TargetClient.create(clientConfig);

Context context = new Context()
        .channel(ChannelType.WEB)
        .address(new Address().url("http://www.target-demo-site.com"));

TargetDeliveryResponse targetResult = targetJavaClient.getOffers(TargetDeliveryRequest.builder()
        .context(context
        )
        .prefetch(new PrefetchRequest()
                .mboxes(new ArrayList() {{
                    add(new MboxRequest().name("homepage").index(1));
                }})
        )
        .build());

List<Notification> notifications = new ArrayList<>();
List<PrefetchMboxResponse> mboxes = targetResult.getResponse().getPrefetch().getMboxes();

for (PrefetchMboxResponse mbox : mboxes) {
    List<Option> options = mbox.getOptions();

    notifications.add((Notification) new Notification()
            .id(targetResult.getResponse().getRequestId())
            .impressionId(UUID.randomUUID().toString())
            .timestamp(System.currentTimeMillis())
            .type(MetricType.DISPLAY)
            .mbox(new NotificationMbox().name(mbox.getName()))
            .tokens(options.stream().map(Option::getEventToken).collect(Collectors.toList()))
            .address(new Address().url("http://www.target-demo-site.com"))
    );
}

TargetDeliveryRequest notificationRequest = TargetDeliveryRequest.builder()
        .context(context)
        .notifications(notifications).build();

targetJavaClient.sendNotifications(notificationRequest);
```

>[!ENDTABS]

### mbox をクリック済み

このサンプルでは、最初に `getOffers`. その後、その mbox オファーに基づく通知を使用してリクエストを作成します。

通知 `type` プロパティはに設定されます。 `click`.

次の項目を設定する必要があります。 `mbox` プロパティを通知オブジェクトに設定し、 `targetResult`.

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const { v4: uuidv4 } = require("uuid");

const client = TargetClient.create({
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg",
  events: { clientReady: onTargetReady },
});

async function onTargetReady() {
  const targetResult = await client.getOffers({
    request: {
      targetRequest,
      prefetch: {
        mboxes: [
          {
            name: "homepage",
            index: 1
          }
        ]
      },
      sessionId: uuidv4()
    }
  });

  const { mboxes = [] } = targetResult.response.prefetch;

  const request = {
    context: { channel: "web" },
    notifications: mboxes.map(mbox => {
      const { options = [], metrics = [] } = mbox;

      return {
        id: targetResult.response.id,
        impressionId: uuidv4(),
        address: {
          url: "http://www.target-demo-site.com"
        },
        timestamp: new Date().getTime(),
        type: "click",
        mbox: {
          name: mbox.name
        },
        tokens: metrics
                  .filter(metric => metric.type === "click")
                  .map(metric => metric.eventToken)
      };
    })
  };
  // send the notification event
  await client.sendNotifications({ request });
}
```

>[!TAB Java]

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
                .client("acmeclient")
                .organizationId("1234567890@AdobeOrg")
                .build();

TargetClient targetClient = TargetClient.create(clientConfig);

Context context = new Context()
        .channel(ChannelType.WEB)
        .address(new Address().url("http://www.target-demo-site.com"));

TargetDeliveryResponse targetResult = targetJavaClient.getOffers(TargetDeliveryRequest.builder()
        .context(context
        )
        .prefetch(new PrefetchRequest()
                .mboxes(new ArrayList() {{
                    add(new MboxRequest().name("homepage").index(1));
                }})
        )
        .build());

List<Notification> notifications = new ArrayList<>();
List<PrefetchMboxResponse> mboxes = targetResult.getResponse().getPrefetch().getMboxes();

for (PrefetchMboxResponse mbox : mboxes) {
    List<Metric> metrics = mbox.getMetrics();

    notifications.add((Notification) new Notification()
            .id(targetResult.getResponse().getRequestId())
            .impressionId(UUID.randomUUID().toString())
            .timestamp(System.currentTimeMillis())
            .type(MetricType.CLICK)
            .mbox(new NotificationMbox().name(mbox.getName()))
            .tokens(metrics.stream()
                    .filter(metric -> MetricType.CLICK.equals(metric.getType()))
                    .map(Metric::getEventToken)
                    .collect(Collectors.toList()))
            .address(new Address().url("http://www.target-demo-site.com"))
    );
}

TargetDeliveryRequest notificationRequest = TargetDeliveryRequest.builder()
        .context(context)
        .notifications(notifications).build();

targetJavaClient.sendNotifications(notificationRequest);
```

>[!ENDTABS]

### 表示済み

このサンプルでは、最初に `getOffers`. その後、これらのビューに基づいて通知付きのリクエストを作成します。

通知 `type` プロパティはに設定されます。 `display`.

ビューの場合、 `view` プロパティを通知オブジェクトに設定し、 targetResult 内の options 配列に基づいてトークンの配列を指定します。

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const { v4: uuidv4 } = require("uuid");

const client = TargetClient.create({
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg",
  events: { clientReady: onTargetReady },
});

async function onTargetReady() {
  const targetResult = await client.getOffers({
    request: {
      targetRequest,
      prefetch: {
        views: [{}]
      },
      sessionId: uuidv4()
    }
  });

  const { views = [] } = targetResult.response.prefetch;

  const request = {
    context: { channel: "web" },
    notifications: views.map(view => {
      const { options = [], metrics = [] } = view;

      return {
        id: targetResult.response.id,
        impressionId: uuidv4(),
        address: {
          url: "http://www.target-demo-site.com"
        },
        timestamp: new Date().getTime(),
        type: "display",
        view: {
          name: view.name
        },
        tokens: options.map(option => option.eventToken)
      };
    })
  };
  // send the notification event
  await client.sendNotifications({ request });
}
```

>[!TAB Java]

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
                .client("acmeclient")
                .organizationId("1234567890@AdobeOrg")
                .build();

TargetClient targetClient = TargetClient.create(clientConfig);

Context context = new Context()
        .channel(ChannelType.WEB)
        .address(new Address().url("http://www.target-demo-site.com"));

TargetDeliveryResponse targetResult = targetJavaClient.getOffers(TargetDeliveryRequest.builder()
        .context(context)
        .prefetch(new PrefetchRequest()
                .views(new ArrayList() {{
                    add(new ViewRequest());
                }})
        )
        .build());

List<Notification> notifications = new ArrayList<>();
List<View> views = targetResult.getResponse().getPrefetch().getViews();

for (View view : views) {
    List<Option> options = view.getOptions();
    List<Metric> metrics = view.getMetrics();

    notifications.add((Notification) new Notification()
            .id(targetResult.getResponse().getRequestId())
            .impressionId(UUID.randomUUID().toString())
            .timestamp(System.currentTimeMillis())
            .type(MetricType.DISPLAY)
            .view(new NotificationView().name(view.getName()))
            .tokens(options.stream().map(Option::getEventToken).collect(Collectors.toList()))
            .address(new Address().url("http://www.target-demo-site.com"))
    );
}

TargetDeliveryRequest notificationRequest = TargetDeliveryRequest.builder()
        .context(context)
        .notifications(notifications).build();

targetJavaClient.sendNotifications(notificationRequest);
```

>[!ENDTABS]

### ビューをクリックしました

このサンプルでは、最初に `getOffers`. 次に、これらのビューに基づいて通知を含む要求を作成します。

通知 `type` プロパティはに設定されます。 `click`.

次の項目を設定する必要があります。 `view` プロパティを通知オブジェクトに設定し、targetResult 内の指標配列に基づいてトークンの配列を指定します。

>[!BEGINTABS]

>[!TAB Node.js]

```js {line-numbers="true"}
const TargetClient = require("@adobe/target-nodejs-sdk");
const { v4: uuidv4 } = require("uuid");

const client = TargetClient.create({
  client: "acmeclient",
  organizationId: "1234567890@AdobeOrg",
  events: { clientReady: onTargetReady },
});

async function onTargetReady() {
  const targetResult = await client.getOffers({
    request: {
      targetRequest,
      prefetch: {
        views: [{}]
      },
      sessionId: uuidv4()
    }
  });

  const { views = [] } = targetResult.response.prefetch;

  const request = {
    context: { channel: "web" },
    notifications: views.map(view => {
      const { options = [], metrics = [] } = view;

      return {
        id: targetResult.response.id,
        impressionId: uuidv4(),
        address: {
          url: "http://www.target-demo-site.com"
        },
        timestamp: new Date().getTime(),
        type: "click",
        view: {
          name: view.name
        },
        tokens: metrics
                  .filter(metric => metric.type === "click")
                  .map(metric => metric.eventToken)
      };
    })
  };
  // send the notification event
  await client.sendNotifications({ request });
}
```

>[!TAB Java]

```java {line-numbers="true"}
ClientConfig clientConfig = ClientConfig.builder()
                .client("acmeclient")
                .organizationId("1234567890@AdobeOrg")
                .build();

TargetClient targetClient = TargetClient.create(clientConfig);

Context context = new Context()
        .channel(ChannelType.WEB)
        .address(new Address().url("http://www.target-demo-site.com"));

TargetDeliveryResponse targetResult = targetJavaClient.getOffers(TargetDeliveryRequest.builder()
        .context(context)
        .prefetch(new PrefetchRequest()
                .views(new ArrayList() {{
                    add(new ViewRequest());
                }})
        )
        .build());

List<Notification> notifications = new ArrayList<>();
List<View> views = targetResult.getResponse().getPrefetch().getViews();

for (View view : views) {
    List<Option> options = view.getOptions();
    List<Metric> metrics = view.getMetrics();

    notifications.add((Notification) new Notification()
            .id(targetResult.getResponse().getRequestId())
            .impressionId(UUID.randomUUID().toString())
            .timestamp(System.currentTimeMillis())
            .type(MetricType.CLICK)
            .view(new NotificationView().name(view.getName()))
            .tokens(metrics.stream()
                    .filter(metric -> MetricType.CLICK.equals(metric.getType()))
                    .map(Metric::getEventToken)
                    .collect(Collectors.toList()))
            .address(new Address().url("http://www.target-demo-site.com"))
    );
}

TargetDeliveryRequest notificationRequest = TargetDeliveryRequest.builder()
        .context(context)
        .notifications(notifications).build();

targetJavaClient.sendNotifications(notificationRequest);
```

>[!ENDTABS]
