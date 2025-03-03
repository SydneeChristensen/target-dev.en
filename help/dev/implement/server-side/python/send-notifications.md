---
title: Send display or click notifications to [!DNL Adobe Target] using the Python SDK
description: Learn how to use sendNotifications() to send display or click notifications to [!DNL Adobe Target] for measurement and reporting.
feature: APIs/SDKs
role: Developer
exl-id: 03827b18-a546-4ec8-8762-391fcb3ac435
---
# Send Notifications (Python)

## Description

`send_notifications()` is used to send display or click notifications to [!DNL Adobe Target] for measurement and reporting.

>[!NOTE]
>
>When an `execute` object with required params is within the request itself, the impression will be incremented automatically for qualifying activities.

SDK methods that will increment an impression automatically are:

* `get_offers()`
* `get_attributes()`

When a `prefetch` object is passed within the request, the impression is not automatically incremented for the activities with mboxes within the `prefetch` object. `Send_notifications()` must be used for prefetched experiences for incrementing impressions and conversions.

## Method

### send_notifications

```python {line-numbers="true"}
target_client.send_notifications(options)
```

## Parameters

`options` has the following structure:

|Name|Type|Required|Default|Description|
| --- | --- | --- | --- | --- |
|request|DeliveryRequest|Yes|None|Conforms to the [[!UICONTROL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) request|
|target_cookie|str|no|None|[!DNL Target] cookie|
|target_location_hint|str|no|None|[!DNL Target] location hint|
|consumer_id|str|no|None|When stitching multiple calls, different consumer IDs should be provided|
|customer_ids|list[CustomerId]|no|None|A list of Customer Ids in VisitorId-compatible format|
|session_id|str|no|None|Used for linking multiple requests|
|callback|callable|no|None|If handling request asynchronously, the callback is invoked when response is ready|
|err_callback|callable|no|None|If handling request asynchronously, error callback is invoked when exception is raised|

## Returns

`Returns` a `TargetDeliveryResponse` if called synchronously (default), or an `AsyncResult` if called with a callback. `TargetDeliveryResponse` has the following structure:

|Name|Type|Description|
| --- | --- | --- |
|response|DeliveryResponse|Conforms to the [[!DNL Target Delivery API]](/help/dev/implement/delivery-api/overview.md) response|
|target_cookie|dict|[!DNL Target] cookie|
|target_location_hint_cookie|dict|[!DNL Target] location hint cookie|
|analytics_details|list[AnalyticsResponse]|[!DNL Analytics] payload, in case of client-side [!DNL Analytics] usage|
|trace||list[dict]|Aggregated trace data for all request mboxes/views|
|response_tokens|list[dict]|A list of [​Response Tokens](https://experienceleague.adobe.com/docs/target/using/administer/response-tokens.html)|
|meta|dict|Additional decisioning metadata for use with on-device-decisioning|

## Example

First, let's build the [!UICONTROL Target Delivery API] request for prefetching content for the `home` and `product1` mboxes.

### Python

```python {line-numbers="true"}
mboxes = [MboxRequest(name="home"),
          MboxRequest(name="product1")]
prefetch = PrefetchRequest(mboxes=mboxes)
delivery_request = DeliveryRequest(prefetch=prefetch)

# Next, we fetch the offers via Target Python SDK getOffers() API
response = target_client.get_offers({ "request": delivery_request })
```

A successful response will contain a [!UICONTROL Target Delivery API] response object, which contains prefetched content for the requested mboxes. A sample `target_response["response"]` object (formatted as a dict) may appear as follows:

### Python

```python {line-numbers="true"}
{
  "status": 200,
  "requestId": "e8ac2dbf5f7d4a9f9280f6071f24a01e",
  "id": {
    "tntId": "08210e2d751a44779b8313e2d2692b96.21_27"
  },
  "client": "adobetargetmobile",
  "edgeHost": "mboxedge21.tt.omtrdc.net",
  "prefetch": {
    "mboxes": [
      {
        "index": 0,
        "name": "home",
        "options": [
          {
            "type": "html",
            "content": "HOME OFFER",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      },
      {
        "index": 1,
        "name": "product1",
        "options": [
          {
            "type": "html",
            "content": "TEST OFFER 1",
            "eventToken": "t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q==",
            "responseTokens": {
              "profile.memberlevel": "0",
              "geo.city": "dublin",
              "activity.id": "302740",
              "experience.name": "Experience B",
              "geo.country": "ireland"
            }
          }
        ],
        "state": "J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U"
      }
    ]
  }
}
```

Note the mbox `name` and `state` fields, as well as the `eventToken` field, in each of the Target content options. These should be provided in the `send_notifications()` request, as soon as each content option is displayed. Let's suppose the `product1` mbox has been displayed on a non-browser device. The notifications request will appear as follows:

### Python

```python {line-numbers="true"}
notification_mbox = NotificationMbox(name="product1",
                                     state="J+W1Fq18hxliDDJonTPfV0S+mzxapAO3d14M43EsM9f12A6QaqL+E3XKkRFlmq9U")
notification = Notification(
    id="1",
    type=MetricType.DISPLAY,
    timestamp=1621530726000,  # Epoch time in milliseconds
    mbox=notification_mbox,
    tokens=["t0FRvoWosOqHmYL5G18QCZNWHtnQtQrJfmRrQugEa2qCnQ9Y9OaLL2gsdrWQTvE54PwSz67rmXWmSnkXpSSS2Q=="]
)
notification_request = DeliveryRequest(notifications=[notification])
```

Notice we have included both the mbox state and the event token corresponding to the [!DNL Target] offer delivered in the prefetch response. Having built the notifications request, we can send it to [!DNL Target] via the `send_notifications()` API method:

### Python

```python {line-numbers="true"}
response = target_client.send_notifications({ "request": notification_request })
```
