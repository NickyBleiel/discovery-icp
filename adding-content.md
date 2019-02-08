---

copyright:
  years: 2015, 2019
lastupdated: "2018-01-11"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Adding content

You can upload documents by using either the [API](/docs/services/discovery-icp/getting-started.html) or the [{{site.data.keyword.dcp_short}} tooling](/docs/services/discovery-icp/getting-started-tool.html).
{: shortdesc}

-   Use the [API](/docs/services/discovery-icp/getting-started.html) if you are integrating the upload of content with an existing application or creating your own custom upload mechanism.
-   Use the [{{site.data.keyword.dcp_short}} tooling](/docs/services/discovery-icp/getting-started-tool.html) if you want to quickly upload locally accessible files.
    When uploading documents using the {{site.data.keyword.dcp_short}} tooling, each document needs to have a unique file name. If two files have the same name, the document that was uploaded first is overwritten when the second document is uploaded. If you would prefer that documents with the same file name coexist in your collection, the Document ID needs to be specified. You can specify the Document ID if you upload documents by using the API.

## Adding content with the API or tooling
{: #adding_content}

Consider the following when you are ready to add documents to your collection:

-   The maximum file size that can be uploaded to the {{site.data.keyword.dcp_short}} service is 50MB.
-   The sample documents are not automatically added to the collection. You must add them if you want them as part of your collection.
-   Only the first 50,000 characters of each JSON field selected for enrichment will be enriched.
-   When creating a collection, you select the document language (English is the default). See [Language support](/docs/services/discovery-icp/language-support.html) for the list of languages. Your documents will be enriched in the selected language. Do not mix languages within the same collection.
-   You can add Microsoft Word, HTML and XHTML, and JSON documents to your collection. Other document types, including PDF and XML, are not supported.
-   The documents in your collection are converted using the configuration file provided, which is named **Default Configuration**, unless you choose a different configuration file. For information about creating a configuration file, see [Custom configuration](/docs/services/discovery-icp/building.html#custom-configuration).
-   When documents are uploaded to a data collection, they are converted and enriched using the configuration file chosen for that collection. If you decide later that you would like to switch a collection to a different configuration file, you can do that, but the documents that have already been uploaded will remain converted by the original configuration file. All documents uploaded after switching the configuration file will use the new configuration file. If you want the **entire** collection to use the new configuration, you will need to create a new collection, choose that new configuration file, and re-upload all the documents.
-   You cannot specify the `data type` (For example: `text` or `date`) of fields. During document ingestion, if a field is detected that does not yet exist in the index, {{site.data.keyword.dcp_short}} will automatically detect the `data type` of that field based on the value of the field for the first document indexed.
-   A document can fail to be ingested because of a type mismatch between data in the current document and similar data in a previously ingested document. For example, a field might be typed as a `date` in one document and a `string` in a subsequent document, preventing the subsequent document from being indexed correctly.

## Uploading documents with the Discovery tooling
{: #upload_tool}

1.  Create a collection. See [Preparing the service for your documents](/docs/services/discovery-icp/building.html#preparing-the-service-for-your-documents).
1.  Click on the collection to open it.
1.  Click the **Upload documents** button and start uploading your documents via drag and drop or browse.

Your documents are now enqueued to be converted and enriched. The time this task depend on the size of your collection. After the documents are indexed and enriched, the details of the collection are displayed in the **Overview** section.

-   **Created** and **Last updated** dates (Click **Use this collection in API** to see the `collection_id`, `configuration_id`, and `environment_id` values.)
-   **Number of documents** in your collection
-   **Configuration** — The name of the configuration file used to convert this collection
-   **Errors and Warnings**

## Uploading documents with the API
{: #upload_api}

See [Getting started with the {{site.data.keyword.dcp_short}} API](/docs/services/discovery-icp/getting-started.html) for a step-by-step tutorial.

For more information about the API, see the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://console.bluemix.net/apidocs/discovery-icp){: new_window}.

1.  Use the `POST /v1/environments/{environment_id}/collections` method to create a collection.
1.  Then use the `POST /v1/environments/{environment_id}/collections/{collection_id}/documents` method to add documents to your collection.

