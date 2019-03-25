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

# Query operators
{: #query-operators}

Operators are the separators between different parts of a query. For the complete list of available operators, see the [Query reference](/docs/services/discovery-icp?topic=discovery-icp-query-ref#operators).

## . \[JSON delimiter\]
{: #delimiter}

This delimiter separates the levels of hierarchy in the JSON schema

For example:
```bash
enriched_text.keywords.text
```
{: codeblock}

## : \[Includes\]
{: #includes}

This operator specifies a match for the query term.

For example:
```bash
enriched_text.keywords.text:"cloud"
```
{: codeblock}

## :: \[Exact match\]
{: #match}

This operator specifies an exact match for the query term.

For example:
```bash
enriched_text.keywords.text::"Cloud"
```
{: codeblock}

Exact matches are case-sensitive.

## :! \[Does not include\]
{: #notinclude}

This operator specifies that the results do not contain a match for the query term

For example:
```bash
enriched_text.keywords.text:!"cloud"
```
{: codeblock}

## ::! \[Not an exact match\]
{: #notamatch}

This operator specifies that the results do not exactly match the query term

For example:
```bash
enriched_text.keywords.text::!"Cloud"
```
{: codeblock}

Exact matches are case-sensitive.

## \\ \[Escape character\]
{: #escape}

Escape character for queries that require the ability to query terms by using string literals that contain control characters.

For example:
```bash
title::"Dorothy said: \"There's no place like home\""
```
{: codeblock}

## "" \[Phrase query\]
{: #phrase}

All contents of a phrase query are processed as escaped. So no special characters within a phrase query are parsed, except for double quotes (`"`) inside a phrase query, which must be escaped (`\"`). Use phrase queries with full-text, rank-based queries, and not with boolean filter operations. Do not use wildcards (`*`) in phrase queries. **Note**: Single quotes (`'`) are not supported.)

For example:
```bash
text:"IBM"
```
{: codeblock}

## (), \[\] \[Nested grouping\]
{: #nestedquery}

Logical groupings can be formed to specify more specific information.

For example:
```bash
enriched_text.keywords:(text:IBM,type:Company)
```
{: codeblock}

## \| \[or\]
{: #or}

Boolean operator for "or".

For example:
```bash
text:Google|IBM
```
{: codeblock}

## , \[and\]
{: #and}

Boolean operator for "and".

For example:
```bash
text:Google,IBM
```
{: codeblock}

## <=, >=, >, < \[Numerical comparisons\]
{: #comparisons}

Creates numerical comparisons of less than or equal to, greater than or equal to, greater than, and less than.

For example:
```bash
enriched_text.keywords.document.score>0.679
```
{: codeblock}

## ^x \[Score multiplier\]
{: #multiplier}

Increases the score value of a search term.

For example:
```bash
text:IBM^3
```
{: codeblock}

## * \[Wildcard\]
{: #Wildcard}

Matches unknown characters in a search expression. Do not use capital letters with wildcards.

For example:
```bash
text:ib*
```
{: codeblock}

## ~n \[String variation\]
{: #variation}

The number of one character changes that need to be made to one string to make it the same as another string. For example `car~1` matches `car`,`cap`,`cat`,`can`, etc.

For example:
```bash
text:Watson~3
```
{: codeblock}

## :* \[Exists\]
{: #exists}

Used to return all results where the specified `field` exists.

For example:
```bash
title:*
```
{: codeblock}

## !* \[Does not exist\]
{: #dnexist}

Used to return all results that do not include the specified `field`.

For example:
```bash
title!*
```
{: codeblock}