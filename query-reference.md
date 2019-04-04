---

copyright:
  years: 2015, 2019
lastupdated: "2018-04-01"

subcollection: discovery-icp

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

# Query reference
{: #query-ref}

The {{site.data.keyword.dcp_long}} service offers powerful content search capabilities through queries. After your content is uploaded and enriched by the {{site.data.keyword.dcp_short}} service, you can build queries or integrate {{site.data.keyword.dcp_short}} into your own projects.
{: shortdesc}

For more information about writing queries, see:
- [Getting started with querying](/docs/services/discovery-icp?topic=discovery-icp-query-gs)
- [Query concepts](/docs/services/discovery-icp?topic=discovery-icp-query-concepts)

## Parameters descriptions
{: #parameter-descriptions}

Query parameters enable you to search your collection, identify a result set, and perform analysis on the result set.


| Parameter | Description | Example |
|:-------------------:|------------------------------------------------------------|--------------------------------|
|** Search parameters **|  |  |
| [query](/docs/services/discovery-icp?topic=discovery-icp-query-parameters#query) | A ranked query language search for matching documents. | `query=bees` |
| [filter](/docs/services/discovery-icp?topic=discovery-icp-query-parameter#filter) | An unranked query language search for matching documents. | `filter=bees` |
| [aggregation](/docs/services/discovery-icp?topic=discovery-icp-query-parameter#aggregation) | A statistical query of the results set | `aggregation=term(enriched_text.entities.type)` |
| **Structure parameters** | | |
| [count](/docs/services/discovery-icp?topic=discovery-icp-query-parameters#count) | The number of `result` documents to return. | `count=15` |
| [offset](/docs/services/discovery-icp?topic=discovery-icp-query-parameters#offset) | The number of results to ignore before returning `result` documents from the results set | `offset=100` |
| [return](/docs/services/discovery-icp?topic=discovery-icp-query-parameters#return) | List of fields to return | `return=title,url` |
| [sort](/docs/services/discovery-icp?topic=discovery-icp-query-parameters#sort) | Field to sort results set by | `sort=enriched_text.sentiment.document.score` |
| [bias](/docs/services/discovery-icp?topic=discovery-icp-query-parametersl#bias) | Field to bias results set by | `bias=publication_date` |
| [highlight](/docs/services/discovery-icp?topic=discovery-icp-query-parameters#highlight) | Highlight query matches | `highlight=true` |
| [collection_ids](/docs/services/discovery-icp?topic=discovery-icp-query-parameters#collection_ids) | Query multiple collections | `collection_ids={1},{2},{3}` |

### Query limitations

You cannot query on field names that contain the following:
- Numerical characters (`0 - 9`) in the suffix of the field name (for example `extracted-content2`).
- The characters `_`, `+`, and `-` in the prefix of the `field_name` (for example `+extracted-content`).
- The characters `.`, `,`, and `:` in the `field_name` (for example `new:extracted-content`)

## Operators
{: #operators}

Operators are the separators between different parts of a query. These are the available operators:

| Operator | Description | Example |
|:-------------------:|------------------------------------------------------------|--------------------------------|
| [.](/docs/services/discovery-icp?topic=discovery-icp-query-operators#delimiter) | JSON delimiter | `enriched_text.keywords.text` |
| [:](/docs/services/discovery-icp?topic=discovery-icp-query-operators#includes) | Includes | `text:computer` |
| [::](/docs/services/discovery-icp?topic=discovery-icp-query-operators#match) | Exact match | `title::"Query building"` |
| [:!](/docs/services/discovery-icp?topic=discovery-icp-query-operators#notinclude) | Does not include | `text:!computer` |
| [::!](/docs/services/discovery-icp?topic=discovery-icp-query-operators#notamatch) | Not an exact match | `text::!winter` |
| [\\](/docs/services/discovery-icp?topic=discovery-icp-query-operators#escape) | Escape character | `title::"Dorothy said: \"There's no place like home\""` |
| [""](/docs/services/discovery-icp?topic=discovery-icp-query-operators#phrase) | Phrase query | `enriched_text.keywords.text:Watson"` |
| [(), \[\]](/docs/services/discovery-icp?topic=discovery-icp-query-operators#nestedquery) | Nested grouping | `filter-entities:(text:Turkey,type:Location)` |
| [<code>&vert;</code>](/docs/services/discovery-icp?topic=discovery-icp-query-operators#or) | or | <code>query-enriched.keywords.text:Google&vert;IBM</code> |
| [,](/docs/services/discovery-icp?topic=discovery-icp-query-operators#and) | and | `query-enriched.keywords.text:Google,IBM` |
| [<=, >=, >, <](/docs/services/discovery-icp?topic=discovery-icp-query-operators#comparisons) | Numerical comparisons |  `enriched_text.sentiment.document.score>0.679`     |
| [^x](/docs/services/discovery-icp?topic=discovery-icp-query-operators#multiplier) | Score multiplier | `text:IBM^3` |
| [*](/docs/services/discovery-icp?topic=discovery-icp-query-operators#wildcard) | Wildcard | `query-enriched_text.keywords.text:pre*` |
| [~n](/docs/services/discovery-icp?topic=discovery-icp-query-operators#variation) | String variation | `query-enriched_text.keywords.text:cat~1` |
| [:*](/docs/services/discovery-icp?topic=discovery-icp-query-operators#exists) | Exists | `title:*` |
| [!*](/docs/services/discovery-icp?topic=discovery-icp-query-operators#dnexist) | Does not exist | `title!*` |

## Aggregations
{: #aggregations}

Aggregations return a set of data values. These are the available aggregations:

| Aggregation | Description | Example |
|:-------------------:|------------------------------------------------------------|--------------------------------|
| [term](/docs/services/discovery-icp?topic=discovery-icp-query-aggregations#term) | Count of identical values | `term(enriched_text.keywords.text,count:10)` |
| [filter](/docs/services/discovery-icp?topic=discovery-icp-query-aggregations#filter) | Filter results set to defined pattern | `filter(enriched_text.keywords.text:cloud)`
| [nested](/docs/services/discovery-icp?topic=discovery-icp-query-aggregations#nested) | Restrict aggregation | `nested(enriched_text.keywords)` |
| [histogram](/docs/services/discovery-icp?topic=discovery-icp-query-aggregations#histogram) | Interval based distribution | `histogram(product.price,interval:1)` |
| [timeslice](/docs/services/discovery-icp?topic=discovery-icp-query-aggregations#timeslice) | Time base distribution | `timeslice(last_modified,2day,America/New York)` |
| [top_hits](/docs/services/discovery-icp?topic=discovery-icp-query-aggregations#top_hits) | Top ranked results documents for the current aggregation | `term(enriched_text.keywords.text).top_hits(10)` |
| [unique_count](/docs/services/discovery-icp?topic=discovery-icp-query-aggregations#unique_count) | Count of unique values for a field within an aggregation | `unique_count(enriched_text.keywords.term)` |
| [max](/docs/services/discovery-icp?topic=discovery-icp-query-aggregations#max) | Maximum value for the specified field in the results set. | `max(product.price)` |
| [min](/docs/services/discovery-icp?topic=discovery-icp-query-aggregations#min) | Minimum value for the specified field in the results set. | `min(product.price)` |
| [average](/docs/services/discovery-icp?topic=discovery-icp-query-aggregations#average) |Mean value for the specified field in the results set. | `average(product.price)` |
| [sum](/docs/services/discovery-icp?topic=discovery-icp-query-aggregations#sum) | Sum of all fields in the results set. | `sum(product.price)` |
