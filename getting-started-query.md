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
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Getting started with querying
{: #query-gs}

In this tutorial, we will learn how to write a few different types of queries in {{site.data.keyword.dcp_short}}.
{: shortdesc}

For more information about writing queries, see:
- [Query concepts](/docs/services/discovery-icp?topic=discovery-icp-query-concepts)
- [Query reference](/docs/services/discovery-icp?topic=discovery-icp-query-ref) (includes the list of parameters, operators, and aggregations available in the Discovery Query Language)

These example queries are built by using the {{site.data.keyword.dcp_short}} tooling. If you'd like to use the API instead, add the query parameters to your API call. For more information and examples, see the Queries section of the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https:/{DomainName}/apidocs/discovery-icp){: new_window}.

## Before you begin

**Complete the steps in [Getting started](/docs/services/discovery-icp?topic=discovery-icp-gs-tool).** If you haven't completed the **Getting started**, go to the **Manage data** screen, create a new collection named {{site.data.keyword.IBM_notm}} Press Releases, and add these four documents to it:<br />
<a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc1.html" download>test-doc1.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a><br />
<a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc2.html" download>test-doc2.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a><br />
<a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc3.html" download>test-doc3.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a><br />
<a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/discovery/test-doc4.html" download>test-doc4.html <img src="../../icons/launch-glyph.svg" alt="External link icon" title="External link icon" class="style-scope doc-content"></a>

## Step 1: Quick tour of the Discovery data schema

Let's start out by getting to know the {{site.data.keyword.dcp_short}} JSON. To understand how to build a query using the Discovery Query Language, it helps to be familiar with the JSON produced by {{site.data.keyword.dcp_short}} after it enriches the documents in your collection.

1.  [Launch the {{site.data.keyword.dcp_short}} tooling](/docs/services/discovery-icp?topic=discovery-icp-gs-tool#launch-the-tooling). On the **Manage data** screen, choose the {{site.data.keyword.IBM_notm}} Press Releases collection.

1.  To get familiar with the data schema of your documents, let's look at the **View data schema** screen. It displays the fields and values in your transformed documents two ways: by document (**Document view**), or by field (**Collection view**). **Collection view** displays all fields in your collection.

If your query does not return any matching results, and you think it should, try swapping out the field/value your query is using for one that you can verify in the data schema.
{: tip}    

## Step 2: Build a basic query

Let's start out by writing a query that will find the text `cloud` in your collection:

1.  Click on the build queries icon ![Query icon](images/search_icon.svg)<!-- {width="20" height="20" style="padding-left:5px;padding-right:5px;"} --> to open the query page. Select the collection that contains the {{site.data.keyword.IBM_notm}} Press Releases and click **Get started**.
1.  On the **Build queries** screen, click **Search for Documents**, then **Use the Discovery Query Language** then:
    - Click the **Field** drop-down and choose `text`, for the **Operator** choose `contains`, then enter the **Value** of `cloud`. The query `text:cloud` is displayed under the **Visual Query Builder**.

    - Alternatively, you can click **Edit in query language**, then **Use the Discovery Query Language**. Enter `text:"cloud"` into the **Enter query here** field.

1.  Click **Run query**. Copy the **Query URL** at the top of the **Summary** or **JSON** tab to use in your application.

If you'd like to check out a few pre-built queries, click the **Use a sample query** button.
{: tip}

## Step 3: Experiment with different queries

Try out these queries:

To return all documents that do not contain the word `Watson`: Click **Search for Documents**, **Use the Discovery Query Language** then:
-  Click the **Field** drop-down and choose `text`, for the **Operator** choose `does not contain`, then enter the **Value** of `Watson`.

The query `text:!Watson` will display under the **Visual Query Builder**. The operator `:!` specifies "does not contain".

To return documents that exactly match a specific field; for example, to return documents with the file name `test-doc2.html` : Click **Search for Documents**, **Use the Discovery Query Language** then:
-  Click the **Field** drop-down and choose `extracted_metadata.filename`, for the **Operator** choose `is`, then enter the **Value** of `"test-doc2.html"`.

   The query `extracted_metadata.filename::"test-doc2.html"` will display under the **Visual Query Builder**. The operator `::` specifies an exact match.

To return all documents that contain `IBM`, but not `Watson`: Click **Search for Documents**, **Use the Discovery Query Language** then:
-  Click the **Field** drop-down and choose `text`, for the **Operator** choose `contains`, then enter the **Value** of `IBM`. Click **Add rule**, then for the **Field** choose `text`, for the **Operator** choose `does not contain`, then enter the **Value** of `Watson`.

   The query `text:IBM,text:!Watson` will display under the **Visual Query Builder**. The operator `,` specifies "and".

## Step 4: Build a combined query

You can combine query parameters together to build more targeted queries. Let's try using both the `filter` and `query` parameters to return documents. The filter parameter will narrow down the results to only documents that mention `IBM`, and then the query parameter will return all results about `world wide web`,in order of relevance.

1.  Click on the build queries icon ![Query icon](images/search_icon.svg)<!-- {width="20" height="20" style="padding-left:5px;padding-right:5px;"} --> to open the query page. Select the collection that contains the {{site.data.keyword.IBM_notm}} Press Releases and click **Get started**.

1.  Under **Filter which documents you query**:
    -  Click the **Field** drop-down and choose `text`, for the **Operator** choose `contains`, then enter the **Value** of `IBM`.

       The query `text:IBM` will narrow down the documents to only those that mention `IBM`.

1.  Under **Search for Documents**, click **Use the Discovery Query Language**, then:
    -  Click the **Field** drop-down and choose `text`, for the **Operator** choose `contains`, then enter the **Value** of `world wide web`.

       The query `text:"world wide web"` will return all documents that include `world wide web`, and those documents will be ranked in order of relevance.

1.  Click **More options**, then **Fields to return** and choose **Specify**. Select `text`. This will limit the response to the text of the relevant articles and exclude everything else.

1.  Click **Run query**. There will be one matching document: `"matching_results": 1`
