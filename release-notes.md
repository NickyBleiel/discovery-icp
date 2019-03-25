---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

subcollection="discovery-icp"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:note: .note}
{:important: .important}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Release notes
{: #relnotes}

The release notes provide information about changes to the {{site.data.keyword.dcp_long}} service since the previous release.
{: shortdesc}

## Service API Versioning
{: #api-version}

API requests require a version parameter that takes a date in the format `version=YYYY-MM-DD`. Whenever we change the API in a backwards-incompatible way, we release a new minor version of the API.

Send the version parameter with every API request. The service uses the API version for the date you specify, or the most recent version before that date. Do not default to the current date. Instead, specify a date that matches a version that is compatible with your application, and do not change it until your application is ready for a later version.

The current version is `2018-08-01`.

## Beta features
{: #beta-features}

IBM will release services, features, and language support that are classified as beta or experimental. These capacities can be unstable, can change frequently, and can be discontinued with short notice. They are provided so you can evaluate their functionality. A beta or experimental capacity might not provide the same level of performance or compatibility that generally released capacities provide. These capacities are not designed for use in a production environment, and any such use is at your own risk.
{: important}

## Changes
{: #change-log}

The following new features and changes to the service are available.

## 1.0.1, 8 February 2019
{: #08-feb-2019}

Version 1.0.1 includes the following changes and updates:

 - An implementation to back up and restore your data. For more information, see [Backing up and restoring data](/docs/services/discovery-icp?topic=discovery-icp-backup-restore).
 
 - {{site.data.keyword.dcp_short}} is now available in the following 11 languages:
   - Arabic (`ar`)
   - Brazilian Portuguese (`pt-br`)
   - Chinese (`zh`)
   - Dutch (`nl`)
   - English (`en`)
   - French (`fr`)
   - German (`de`)
   - Italian (`it`)
   - Japanese (`ja`)
   - Korean (`ko`)
   - Spanish (`es`)
   
  Although the service supports these languages for collections and queries, Natural Language Understanding (NLU) enhancements are available only in English (`en`) at the current time.
  {: important}
  
  - Support for IBM Cloud Private for Data.

## 1.0.0 (General Availability release), 20 November 2018
{: #20-nov-2018}

The {{site.data.keyword.dcp_long}} service brings the cognitive capabilities of the Watson Discovery Service to the {{site.data.keyword.icpfull_notm}} platform.

{{site.data.keyword.dcp_short}} includes the following endpoints:
 - A health check endpoint accessible on `/api/health/check`.
 - Environment endpoints accesible on `/api/v1/environments`.
 - Configuration endpoints accessible on `/api/v1/environments/{environment_id}/configurations`.
 - Collection endpoints accessible on `/api/v1/environments/{environment_id}/collections`.
 - Document endpoints accessible on `/api/v1/environments/{environment_id}/collections/{collection_id}/documents`.
 - A query endpoint accesible on `/api/v1/environments/{environment_id}/collections/{collection_id}/query`.
  
## Known issues in the GA release
{: #known-issues-ga}

The following known issues apply to the GA release:

  - {{site.data.keyword.icpfull_notm}} 3.1.1 and later are not currently supported.
  - The [API Reference](https://{DomainName}/apidocs/discovery-icp) lists methods and parameters that are not currently available on {{site.data.keyword.dcp_short}}. The preceding section lists the supported method endpoints.
    Additionally, the [API Reference](https://{DomainName}/apidocs/discovery-icp) lists `curl` API paths 
  - {{site.data.keyword.dcp_short}} is currently available only in English (`en`). Some parts of the {{site.data.keyword.dcp_short}} interface, including the tooling, might appear to support non-English languages. If you find a reference to non-English support, disregard it.
  - Disregard references to natural language query in the {{site.data.keyword.dcp_short}} tooling. If you submit a natural language query, the service processes it as a Discovery Query Language request. The results returned by a natural language query are indeterminable.
  - Disregard references to Watson Discovery News in the {{site.data.keyword.dcp_short}} tooling.
  - Disregard references to passage retrieval in the {{site.data.keyword.dcp_short}} tooling. Also disregard any occurrences of  `"passages": []` in service output.
  - The **Learn more** links in the {{site.data.keyword.dcp_short}} tooling link to the public {{site.data.keyword.discoveryshort}} documentation, not to the {{site.data.keyword.dcp_short}} documentation.

## Using an IAM access token to authenticate
{: #iam}

The the {{site.data.keyword.dcp_short}} service uses IAM access tokens. When you use IAM access tokens, you authenticate before you send a request to the  service. Perform the following steps to use an IAM access token.

1.  Get an API key from IBM Cloud Private. Use that key to generate an IAM access token. For more information, see [Creating a service ID by using IBM Cloud Private CLI ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/manage_cluster/cli_iam_create_serviceid.html){: new_window}.
1.  Pass the IAM access token to the {{site.data.keyword.dcp_short}} service by using the `Authorization` header. In the header, indicate that the access token is a `Bearer` token by specifying `Authorization: Bearer {access_token}`.

    The following simple cURL example uses an access token:

    ```bash
    curl -X GET
    --header "Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs"
    "https://{cluster_CA_domain}/{deployment_name}/discovery/api/v1/environments?version=2018-08-01"
    ```
    {: pre}

    For more information, see [Preparing to run component or management API commands ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/apis/access_api.html){: new_window} and [User management and authentication API ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/apis/auth_manage_api.html){: new_window}.

### Refreshing an IAM access token
{: #iam-refresh}

IAM access tokens that you generate have the following structure. You use the value of the `access_token` field to make an authenticated request to the service.

```javascript
{
  "access_token": "eyJhbGciOiJIUz......sgrKIi8hdFs",
  "refresh_token": "SPrXw5tBE3......KBQ+luWQVY=",
  "token_type": "Bearer",
  "expires_in": 3600,
  "expiration": 1473188353
}
```
{: codeblock}

Access tokens have a limited time to live. The `expires_in` field indicates how long the token lasts, in this case one hour. The `expiration` field shows when the token expires as a UNIX timestamp that specifies the number of seconds since January 1, 1970 (midnight UTC/GMT).

In your application, check the access token's expiration time before you use it to make an authenticated request. If it is expired, you must refresh the access token before you can use it. You use the value of the `refresh_token` field to refresh the access token. For more information, see [Configuring access and identity token validity ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/user_management/token_value.html){: new_window}.

## Known limitations
{: #limitations}

### Ingestion limitations

-   {{site.data.keyword.nlufull}} enrichments are limited to the first 50 kB per field.

