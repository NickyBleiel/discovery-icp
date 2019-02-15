---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-15"

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

# Query concepts
{: #query-concepts}

The {{site.data.keyword.dcp_long}} service offers powerful content search capabilities. After your content is uploaded and enriched by the {{site.data.keyword.dcp_short}} service, you can build queries, then integrate {{site.data.keyword.dcp_short}} into your own projects.
{: shortdesc}

  The queries you write vary by collection, because all collections contain unique content.
  {: tip}

When you create a query or filter, {{site.data.keyword.dcp_short}} looks at each result and tries to match the paths you have defined. When matches occur, they are added to the results set. When creating a query, you can be as vague or as specific as you want. The more specific the query, the more targeted the results.

For more information about writing queries, see:
- [Getting started with querying tutorial](/docs/services/discovery-icp/getting-started-query.html#)
- [Query reference](/docs/services/discovery-icp/query-reference.html#query-ref) (includes the list of parameters, operators, and aggregations available in the Discovery Query Language)

## The Watson Assistant Discovery Extension data schema
{: #discovery-schema}

Start out by getting to know the {{site.data.keyword.dcp_short}} JSON. To understand how to build a query using the Discovery Query Language, you need to be familiar with the JSON produced by {{site.data.keyword.dcp_short}} after it enriches the documents in your collection. Once you are familiar with the data schema of your documents, it will be easier to write queries in the Discovery Query Language. There are three ways to do this.

  1. In the {{site.data.keyword.dcp_short}} Tooling, open the **Manage data** screen, choose the collection that contains the {{site.data.keyword.IBM_notm}} Press Releases. Click the **View Data Schema** button. The **View data schema** screen displays the fields and values in your transformed documents two ways: by document (**Document view**), or by field (**Collection view**). A maximum of 50 documents displays in **Document view**. **Collection view** displays the fields in the entire collection.

    In the **Collection view**, under `enriched_text`, you can examine the enrichments you applied with the **Default Configuration** file. Click on `keywords` to see how your collection was enriched with Watson insights.

  1. Run an "empty" query to view the JSON. On the **View data schema** screen, click the **Build queries** button, then click **Run Query**. The results display on the right, under two tabs, **Summary** (an overview of the query results) and **JSON**. Start by opening the **JSON** tab.

     -  Each of the four documents is proceeded by an `id` number.
     -  Scroll down to the `enriched_text` field. Examine each enrichment to learn about the JSON fields you can query on.
     -  **keywords** - Start by finding the `text` field, and examine the other enrichment information.

     After you have examined the insights in the first document, you can look at the other three documents if you like.

  1. View the available fields in the **Visual Query Builder**. On the **Build queries** screen, click **Search for documents**, then **Use the Discovery Query Language**. Click the **Field** drop-down to view the fields available in your data. Click **Edit in query language** to build queries manually by using the Discovery Query Language.

### How to structure a basic query
{: #structure-basic-query}

As you have noticed, the JSON is hierarchical, so queries need to be written using that same hierarchy. So if your JSON looks like this:

```json
"enriched_text": {
  "keywords": [
    {
    "text": "Cloud computing",
    "relevance": 0.610029,
    "count": 2}
    ]
  }
```
{: codeblock}

Structure your query as follows:

```
enriched_text.keywords.text:Cloud computing
```

The elements of the query as are follows:

  - `enriched_text`: field
  - `.`: delimiter
  - `keywords`: field
  - `.`: delimiter
  - `text`:  field
  - `:`: operator
  - `Cloud computing`: value

![Example query structure](images/query_structure2.png)

  Operators that evaluate a field (`<=` , `>=`, `<`, `>`) require a `number` or `date` as the value. Using quotations around a value always makes it a `string`. Therefore `relevance>=0.5` is a valid query and `relevance>="0.5"` is not. See [Query operators](/docs/services/discovery-icp/query-operators.html#query-operators) for a complete list of operators.
  {: tip}

## Building combined queries
{: #building-combined-queries}

You can combine query parameters together to build more targeted queries. For example. you can use the both the `filter` and `query` parameters together. For more information about filtering vs. querying, see [Differences between the filter and query parameters](/docs/services/discovery-icp/query-parameters.html#filtervquery).

## How to structure an aggregation
{: #structure-aggregation}

Aggregations return a set of data values, including top keywords. For the full list of aggregation options, see [Aggregations](/docs/services/discovery-icp/query-reference.html#aggregations).

```
term(enriched_text.keywords.text)
```

The elements of the aggregation are as follows:

  - `term`: aggregation type
  - `(`: start grouping
  - `enriched_text`: field
  - `.`: delimiter
  - `keywords`: field
  - `.`: delimiter
  - `text`: field
  - `)`: end grouping

This example aggregation will find all of the `keywords` in your collection.
The delimiter in this query is `.` and the operator is `()`. See [Query operators](/docs/services/discovery-icp/query-operators.html#query-operators) to learn about other operators available in the Discovery Query Language.

### Example aggregation queries
{: #example-aggregations}

There are several types of ways you can aggregate results with {{site.data.keyword.dcp_short}}, including top values, sum, min, max, average, timeslice, and histogram. You can also add filters and nest aggregations.

#### Filter aggregations
{: #filter-aggregations}

The following example aggregation returns the number of documents found in a {{site.data.keyword.dcp_short}} collection about the National Football League (NFL) and how many of those documents mention "Pittsburgh" or "Steelers."

- `filter(enriched_text.keywords.text:"NFL|National Football League").term(enriched_text.keywords.text:"Pittsburgh|Steelers")`

The following example aggregation first narrows down (filters) a set of articles in a {{site.data.keyword.dcp_short}} documents to only the documents that include the keyword text `banana`, then divide those documents that include the keyword text `apple`.

- `filter(enriched_text.keywords.text:banana).term(enriched_text.keywords.text:apple)`

#### Nested aggregations
{: #nested-aggregations}

Adding `nested` before an aggregation restricts the aggregation to the area of the results specified. For example, `nested(enriched_text.keywords)` means that only the `enriched_text.keywords` components of any result are used to aggregate against. 

Additionally, any subsequent operation further restrict the result set that can be aggregated against. For example, `nested(enriched_text.keywords).filter(enriched_text.keywords.count:5)` means that only keywords that occur 5 times are aggregated.

