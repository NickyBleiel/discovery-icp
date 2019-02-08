---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-03"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:note: .note}
{:important: .important}
{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:download: .download}

# Getting started with the API
{: #gs-api}

In this short tutorial, we introduce the {{site.data.keyword.dcp_short}} API and go through the process of creating a private data collection and searching it.
{: shortdesc}

If you prefer to work in the {{site.data.keyword.dcp_short}} tooling, see [Getting started with the tooling](/docs/services/discovery-icp/getting-started-tool.html).
{: tip}

## Before you begin
{: #before-you-begin}

You need a service instance to start. An administrator must install the service as described in [Installing and configuring Watson Assistant Discovery Extension](/docs/services/discovery-icp/install.html#install). 

<!-- Remove the text marked `download` after there's no g-s tab in the catalog dashboard -->

If a {{site.data.keyword.dcp_short}} service instance is already installed, you're all set with these prerequisites. Go to [Step 1](/docs/services/discovery-icp/getting-started-tool.html#launch-the-tooling).

## Step 1: Verify the environment
{: #create-an-environment}

A {{site.data.keyword.dcp_short}} instance includes one environment. Think of an environment as the warehouse where you store all your boxes of documents.

This tutorial uses an API key to authenticate. For production uses, make sure that you review the [API key management APIs](https://www.ibm.com/support/knowledgecenter/SSBS6K_3.1.0/apis/cfc_api_files/api_keys.html).
{: tip}

1. Issue a call to the `GET /v1/environments` method to retrieve the status of  environments. Replace `{apikey}` with your information:

    ```bash
    curl -u "apikey":"{apikey}" "https://{cluster_CA_domain}/{deployment_name}/discovery/api/v1/environments?version=2018-08-01"
    ```
    {: pre}
    
    The method returns output that resembles the following:
    
    ```json
    {
      "environments" : [ {
      "environment_id" : "system",
      "name" : "Watson System Environment",
      "description" : "Shared system data sources",
      "read_only" : true
    }, {
      "environment_id" : "402ead7d-c14f-463d-a878-97beba39e088",
      "name" : "gol",
      "description" : "ICP gol Environment",
      "created" : "2018-05-04T17:44:31.747Z",
      "updated" : "2018-05-04T17:44:31.747Z",
      "read_only" : false
     } ]
    }
    ```

1. Note the `environment_id` value in the second object in the previously listed output. You need this value to issue additional commands.

## Step 2: Create a collection
{: #create-a-collection}

Now that the environment is ready, you can create a collection. Think of a collection as a box that stores your documents in your environment.

1.  You need the ID of your default configuration first. To find your default `configuration_id`, use the `GET /v1/environments/{environment_id}/configurations` method. Replace `{apikey}` and `{environment_id}` with your information:

    ```bash
      curl -u "apikey":"{apikey}" https://{cluster_CA_domain}/{deployment_name}/discovery/api/v1/environments/{environment_id}/configurations?version=2018-08-01
    ```
    {: pre}
1.  Use the `POST /v1/environments/{environment_id}/collections` method to create a collection called **my-first-collection**. Replace `{apikey}`, `{environment_id}` and `{configuration_id}` with your information:

    ```bash
    curl -X POST -u "apikey":"{apikey}" -H "Content-Type: application/json" -d '{"name": "my-first-collection", "description": "exploring collections", "configuration_id":"{configuration_id}" , "language": "en_us"}' https://{icp_cluster_host}:{port}/discovery/api/v1/environments/{environment_id}/collections?version=2018-08-01
    ```
    {: pre}

    The API returns information such as your collection ID, collection status, and how much storage your collection is using.
1.  Check the collection status periodically until you see a status of `active`.
    - Issue a call to the `GET /v1/environments/{environment_id}/collections/{collection_id}` method to retrieve the status of your collection.  Again, Replace `{apikey}`, `{environment_id}`, and `{configuration_id}` with your information:

    ```bash
    curl -u "apikey":"{apikey}" https://{cluster_CA_domain}/{deployment_name}/discovery/api/v1/environments/{environment_id}/collections/{collection_id}?version=2018-08-01
    ```
    {: pre}

## Step 3: Download the sample documents
{: #download-sample-documents}

Download these sample documents: <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc1.html" download>test-doc1.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>, <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc2.html" download>test-doc2.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>, <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc3.html" download>test-doc3.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>, and <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc4.html" download>test-doc4.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>.

**Note:** In some browsers, the preceding links open in a new window instead of saving locally. If this occurs, select `Save As` in your browser's `File` menu to save a copy of the file.

## Step 4: Upload the documents
{: #upload-the-documents}

1.  Now, add the example documents to your collection. This example uploads the document **test-doc1.html** to your collection. Replace `{apikey}`, `{environment_id}`, and `{configuration_id}` with your information. Modify the location of the sample document to point to where you saved the `test-doc1.html` file.
    - Use the `POST /v1/environments/{environment_id}/collections/{collection_id}/documents` method:

        ```bash
        curl -X POST -u "apikey":"{apikey}" -F "file=@test-doc1.html" https://{cluster_CA_domain}/{deployment_name}/discovery/api/v1/environments/{environment_id}/collections/{collection_id}/documents?version=2018-08-01
        ```
        {: pre}

    Alternatively, use one of the SDKs listed in the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://console.bluemix.net/apidocs/discovery-icp){: new_window}:
    - Java:

      ```java
      Discovery service = new Discovery("2017-11-07");
      service.setUsernameAndPassword("apikey","{api_key}");
      service.setEndPoint("https://{icp_cluster_host}:{port}/discovery/api");
      HttpConfigOptions options = new HttpConfigOptions.Builder()
        .disableSslVerification(true)
        .build();
      service.configureClient(options);
      String environmentId = "{environment_id}";
      String collectionId = "{collection_id}";
      String documentJson = "{\"field\":\"value\"}";
      InputStream documentStream = new ByteArrayInputStream(documentJson.getBytes());

      CreateDocumentRequest.Builder builder = new CreateDocumentRequest.Builder(environmentId, collectionId);
      builder.inputStream(documentStream, HttpMediaType.APPLICATION_JSON);
      CreateDocumentResponse createResponse = discovery.createDocument(builder.build()).execute();
      ```
      {: codeblock}

    - Python:

      ```python
      import sys
      import os
      import json
      from watson_developer_cloud import DiscoveryV1

      discovery = DiscoveryV1(
        version='2018-08-01',
        api_key='{apikey}'
      )

      with open((os.path.join(os.getcwd(), '{path_element}', '{filename}' as fileinfo:
        add_doc = discovery.add_document('{environment_id}', '{collection_id}', file_info=fileinfo)
      print(json.dumps(add_doc, indent=2))
      ```
      {: codeblock}

    - Node.js:

      ```javascript
      var DiscoveryV1 = require('watson-developer-cloud/discovery/v1');
      var fs = require('fs');

      var discovery = new DiscoveryV1({
        version_date: '2018-08-01',
        iam_apikey: '{apikey}',
      });

      var file = fs.readFileSync('{/path/to/file}');

      discovery.addDocument({ environment_id: '{environment_id}', collection_id: '{collection_id}', file: file },
      function(error, data) {
        console.log(JSON.stringify(data, null, 2));
        }
      );
      ```
      {: codeblock}

1.  Repeat this process for each of the other 3 sample files.

## Step 5: Query your collection
{: #query-your-collection}

Finally, use the `GET /v1/environments/{environment_id}/collections/{collection_id}/query` method to search your collection of documents.

The following example returns all keywords that are match **cloud**. Replace `{apikey}`, `{environment_id}`, and `{configuration_id}` with your information:

```bash
curl -u "apikey":"{apikey}" 'https://{cluster_CA_domain}/{deployment_name}/discovery/api/v1/environments/{environment_id}/collections/{collection_id}/query?version=2018-08-01&query=text:"IBM"'
```
{: pre}

## Next steps
{: #next-steps}

You successfully queried documents in the environment and collection you created. You can now begin customizing your collection by adding more documents and enrichments, and customizing conversion settings.

- Read about the API in the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://console.bluemix.net/apidocs/discovery-icp){: new_window}
- [Configure](/docs/services/discovery-icp/building.html) your service
