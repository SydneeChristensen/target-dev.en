---
keywords: implement, implementing, setting up, setup, customer attributes
description: Get data into [!DNL Target] using customer attributes.
title: How Do I Get Data into [!DNL Target] Using Customer Attributes?
feature: Implementation
role: Developer
exl-id: d05cdd38-ba7c-4f29-a0ef-ae68619e7617
---
# Customer attributes

Customer attributes let you upload visitor profile data via FTP to the [!DNL Adobe Experience Cloud]. Once uploaded, use the data in [!DNL Adobe Analytics] and [!DNL Adobe Target].

Target Standard customers can apply five attributes, [!DNL Target Premium] customers can apply 200 attributes.

## Format

A .csv file with [!DNL Experience Cloud] IDs (ECIDs) and attribute name/value pairs is uploaded via FTP or manually in the Experience Cloud UI.

## Example use cases

Your CRM or other internal system stores valuable information you want to share with [!DNL Adobe Experience Cloud], including [!DNL Target] and [!DNL Analytics].

## Benefits of method

Uploading customer data creates a profile entry for that visitor in Target, even if [!DNL Target] has yet to see the visitor.

Same data is automatically available in [!DNL Target] and [!DNL Analytics].

FTP can be a simpler implementation method than API.

## Caveats

Target Standard customers can apply five attributes, [!DNL Target Premium] customers can apply 200 attributes

Can only update values via Customer Attributes, not on page.

Requires Experience Cloud ID (ECID) implementation.

## Code examples

Details can be found in [Create a customer attribute source and upload the data file](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html).

### Links to relevant information

[Create a customer attribute source and upload the data file](https://experienceleague.adobe.com/docs/core-services/interface/customer-attributes/t-crs-usecase.html).
