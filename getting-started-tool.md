---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-15"

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
{:download: .download}

# Getting started with the tooling
{: #gs-tool}

In this short tutorial, we introduce the {{site.data.keyword.dcp_short}} tooling and go through the process of creating a private data collection and searching it.
{: shortdesc}

If you prefer to work in the API, see [Getting started with the API](/docs/services/discovery-icp/getting-started.html#gs-api).
{: tip}

## Before you begin
{: #before-you-begin-tool}

You need a service instance to start. An administrator must install the service as described in [Installing and configuring Watson Assistant Discovery Extension](/docs/services/discovery-icp/install.html#install). 

<!-- Remove the text marked `download` after there's no g-s tab in the catalog dashboard -->

If a {{site.data.keyword.dcp_short}} service instance is already installed, you're all set with these prerequisites. Go to [Step 1](/docs/services/discovery-icp/getting-started-tool.html#launch-the-tooling).

## Step 1: Launch the tooling
{: #launch-the-tooling}

  1. Log into the {{site.data.keyword.icpfull_notm}} Foundation management console.
  
  1. From the main menu, expand **Workloads**, and then choose **Deployments**.
  
  1. Find the deployment named `{release-name}-tooling`. If you don't know the `{release-name}`, ask your {{site.data.keyword.icpfull_notm}} administrator.
  
  1. Click **Launch.**
  
     A new web browser tab opens and shows the {{site.data.keyword.dcp_short}} tooling page.
     
     1. Log in by using the same credentials you used to log in to the {{site.data.keyword.icpfull_notm}} Foundation dashboard. 

## Step 2: Create a collection
{: #create-a-collection-tool}

Your first step in the {{site.data.keyword.dcp_short}} tooling is to create a data collection.

A collection is a set of your documents. *Why would I want more than one collection?* There are a few reasons:

- You might want multiple collections to separate results for different audiences.
- The data might be so different that it does not make sense for it all to be queried at the same time.

1. Click the **Upload your own data** button, then you can **Name your new collection**. Name your collection and click **Create**.

## Step 3: Upload your documents
{: #upload-your-documents}

Ingest content into your collection.

1. Download these four sample documents:<br />
<a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc1.html" download>test-doc1.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a><br />
<a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc2.html" download>test-doc2.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a><br />
<a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc3.html" download>test-doc3.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a><br />
<a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc4.html" download>test-doc4.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>
1.  Click ![File icon](images/icon_yourData.png)<!-- {width="20" height="20" style="padding-left:5px;padding-right:5px;"} --> and select your collection.
1.  Click the **Upload documents** button and start uploading the four sample documents: <br />
<a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc2.html" download>test-doc2.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a><br />
<a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc3.html" download>test-doc3.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a><br />
<a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc4.html" download>test-doc4.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>
1.  Wait for the documents to upload. The status of your documents display in the **Overview** section.

## Step 5: Build a query
{: #build-a-query}

1.  Click ![Query icon](images/search_icon.svg)<!-- {width="20" height="20" style="padding-left:5px;padding-right:5px;"} --> to open the query page. Select your collection and click **Get started**.
1.  On the **Build queries** screen, click **Search for documents**, then **Use the Discovery Query Language**:
    - To search for results with keywords named "IBM":
        1.  Click **Field** and select `text`. Select `contains` for **Operator** and `IBM` for **Value**. The query `text:"IBM"` is displayed in **Visual Query Builder**.
        1.  Click **Run Query**. The query returns 4 results.
    - To search for results with keywords named "Watson":
        1.  Click **Field** and select `text`. Select `contains` for  **Operator** and `watson` for **Value**. The query `text:watson` is displayed in **Visual Query Builder**.
        1.  Click **Run Query**. The query returns 3 results.
    - To search for results with keywords named "Watson" and "Slack":
        1.  Click **Field** and select `text`. Select `contains` for **Operator** and `watson` for **Value**. Click **Add rule**, then repeat your selections, but choose the **Value** of `Slack`. The query `text:watson,text:Slack` is displayed in **Visual Query Builder**.
        1.  Click **Run Query**. The query returns 1 result.

    Click **Edit in query language** to build queries using the Discovery Query Language. To learn more about the Discovery Query Language, see [Query reference](/docs/services/discovery-icp/query-reference.html#query-ref) and [Query concepts](/docs/services/discovery-icp/using.html#query-concepts).
1.  The results of your query are displayed in the **Results** section:
    - The **Summary** tab provides an overview of the query results,
    - The **JSON** tab displays the full JSON results.

    For the queries listed, the **Summary**  displays the document passages (in order of relevance) first, followed by the names of the documents found, then the results by enrichment.

    The **Query URL** link provided under both the **JSON** and **Summary** tabs is ready-to-use in your application.

    Additional resources:
    - To learn more about the data schema of your documents, go to the **Query Builder** and click the **View Data Schema** icon or on the **JSON** tab. See [The Discovery data schema](/docs/services/discovery-icp/using.html#discovery-schema) for details.
    - If editing in the Discovery Query Language, click on the **?** icons next to any of the **Enter query here** fields for more examples.

## Next steps
{: #next-steps-gs-tool}

Now you have a functioning and populated {{site.data.keyword.dcp_short}} service instance. You can now begin customizing your collection by adding more documents and customizing conversion settings.
