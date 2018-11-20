---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-17"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:note: .note}
{:important: .important}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Query parameters
{: #query-parameters}

The {{site.data.keyword.dcp_long}} service offers powerful content search capabilities through queries. After your content is uploaded and enriched by the {{site.data.keyword.dcp_short}} service, you can build queries, integrate {{site.data.keyword.dcp_short}} into your own projects, or create a custom application by using the {{site.data.keyword.watson}} Explorer Application Builder. To get started with queries, see [Query concepts](/docs/services/discovery-icp/using.html). For the complete list of parameters, see the [Query reference](/docs/services/discovery-icp/query-reference.html#parameter-descriptions).
{: shortdesc}

**Search parameters**

Search parameters enable you to search your collection, identify a result set, and perform analysis on the result set.

The **results set** is the group of documents identified by the combined searches of the search parameters. The results set may be significantly larger than the returned results. If an empty query is performed, the results set is equal to all the documents in the collection.

## query
{: #query}

A query search returns all documents in your data set with full enrichments and full text in order of relevance. A query also excludes any documents that do not mention the query content. These queries are written by using the [Discovery Query Language](/docs/services/discovery-icp/query-operators.html).

## filter
{: #filter}

A cacheable query that excludes any documents that do not mention the query content. Filter search results are **not** returned in order of relevance. These queries are written by using the [Discovery Query Language](/docs/services/discovery-icp/query-operators.html)

### Differences between the filter and query parameters
{: #filtervquery}

If you test the same search term on a small data set, you might find that the `filter` and `query` parameters return very similar (if not identical) results. However, there is a difference between the two parameters.

- Using a filter parameter alone returns search results in no specific order.
- Using a query parameter alone returns search results in order of relevance.

In large data sets, if you need results to be returned in order of relevance,  combine the `filter` and `query` parameters, because using them together improves performance. The `filter` parameter runs first and caches results, then the `query` parameter ranks them. For an example of using filters and queries together, see [Building combined queries](/docs/services/discovery-icp/using.html#building-combined-queries). Filters can also be used in aggregations.

When you write a query that includes both a `filter`, and an `aggregation` or `query`; the `filter` parameters run first, after which any `aggregation` or `query` parameters run in parallel.

With a simple query, especially on a small data set, `filter` and `query` often return the same or similar results. If a `filter` and `query` call return similar results, and getting a response in order of relevance does not matter, it is better to use `filter` because filter calls are faster and are cached. Caching means that the next time you make that call, you get a much quicker response, particularly in a big data set.

## aggregation
{: #aggregation}

Aggregation queries return a count of documents matching a set of data values; for example, top keywords. For the full list of aggregation options, see the [Aggregations table](/docs/services/discovery-icp/query-aggregations.html). These aggregations are written by using the [Discovery Query Language](/docs/services/discovery-icp/query-operators.html)

**Structure parameters**

Structure parameters define the content and organization of the documents in the returned JSON. This includes the number of results retuned, where in the results set to start returning documents, how the result set is sorted, and which fields to return for each documents. Structure parameters do not affect which documents are part of the entire results set.

## count
{: #count}

The number of documents that you want to be returned in the response. The default is `10`. The maximum for the `count` and `offset` values together in any single query is `10000`.

## offset
{: #offset}

The number of query results to skip at the beginning. For example, if the total number of results that are returned is 10, and the offset is 8, it returns the last two results. The default is `0`. The maximum for the `count` and `offset` values together in any one query is `10000`.

## return
{: #return}

A comma-separated list of the portion of the document hierarchy to return. Any of the document hierarchy are valid values.

## sort
{: #sort}

A comma-separated list of fields in the document to sort on. You can optionally specify a sort direction by prefixing the field with `-` for descending order or `+` for ascending order. Ascending order is the default sort direction.

The `sort` parameter is currently available for use only with the API; it is not available through the tooling.

## bias
{: #bias}

Adjusts search results to bias towards certain results, for example, documents that were published most recently. `bias` must be set to either a `date` type field or a `number` type field, for example `bias=publication_date` or `bias=field_1`.  When a `date` type field is specified, returned results are biased towards field values closer to the current date. When a `number` type field is specified, returned results are biased towards higher field values. This parameter cannot be used in the same query as the `sort` parameter.

The `bias` parameter is currently available for use only with the API; it is not available through the tooling.

