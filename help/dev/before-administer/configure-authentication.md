---
title: How to Configure Authentication for [!DNL Adobe Target] APIs
description: How do I generate authentication tokens needed to successfully interact with [!DNL Adobe Target] APIs?
role: Developer, Admin, Architect
level: Intermediate
topic: Personalization, Administration, Integrations, Development
feature: APIs/SDKs, Administration & Configuration
exl-id: fc67363c-6527-40aa-aff1-350b5af884ab
---
# Configure authentication for [!DNL Adobe Target] APIs

The [!DNL Adobe Target] Admin APIs, including [!DNL Recommendations Admin] APIs, are secured by authentication to ensure only authorized users use them to access [!DNL Adobe Target]. Use the [Adobe Developer Console](https://developer.adobe.com/console/home) to manage this authentication for all [!DNL Adobe Experience Cloud solutions], including [!DNL Adobe Target].

Here are the preliminary steps required to generate authentication tokens needed to successfully interact with [!DNL Adobe Target] APIs:

1. Create a project (previously called integration) in the [!DNL Adobe Developer Console].
1. Export project details to Postman.
1. Generate a bearer access token.
1. Test the bearer access token.

## Pre-requisites

|Resource|Details|
| --- | --- |
|Postman|In order to complete these steps successfully, get the [Postman app](https://www.postman.com/downloads/) for your operating system. Postman basic is free with account creation. While not required in order to use [!DNL Adobe Target] APIs in general, Postman makes API workflows easier, and [!DNL Adobe Target] provides several Postman collections to help execute its APIs and learn how they operate. The rest of this guide assumes working knowledge of Postman. For assistance, see the [Postman documentation](https://learning.getpostman.com/).  |
|References|Familiarity with the following resources is assumed throughout the rest of this guide:<ul><li>[Adobe I/O Github](https://github.com/adobeio)</li><li>[Target Admin and Profile API documentation](../administer/admin-api/admin-api-overview-new.md)</li><li>[Recommendations API documentation](https://developers.adobetarget.com/api/recommendations/)</li></ul>|

## Create an Adobe I/O project

In this section, you will access the [!DNL Adobe Developer Console] and create a project for [!DNL Adobe Target]. For more information, reference the [documentation on projects](https://developer.adobe.com/developer-console/docs/guides/projects/).

<!---(1. Generate your private key and public certificate, per the [documentation on authentication](https://developer.adobe.com/developer-console/docs/guides/authentication/). // [//]: # (as described in **Step 1** of [How to set up Adobe IO: Authentication - Step by Step](https://helpx.adobe.com/marketing-cloud-core/kb/adobe-io-authentication-step-by-step.html). After completing Step 1, return to this guide and resume with Step 2, below. // The outcome of this step should be the creation of a `private.key` file and a `certificate_pub.crt` file. Return to this guide once you have generated these two files.)--->

1. In the [Adobe Admin Console](https://adminconsole.adobe.com/), ensure your [!DNL Adobe] user account has been granted both [Product Admin](https://helpx.adobe.com/enterprise/using/admin-roles.html) and [Developer](https://helpx.adobe.com/enterprise/using/manage-developers.html) level access to [!DNL Target].

1. In the [Adobe Developer Console](https://developer.adobe.com/console/home), select the [!UICONTROL Experience Cloud Organization] for which you want to create this integration. (Note it is likely you may only have access to a single [!UICONTROL Experience Cloud Organization].) 

   ![configure-io-target-createproject2.png](assets/configure-io-target-createproject2.png)

1. Click **[!UICONTROL Create new project]**.

   ![configure-io-target-createproject3.png](assets/configure-io-target-createproject3.png)

1. Click **[!UICONTROL Add API]** to add a REST API to your project to access [!DNL Adobe] services and products.

   ![Add API](assets/configure-io-target-createproject4.png)

1. Select **[!DNL Adobe Target]** as the [!DNL Adobe] service you wish to integrate with. Click the **[!UICONTROL Next]** button that appears. 

   ![configure-io-target-createproject5](assets/configure-io-target-createproject5.png)

1. Select an option for associating public and private keys with the service account integration you are creating for [!DNL Target]. For this example, select **[!UICONTROL Option 1: Generate a key pair]** and click **[!UICONTROL Generate keypair]**.

   ![configure-io-target-createproject6](assets/configure-io-target-createproject6.png)

1. As instructed, make note of the automatically downloaded configuration file (`config`), which contains your private key. Click **[!UICONTROL Next]**.

   ![configure-io-target-createproject7](assets/configure-io-target-createproject7.png)

1. In your file system, verify the location of `config`, which is the compressed configuration file created in the previous step. Again, this `config` file contains your private key, which you will need later. The exact location within your file system may differ from the one shown here.

   ![configure-io-target-createproject8](assets/configure-io-target-createproject8.png)

1. Back in the Adobe Developer Console, select the [product profile(s)](https://helpx.adobe.com/enterprise/using/manage-products-and-profiles.html) corresponding to the properties in which you are using Adobe Recommendations. (If you are not using properties, select the Default Workspace option.) Click **[!UICONTROL Save configured API]**.

   ![configure-io-target-createproject9](assets/configure-io-target-createproject9.png)

1. Click **[!UICONTROL Create Integration]**. You should receive a temporary message indicating your API was successfully configured.
1. As a final step, rename your project to a name more meaningful than the original `Project 1`. To do this, navigate to the project using the navigation path as show, click **[!UICONTROL Edit project]** to access the **[!UICONTROL Edit Project]** modal, and rename the project.

   ![configure-io-target-createproject11](assets/configure-io-target-createproject11.png)

>[!NOTE]
>
>In this example, we name our project "[!DNL Target] Integration." If you anticipate using your project for more than just [!DNL Adobe Target], you may want to name it accordingly. For example, you might choose to name it "Adobe APIs" or "Experience Cloud APIs," since it may be used with other solutions in the Adobe Experience Cloud.

## Export project details

Now that you have an Adobe project you can use for accessing [!DNL Target], you need to make sure to send details of that project along with your Adobe API requests. These details are required in order to interact with several Adobe APIs, including several [!DNL Target] APIs. For example, the integration details include authorization and authentication information required by the [!DNL Target] Admin APIs. Therefore, to use the APIs with Postman, you need to get those details into Postman.

There are many ways to specify the details of your project in Postman, but in this section, we take advantage of some pre-built features and collections. First (in this section), you will export the details of your integration into a Postman environment. Next (in the following section), you will generate a bearer access token to grant you access to the necessary Adobe resources.

>[!NOTE]
>
>For video instructions applicable for any Experience Cloud solution, including [!DNL Target], see [Use Postman with Experience Platform APIs](https://experienceleague.adobe.com/docs/platform-learn/tutorials/platform-api-authentication.html). The following sections are relevant to the [!DNL Target] APIs: 1. Create and export Experience Platform API to Postman 2. Generate an Access Token with Postman. These steps are also provided below.

1. Still in the [Adobe Developer Console](https://developer.adobe.com/console/home), navigate to view your new project's **[!UICONTROL Service Account (JWT)]** credentials. Use either the left navigation or the **[!UICONTROL Credentials]** section as shown.

   ![JWT1](assets/configure-io-target-jwt1.png)

   In **[!UICONTROL Credential details]**, note you may view your **[!UICONTROL Public key(s)]**, **[!UICONTROL Client ID]**, and other information related to your service account.

   ![JWT1a](assets/configure-io-target-jwt1a.png)

1. Click to navigate to information about the **[!DNL Adobe Target]** API. Use either the left navigation or the **Connected products and services** section as shown.

   ![JWT2](assets/configure-io-target-jwt2.png)

1. Click **[!UICONTROL Download for Postman]** > **[!UICONTROL Service Account (JWT)]** to create a JSON file capturing your authentication information for a Postman environment.

   ![JWT3](assets/configure-io-target-jwt3.png)

   Note the JSON file in your file system.

   ![JWT3a](assets/configure-io-target-jwt3a.png)

1. In Postman, click the gear icon to manage your environments, then click **[!UICONTROL Import]** to import the JSON file (environment).

   ![JWT4](assets/configure-io-target-jwt4.png)

1. Choose your file and click **[!UICONTROL Open]**.

   ![JWT5](assets/configure-io-target-jwt5.png)

1. In the Postman **Manage Environments** modal, click the name of the newly imported environment to inspect it. (Your environment name may be different from the one shown here. Edit the name as desired. It does not necessarily need to match the name of the [!DNL Adobe] project.)

   ![JWT6](assets/configure-io-target-jwt6.png)

1. Note `CLIENT_SECRET` and `API_KEY` (along with other variables) have their values pre-populated, taken from your integration as defined in the Adobe Developer Console. (The Postman `CLIENT_SECRET` variable should match the `CLIENT SECRET` Adobe credential as displayed in the Developer Console, and `API_KEY` in Postman should likewise match `CLIENT ID` in the Developer Console.) By contrast, note `PRIVATE_KEY`, `JWT_TOKEN`, and `ACCESS_TOKEN` are blank. Let's start by providing the `PRIVATE_KEY` value.

   ![JWT7](assets/configure-io-target-jwt7.png)

1. From your file system, open your `config` file, and open the `private` key file.

   ![JWT8](assets/configure-io-target-jwt8.png)

1. Select and copy the entire contents of the `private` key file.

   ![JWT9](assets/configure-io-target-jwt9.png)

1. In Postman, paste your private key value into the **[!UICONTROL INITIAL VALUE]** and **[!UICONTROL CURRENT VALUE]** fields.

   ![JWT10](assets/configure-io-target-jwt10.png)

1. Click **[!UICONTROL Update]**, and close the Environments modal.

## Generate the bearer access token

In this section, you generate your bearer access token, which is required for authenticating your interaction with [!DNL Adobe Target] APIs. To generate your bearer access token, you need to send your integration details (established in the preceding sections) to the [Adobe Identity Management Service (IMS)](https://www.adobe.io/authentication/auth-methods.html#!AdobeDocs/adobeio-auth/master/AuthenticationOverview/AuthenticationGuide.md). There are a few different ways to do this, but in this guide we take advantage of a Postman collection containing a pre-built IMS call that makes the process direct and easy. Once you import the collection, you may reuse it whenever needed, to generate new tokens not only for [!DNL Adobe Target], but other Adobe APIs as well.

1. Navigate to the [Adobe Identity Management Service API sample calls](https://github.com/adobe/experience-platform-postman-samples/tree/master/apis/ims).

   ![token1](assets/configure-io-target-generatetoken1.png)

1. Click the **[!UICONTROL Adobe I/O Access Token Generation Postman collection]**.

   ![token2](assets/configure-io-target-generatetoken2.png)

1. Get the raw JSON for this collection by clicking **[!UICONTROL Raw]**, then copying the resulting JSON to your clipboard. (Alternatively, you can save the raw JSON as a .json file.)

   ![token3](assets/configure-io-target-generatetoken3.png)
   
1. In Postman, import the collection by pasting and submitting the raw JSON from your clipboard. (Alternatively, you can upload the .json file you saved.) Click **[!UICONTROL Continue]**.

   ![token4](assets/configure-io-target-generatetoken4.png)

1. Select the **[!UICONTROL IMS: JWT Generate + Auth via User Token]** request in the Adobe I/O Access Token Generation Postman collection, ensure your environment is selected, and click **[!UICONTROL Send]** to generate the token.

   ![token5](assets/configure-io-target-generatetoken5.png)

   >[!NOTE]
   >
   >This bearer access token will be valid for 24 hours. Send the request again whenever you need to generate a new token.
   
1. Open the Manage Environments modal again, and select your environment.

   ![token6](assets/configure-io-target-jwt11.png)

1.  Note the `ACCESS_TOKEN` and `JWT_TOKEN` values are now populated.

    ![token7](assets/configure-io-target-generatetoken7.png)

Question: Do I have to use the Adobe I/O Access Token Generation Postman collection to generate the JSON Web Token (JWT) and bearer access token?

Answer: No. The Adobe I/O Access Token Generation Postman collection is available as a convenience to more easily generate the JWT and bearer access token in Postman. Alternatively, you can use capabilities within the Adobe Developer Console to manually generate the bearer access token.

## Test the bearer access token

In this exercise, you will use your new bearer access token by sending an API request that retrieves a list of activities from your [!DNL Target] account. A successful response indicates your [!DNL Adobe] project and authentication are operating as expected in order to use the API.

1. Import the [[!DNL Adobe Target] Admin APIs Postman Collection](https://developers.adobetarget.com/api/#admin-postman-collection). Follow all prompts until the collection is imported in Postman.

   ![testtoken1](assets/configure-io-target-testtoken0.png)

1. Expand the collection, and note the **[!UICONTROL List activities]** request.

   ![testtoken1](assets/configure-io-target-testtoken1.png)

1. Note that variables such as `{{access_token}}` are initially unresolved. You could resolve this in several different ways—for example, you could define a new collection variable called `{{access_token}}`—but in this guide, you will instead change the API request to leverage the Postman environment you were previously using. This will enable the environment to continue to serve as a single, consistent consolidation of all variables common across Adobe APIs.

   ![testtoken2](assets/configure-io-target-testtoken2.png)

1. Type to replace `{{access_token}}` with `{{ACCESS_TOKEN}}`.

   ![testtoken3](assets/configure-io-target-testtoken3.png)

1. Type to replace `{{api_key}}` with `{{API_KEY}}`.

   ![testtoken4](assets/configure-io-target-testtoken4.png)

1. Type to replace `{{tenant}}` with `{{TENANT_ID}}`. Note `{{TENANT_ID}}` is not yet recognized.

   ![testtoken4](assets/configure-io-target-testtoken4a.png)

1. Open the Manage Environments modal, and select your environment.

   ![JWT11](assets/configure-io-target-jwt11.png)

1. Type to add a new `{{TENANT_ID}}` environment variable. Copy and paste your Tenant ID value into the **[!UICONTROL INITIAL VALUE]** and **[!UICONTROL CURRENT VALUE]** fields for your new `TENANT_ID` environment variable.

   ![testtoken5](assets/configure-io-target-testtoken5.png)

   >[!NOTE]
   >
   >The Tenant ID is different from your [!DNL Target] `clientcode`. The Tenant ID exists in the URL when you are logged in to [!DNL Target]. To obtain your Tenant ID, log in to the Adobe Experience Cloud, open [!DNL Target], and click the Target card. Use the Tenant ID value as noted in the URL subdomain. For example, if your URL when logged in to [!DNL Adobe Target] is `<https://mycompany.experiencecloud.adobe.com/...>` then your Tenant ID is "mycompany."

1. Send your request, after ensuring you have selected the correct environment. You should receive a response containing your list of activities.

   ![testtoken6](assets/configure-io-target-testtoken6.png)

Now that you have verified your Adobe authentication, you can use it to interact with [!DNL Adobe Target] APIs (as well as other Adobe APIs). For example, you can [Use Recommendations APIs](recs-api/overview.md) to create or manage recommendations, or you can use it with the [Target Delivery API](/help/dev/implement/delivery-api/overview.md).
