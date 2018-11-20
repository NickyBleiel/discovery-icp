---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-14"

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

The {{site.data.keyword.dcp_long}} service offers powerful content search capabilities through queries. After your content is uploaded and enriched by the {{site.data.keyword.dcp_short}} service, you can build queries or integrate {{site.data.keyword.dcp_short}} into your own projects.
{: shortdesc}

For more information about writing queries, see:
- [Getting started with querying](/docs/services/discovery-icp/getting-started-query.html)
- [Query concepts](/docs/services/discovery-icp/using.html)

## Parameters descriptions
{: #parameter-descriptions}

Query parameters enable you to search your collection, identify a result set, and perform analysis on the result set.


| Parameter | Description | Example |
|:-------------------:|------------------------------------------------------------|--------------------------------|
|** Search parameters **|  |  |
| [query](/docs/services/discovery-icp/query-parameters.html#query) | A ranked query language search for matching documents. | `query=bees` |
| [filter](/docs/services/discovery-icp/query-parameters.html#filter) | An unranked query language search for matching documents. | `filter=bees` |
| [aggregation](/docs/services/discovery-icp/query-parameters.html#aggregation) | A statistical query of the results set | `aggregation=term(enriched_text.entities.type)` |
| **Structure parameters** | | |
| [count](/docs/services/discovery-icp/query-parameters.html#count) | The number of `result` documents to return. | `count=15` |
| [offset](/docs/services/discovery-icp/query-parameters.html#offset) | The number of results to ignore before returning `result` documents from the results set | `offset=100` |
| [return](/docs/services/discovery-icp/query-parameters.html#return) | List of fields to return | `return=title,url` |
| [sort](/docs/services/discovery-icp/query-parameters.html#sort) | Field to sort results set by | `sort=enriched_text.sentiment.document.score` |
| [bias](/docs/services/discovery-icp/query-parameters.html#bias) | Field to bias results set by | `bias=publication_date` |
| [highlight](/docs/services/discovery-icp/query-parameters.html#highlight) | Highlight query matches | `highlight=true` |
| [collection_ids](/docs/services/discovery-icp/query-parameters.html#collection_ids) | Query multiple collections | `collection_ids={1},{2},{3}` |

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
| [.](/docs/services/discovery-icp/query-operators.html#delimiter) | JSON delimiter | `enriched_text.keywords.text` |
| [:](/docs/services/discovery-icp/query-operators.html#includes) | Includes | `text:computer` |
| [::](/docs/services/discovery-icp/query-operators.html#match) | Exact match | `title::"Query building"` |
| [:!](/docs/services/discovery-icp/query-operators.html#notinclude) | Does not include | `text:!computer` |
| [::!](/docs/services/discovery-icp/query-operators.html#notamatch) | Not an exact match | `text::!winter` |
| [\\](/docs/services/discovery-icp/query-operators.html#escape) | Escape character | `title::"Dorothy said: \"There's no place like home\""` |
| [""](/docs/services/discovery-icp/query-operators.html#phrase) | Phrase query | `enriched_text.keywords.text:Watson"` |
| [(), \[\]](/docs/services/discovery-icp/query-operators.html#nestedquery) | Nested grouping | `filter-entities:(text:Turkey,type:Location)` |
| [<code>&#124;</code>](/docs/services/discovery-icp/query-operators.html#or) | or | <code>query-enriched.keywords.text:Google&#124;IBM</code> |
| [,](/docs/services/discovery-icp/query-operators.html#and) | and | `query-enriched.keywords.text:Google,IBM` |
| [<=, >=, >, <](/docs/services/discovery-icp/query-operators.html#comparisons) | Numerical comparisons |  `enriched_text.sentiment.document.score>0.679`     |
| [^x](/docs/services/discovery-icp/query-operators.html#multiplier) | Score multiplier | `text:IBM^3` |
| [*](/docs/services/discovery-icp/query-operators.html#wildcard) | Wildcard | `query-enriched_text.keywords.text:pre*` |
| [~n](/docs/services/discovery-icp/query-operators.html#variation) | String variation | `query-enriched_text.keywords.text:cat~1` |
| [:*](/docs/services/discovery-icp/query-operators.html#exists) | Exists | `title:*` |
| [!*](/docs/services/discovery-icp/query-operators.html#dnexist) | Does not exist | `title!*` |

## Aggregations
{: #aggregations}

Aggregations return a set of data values. These are the available aggregations:

| Aggregation | Description | Example |
|:-------------------:|------------------------------------------------------------|--------------------------------|
| [term](/docs/services/discovery-icp/query-aggregations.html#term) | Count of identical values | `term(enriched_text.keywords.text,count:10)` |
| [filter](/docs/services/discovery-icp/query-aggregations.html#filter) | Filter results set to defined pattern | `filter(enriched_text.keywords.text:cloud)`
| [nested](/docs/services/discovery-icp/query-aggregations.html#nested) | Restrict aggregation | `nested(enriched_text.keywords)` |
| [histogram](/docs/services/discovery-icp/query-aggregations.html#histogram) | Interval based distribution | `histogram(product.price,interval:1)` |
| [timeslice](/docs/services/discovery-icp/query-aggregations.html#timeslice) | Time base distribution | `timeslice(last_modified,2day,America/New York)` |
| [top_hits](/docs/services/discovery-icp/query-aggregations.html#top_hits) | Top ranked results documents for the current aggregation | `term(enriched_text.keywords.text).top_hits(10)` |
| [unique_count](/docs/services/discovery-icp/query-aggregations.html#unique_count) | Count of unique values for a field within an aggregation | `unique_count(enriched_text.keywords.term)` |
| [max](/docs/services/discovery-icp/query-aggregations.html#max) | Maximum value for the specified field in the results set. | `max(product.price)` |
| [min](/docs/services/discovery-icp/query-aggregations.html#min) | Minimum value for the specified field in the results set. | `min(product.price)` |
| [average](/docs/services/discovery-icp/query-aggregations.html#average) |Mean value for the specified field in the results set. | `average(product.price)` |
| [sum](/docs/services/discovery-icp/query-aggregations.html#sum) | Sum of all fields in the results set. | `sum(product.price)` |
