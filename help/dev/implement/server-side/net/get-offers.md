---
title: Use getOffers() in [!DNL Adobe Target] when using the .NET SDK
description: Learn how to use getOffers() to execute a decision and retrieve an experience from [!DNL Adobe Target].
feature: APIs/SDKs
role: Developer
exl-id: 4d1d1cbd-c7e5-4146-9fea-08e01923874d
---
# Get Offers (.NET)

## Description

`GetOffers()` is used to execute a decision and retrieve an experience from [!DNL Adobe Target].

## Method

`TargetClient.GetOffers` method signature.

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryResponse TargetClient.GetOffers(TargetDeliveryRequest request)
```

`TargetDeliveryRequest` is created using `TargetDeliveryRequest.Builder`.

## \.NET

```dotnet {line-numbers="true"}
TargetDeliveryRequest.Builder TargetDeliveryRequest.Builder()
```

## Parameters

The `TargetDeliveryRequest.Builder` object has the following structure:

|Name|Type|Required|Description|
| --- | --- | --- | --- |
|context|Context|Yes|Specifies the context for the request|
|sessionId|String|No|Used for linking multiple [!DNL Target] requests|
|thirdPartyId|String|No|Your company's identifier for the user that you can send with every call|
|cookies|List|No|List of cookies returned in previous [!DNL Target] request of same user.|
|customerIds|Map|No|Customer Ids in VisitorId-compatible format|
|execute|ExecuteRequest|No|PageLoad or mboxes request to execute. Will be evaluated on server side immediately|
|prefetch|PrefetchRequest|No|Views, PageLoad or mboxes request to prefetch. Returns with notification token to be returned on conversion.|
|notifications|List|No|Used to sent notifications regarding what prefetched content was displayed|
|requestId|String|No|The request ID that will be returned in the response. Generated automatically if not present.|
|impressionId|String|No|If present,  second and subsequent requests with the same id will not increment impressions to activities/metrics. Generated automatically if not present.|
|environmentId|Long|No|Valid client environment id. If not specified host will be determined base on the provided host.|
|property|Property|No|Specifies the at_property via the token field. It can be used to control the scope for the delivery.|
|trace|Trace|No|Enables trace for Delivery API.|
|qaMode|QAMode|No|Use this object to enable the QA mode in the request.|
|locationHint|String|No|[!DNL Target] edge cluster location hint. Used to target given edge cluster for this request.|
|visitor|Visitor|No|Used to provide custom Visitor API object.|
|id|VisitorId|No|Object that contains the identifiers for the visitor. Eg. tntId, thirdParyId, mcId, customerIds.|
|experienceCloud|ExperienceCloud|No|Specifies integrations with Audience Manager and Analytics. Automatically populated using cookies, if not provided.|
|tntId|String|No|Primary identifier in [!DNL Target] for a user. Fetched from targetCookies. Auto-generated if not provided.|
|mcId|String|No|Used to merge and share data between different Adobe solutions(ECID). Fetched from targetCookies. Auto-generated if not provided.|
|trackingServer|String|No|The Adobe Analytics Server in order for [!DNL Adobe Target] and [!DNL Adobe Analytics] to correctly stitch the data together.|
|trackingServerSecure|String|No|The [!UICONTROL Adobe Analytics Secure Server] in order for [!DNL Adobe Target] and [!DNL Adobe Analytics] to correctly stitch the data together.|
|decisioningMethod|DecisioningMethod|No|Can be used to explicitly set ON_DEVICE or HYBRID decisioning method for on-device decisioning|

The values of each field should conform to [Target Delivery API](/help/dev/implement/delivery-api/overview.md) request specification.

## Response

The `TargetDeliveryResponse` returned by `TargetClient.GetOffers()` has the following structure:

|Name|Type|Description|
| --- | --- | --- |
|Request|TargetDeliveryRequest​|[Target Delivery API](/help/dev/implement/delivery-api/overview.md) request|
|Response|DeliveryResponse​|[Target Delivery API](/help/dev/implement/delivery-api/overview.md)* response|
|Status|HttpStatusCode|Response HTTP status code|
|Message|string|Response status message or error message|
|Locations|Locations|[!DNL Target] location names, including global mbox name and mboxes/views for which only remote decisioning is available|
|GetCookies|Dictionary|Returns a dictionary of session metadata for this user. This needs to be passed in next [!DNL Target] request for this user.|
|VisitorState|IDictionary|Visitor state to be set on client side for Visitor API Javascript library initialization|

The `TargetCookie` object used for saving data for user session has the following structure:

|Name|Type|Description|
| --- | --- | --- |
|Name|string|Cookie name|
|Value|string|Cookie value|
|MaxAge|int|The `MaxAge` option is a convenience for setting Expires relative to the current time in seconds|

You don't have to worry about expiring the cookies. [!DNL Target] handles `MaxAge` inside the SDK.

## Example

## \.NET

```dotnet {line-numbers="true"}
var targetClientConfig = new TargetClientConfig.Builder("acmeClient", "ABCDEF012345677890ABCDEF0@AdobeOrg")
    .Build();

var targetClient = TargetClient.Create(targetClientConfig);

var mboxRequests = new List<MboxRequest> { new (index: 1, name: "a1-serverside-ab") };

var targetDeliveryRequest = new TargetDeliveryRequest.Builder()
    .SetExecute(new ExecuteRequest(mboxes: mboxRequests))
    .Build();

var targetResponse = targetClient.GetOffers(targetDeliveryRequest);
```
