---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-10"

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

# Configuring your service

Before you add your own content to the {{site.data.keyword.dcp_short}} service, you need to configure the service to process the content the way that you want.
{: shortdesc}

The first step is to configure the basic parameters of the service ([Preparing the service for your documents](/docs/services/discovery-icp/building.html#preparing-the-service-for-your-documents)). This includes creating an environment, then creating one or more collections within that environment. When a collection is created, it is automatically populated with the [default configuration](/docs/services/discovery-icp/building.html#the-default-configuration). If you are happy with the settings in the default configuration, you can proceed to upload your content as described at [Adding content](/docs/services/discovery-icp/adding-content.html).

However, you most likely want to specify one or more custom configurations (see [When you need a custom configuration](/docs/services/discovery-icp/building.html#when-you-need-a-custom-configuration)). If this is the case, you need to do the following:

-   identify some sample content (documents that are representative of your files)
-   upload the content ([Uploading sample documents](/docs/services/discovery-icp/building.html#uploading-sample-documents))
-   adjust the conversion process ([Converting sample documents](/docs/services/discovery-icp/building.html#converting-sample-documents))
-   define enrichments ([Adding enrichments](/docs/services/discovery-icp/building.html#adding-enrichments))
-   normalize the results ([Normalizing data](/docs/services/discovery-icp/building.html#normalizing-data))

    After you have created your custom configuration, you can upload your documents ([Adding content](/docs/services/discovery-icp/adding-content.html)).

## Preparing the service for your documents
{: #preparing-the-service-for-your-documents}

In the {{site.data.keyword.dcp_short}} service, the content that you upload is stored in a collection that is part of your environment. You must create the environment and collection before you can upload your content.

-   **Environment** — The environment defines the amount of storage space that you have for content in the {{site.data.keyword.dcp_short}} service. A maximum of one environment can be created for each instance of the {{site.data.keyword.dcp_short}} service.

-   **Collection** — A collection is a grouping of your content within the environment. You must create at least one collection to be able to upload your content.

    Collections are comprised of your private data and any public data sets you choose to add.
    
To create an environment and private data collection with the {{site.data.keyword.dcp_short}} tooling, do the following:

1.  On the **Manage Data** screen, click the ![Cog](images/icon_settings.png) icon and choose **Create environment**. The status of your environment is always available from this drop-down.

1.  After your environment is ready, click the **Upload your own data** button, then you can **Name your new collection**.

    By default, the configuration file is **Default Configuration**. If you have another configuration file available, you can select it, or you can create a new one later and apply it to this collection. You can also select the language of the documents you plan to add to this collection: English (`en`), German (`de`), Spanish (`es`), Arabic (`ar`), Japanese (`ja`), French (`fr`), Italian (`it`), Korean (`ko`), Dutch (`nl`), Chinese (`zh`), or Brazilian Portuguese (`pt-br`). There can be only one language in each of your collections. After you click **Create**, your data collection appears as a tile.

Your environment and data collection are ready! If you want to use the default configuration file, you can start [Adding content](/docs/services/discovery-icp/adding-content.html) immediately. If you want to customize your {{site.data.keyword.dcp_short}} configuration with conversion settings, do not begin adding documents right now; instead, start creating your [custom configuration file](/docs/services/discovery-icp/building.html#custom-configuration).

When documents are uploaded to a data collection, they are converted and enriched by using the configuration file chosen for that collection. If you decide later that you would like to switch a collection to a different configuration file, you can do that, but the documents that have already been uploaded remain converted by the original configuration file. All documents uploaded after switching the configuration file use the new configuration file. If you want the **entire** collection to use the new configuration, you  need to create a new collection, choose that new configuration file, and re-upload all of the documents. The {{site.data.keyword.dcp_short}} service stores the converted text of the documents that you upload. Embedded images in  **Microsoft Word** files are not stored and are not returned in results.
{: note}

### The default configuration
{: #the-default-configuration}

The {{site.data.keyword.dcp_short}} service includes a standard configuration that converts, enriches, and normalize your data without requiring you to manually configure these options.

The default configuration named **Default Configuration** contains enrichments, plus standard document conversions based on font styles and sizes. {{site.data.keyword.dcp_short}} enriches (add cognitive metadata to) the text field of your documents with semantic information collected by the {{site.data.keyword.watson}} enrichment Keyword Extraction. Enrichments are discussed at  [Adding enrichments](/docs/services/discovery-icp/building.html#adding-enrichments).

-   [Microsoft Word conversion](/docs/services/discovery-icp/building.html#microsoft-word-conversion)
-   [HTML conversion](/docs/services/discovery-icp/building.html#html-conversion)
-   [JSON conversion](/docs/services/discovery-icp/building.html#json-conversion)

If you want to create a custom configuration, see [Custom configuration](/docs/services/discovery-icp/building.html#custom-configuration).

### When you need a custom configuration
{: #when-you-need-a-custom-configuration}

Getting the right information out of your content and returning it to your users is the goal of the {{site.data.keyword.dcp_short}} service. Identifying what that information is, and how it is stored in your content, is defined by the configuration that you use to ingest the content. The content types that the {{site.data.keyword.dcp_short}} service can ingest are flexible, meaning that even though your unstructured content is saved in a specific format, it is not required that the structure of that content match the structure of other content of the same type.

-   **I understand that my documents may not be structured in the way the default configuration expects. _How do I know if the default
    settings are right for me?_**
    -   The easiest way to see if the default works for you is to test it by [Uploading sample documents](/docs/services/discovery-icp/building.html#uploading-sample-documents). If the sample JSON results meet your expectations, then no additional configuration is required.

## Custom configuration
{: #custom-configuration}

To create a custom configuration in the {{site.data.keyword.dcp_short}} tooling, open a Private data collection, and on the **Manage Data** screen, click **Switch** next to the name of your **Configuration**. On the **Switch configuration** dialog, click **Create a new configuration**.

After you have named your new configuration file, that name is displayed at the top of the configuration screen. The new configuration file automatically contains the settings and enrichments of the [Default configuration](/docs/services/discovery-icp/building.html#the-default-configuration) file to give you a place to begin.

The three steps of customizing a configuration file are **Convert**, **Enrich**, and **Normalize**.

1.  [Converting sample documents](/docs/services/discovery-icp/building.html#converting-sample-documents)
1.  [Adding enrichments](/docs/services/discovery-icp/building.html#adding-enrichments)
1.  [Normalizing data](/docs/services/discovery-icp/building.html#normalizing-data)

For detailed information about configurations, see the [Configuration reference](/docs/services/discovery-icp/custom-config.html).

### Uploading sample documents
{: #uploading-sample-documents}

To make the configuration process more efficient, you can upload up to ten Microsoft Word, HTML, or JSON files that are representative of your document set. These are called **sample documents**. Sample documents are not added to your collection; they are used only to identify fields that are common to your documents and to customize those fields to your requirements.

When creating a new configuration file in the {{site.data.keyword.dcp_short}} tooling, you can upload sample documents via drag and drop or browse. Click on the file name in the **Upload Sample Documents** pane to preview each file.

#### Remember the following items when uploading sample documents:

-   All of your documents are converted to JSON before they are enriched and indexed.
-   Microsoft Word and PDF documents are converted to HTML first, then JSON.
-   HTML documents are converted directly to JSON.
-   The maximum file size for a sample document is 1MB. Sample documents are stored in your browser's local roaming data folder. To delete your sample documents, click the **Delete** icon.

#### Guidelines for choosing good sample documents:

-   At minimum, you need one sample document for each file type you intend to ingest: Microsoft Word, HTML, and JSON.
-   If you have any unique document types, such as financial reports or press releases, include one of each in your set of sample documents.
-   For HTML documents, choose documents that include HTML tags you would like to exclude, as well as tag attributes you would like to include or exclude.
-   JSON documents need to include any fields you would like to remove or merge together (for example, `zipCode` and `postalCode`).

### Converting sample documents
{: #converting-sample-documents}

Converting your sample documents is the process that enables you to  define how each input type is handled. The file type of content that you upload dictates the number of conversion steps that you need to consider.

Before you start, [upload your sample documents](/docs/services/discovery-icp/building.html#uploading-sample-documents) and open a sample document of the file type you want to configure in the right-hand pane.

To work through the Conversion settings, click through the file types.

-   **If you are converting Microsoft Word files you must do the following:**
    -   Set the Microsoft Word conversion options
    -   Set the HTML conversion options
    -   Set the JSON conversion options
    -   Review the result

-   **If you are converting HTML files you must do the following:**
    -   Set the HTML conversion options
    -   Set the JSON conversion options
    -   Review the result

-   **If you are converting JSON files** you must set the JSON conversion options and review the result.

For each configuration file that you create, there is only one set of conversion options for each step of the process. This means the HTML conversion options are the same for Word files and HTML. If you require different conversion options for each type of content that you are ingesting, or if you have files of the same type that require different types of conversion, you need to store your files in different collections and create separate configuration files for each set of conversion settings.

#### Microsoft Word conversion
{: #microsoft-word-conversion}

Microsoft Word font sizes and font styles are used to convert the headings in your documents properly into `H1`, `H2`, and so on. `H1`s are the document title, and `H2`s and below are subheadings. Use the text boxes and radio buttons to change the default settings if you want. You can also add additional heading levels and Word styles. If your Word documents tend to use a specific font or style name for headings, make sure to add that information. This helps to improve your conversion, which yields better query results.

**Example:** If it is common for your Word documents to use a 20 pt font, italic for heading 2s - change the **Font size range** to **20** to **23** and the **Font style** to **italic**.

After making any changes, click **Apply and Save**.

#### HTML conversion
{: #html-conversion}

You can use this step to remove unnecessary tags and other document info so that you retain only the information you need for your queries.

Default HTML settings:

- Exclude these tags, as well as their content: `<script>`, `<sup>`
- Exclude these tags, but keep their content: `<font>`,`<em>`, `<span>`
- Keep these tag attributes: no default
- Exclude these tag attributes: `EVENT_ACTIONS`
- Keep content that matches this/these XPath(s): no default
- Exclude content that matches this/these XPath(s): no default

After making any changes, click **Apply and Save**.

#### JSON conversion
{: #json-conversion}

The last step of the conversion is to ensure that the converted (or uploaded JSON) is formed the way that you expect it to be before enrichments are applied to the content. You can create rules {{site.data.keyword.watson}} uses to convert your HTML to JSON.

-   You can move, merge, copy, or remove fields. For example, you might want to merge **`zipCode`** and **`postalCode`** because they are two similar terms for the same field.
-   Empty fields (fields that contain no information) are deleted by default. You can change that using the **Remove empty fields** toggle.

After making any changes, click **Apply and Save**.

## Adding enrichments
{: #adding-enrichments}

The {{site.data.keyword.dcp_short}} [default configuration](/docs/services/discovery-icp/building.html#the-default-configuration) enriches (add cognitive metadata to) the `text` field of your ingested documents with semantic information collected by the  {{site.data.keyword.watson}} function Keyword Extraction.

Other {{site.data.keyword.watson}} enrichments are not currently available in certain plans or environments.

**Important:** Only the first 50,000 characters of each JSON field selected for enrichment are enriched.

### Keyword extraction
{: #keyword-extraction}

Important topics in your content that are typically used when indexing data, generating tag clouds, or when searching. The {{site.data.keyword.dcp_short}} service automatically identifies supported languages in your input content, and then identifies and ranks keywords in that content.

Example portion of a document enriched with Keyword Extraction:

```json
  {
    "enriched_text": {
        "keywords": [
            {
                "text": "United States of America",
                "relevance": 0.856695,
                "count": 2
            },
            {
                "text": "People of the United States",
                "relevance": 0.706525,
                "count": 1
            },
            {
                "text": "Vice President of the United States",
                "relevance": 0.673334,
                "count": 2
            },
            {
                "text": "Congress of the United States",
                "relevance": 0.633614,
                "count": 1
            },
    ...
  }
```
{: codeblock}

In the preceding example, you can query the keyword text by accessing `enriched_text.keywords.text`

The `relevance` score ranges from `0.0` to `1.0`. The higher the score, the more relevant the keyword.

The `count` value indicates the number of times the keyword appears in the document.

## Normalizing data
{: #normalizing-data}

The last step in customizing your configuration file is doing a final cleanup, also known as normalization.

In the **Normalize** section of the {{site.data.keyword.dcp_short}} tooling:

-   You can move, merge, copy, or remove fields.
-   Empty fields (fields that contain no information) are deleted by default. You can change that using the **Remove empty fields** toggle.

After making any changes, click **Apply and Save**, then **Done**. You are returned to the **Manage Data** screen, where you can apply this configuration to the collection of your choice.

**Note:** You cannot specify the data type (For example: `text` or `date`) of fields. During document ingestion, if a field is detected that does not yet exist in the index, {{site.data.keyword.dcp_short}} automatically detects the data type of that field based on the value of the field for the first document indexed.

### Using CSS selectors to extract fields
{: #using-css}

You can perform an additional normalization by using CSS selectors via the {{site.data.keyword.dcp_short}} API.

If you are ingesting well-formed HTML, you can normalize it, use CSS selectors to extract JSON fields from it, and then apply enrichments to the extracted fields. Edit your configuration file to enable this feature. Specifically, add an `extracted_fields` element to the `conversions/html` hierarchy, then specify field names, CSS selectors, and field types, as follows:

```json
{
  "name": "Extract JSON config",
  "description": "New configuration enabling extraction of JSON fields from HTML",
  "conversions": {
    ...
    "html": {
      ...
      "extracted_fields": {
        "{field_name_1}": {
          "css_selector": "{CSS_selector_expression_1}",
          "type": "{field_type}"
        },
        ...
        "{field_name_N}": {
          "css_selector": "{CSS_selector_expression_N}",
          "type": "{field_type}"
        }
      }
    ...
    }
  }
}
```
{: codeblock}

Specify values for the new fields as follows:

-   `field_name` — The name of the field that will be added to the JSON output.
-   `CSS_selector_expression` — The CSS selector that is to be run against the input HTML to extract the fields. The expression can have one or more matches.

    Valid CSS selectors are those specified by the [JSoup parser ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://jsoup.org/apidocs/org/jsoup/select/Selector.html){: new_window} and its [selector syntax ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://jsoup.org/cookbook/extracting-data/selector-syntax){: new_window}. A short list is provided at [Common selectors](/docs/services/discovery-icp/building.html#common-selectors).
-   `field_type` — Either `array` or `string`. If the field type is not specified, it defaults to `array`. Note that type `string` can be enriched, but information stored in an `array` cannot be enriched unless the array's items are first extracted into text fields.

**Warning:** If a CSS selector matches both a parent node and one or more of its children, the text content of the nodes will be duplicated in the JSON output.

**Note:** Field names must meet the restrictions defined in [Field name requirements](/docs/services/discovery-icp/custom-config.html#field_reqs).

The following JSON passage shows the relevant section of the Default Configuration to which you add CSS selector information.

```json
{
  "name": "Default Configuration",
  "description": "The configuration used by default when creating a new collection without specifying a configuration_id.",
  "conversions": {
    ...
    "html": {
      "exclude_tags_completely": [
        "script",
        "sup"
      ],
      "exclude_tags_keep_content": [
        "font",
        "em",
        "span"
      ],
      "exclude_content": {
        "xpaths": []
      },
      "keep_content": {
        "xpaths": []
      },
      "exclude_tag_attributes": [
        "EVENT_ACTIONS"
      ]
    }
    ...
  }
}
```
{: codeblock}

The following passage shows the configuration with a new name and description, as well as the location where you can specify CSS selectors.

```json
{
  "name": "Extract JSON config",
  "description": "New configuration enabling extraction of JSON fields from HTML",
  "conversions": {
    ...
    "html": {
      "exclude_tags_completely": [
        "script",
        "sup"
      ],
      "exclude_tags_keep_content": [
        "font",
        "em",
        "span"
      ],
      "exclude_content": {
        "xpaths": []
      },
      "keep_content": {
        "xpaths": []
      },
      "exclude_tag_attributes": [
        "EVENT_ACTIONS"
      ],
      "extracted_fields": {
        "{field_name_1}": {
          "css_selector": "{CSS_selector_expression_1}",
          "type": "{field_type}"
        },
        ...
        "{field_name_N}": {
          "css_selector": "{CSS_selector_expression_N}",
          "type": "{field_type}"
        }
      }
    }
  }
  ...
}
```
{: codeblock}

Finally, the following passage shows the configuration the new name and description as well as some CSS selectors. The selectors match items in an HTML example that is described further down on this page.

```json
{
  "name": "Extract JSON config",
  "description": "New configuration enabling extraction of JSON fields from HTML",
  "conversions": {
    ...
    "html": {
      "exclude_tags_completely": [
        "script",
        "sup"
      ],
      "exclude_tags_keep_content": [
        "font",
        "em",
        "span"
      ],
      "exclude_content": {
        "xpaths": []
      },
      "keep_content": {
        "xpaths": []
      },
      "exclude_tag_attributes": [
        "EVENT_ACTIONS"
      ],
      "extracted_fields": {
        "chapters": {
          "css_selector": ".chapter",
          "type": "array"
        },
        "authors": {
          "css_selector": ".author"
        },
        "authors_str": {
          "css_selector": ".author",
          "type": "string"
        },
        "comments": {
          "css_selector": "[^comments-content]"
        }
      }
    }
  }
  ...
}
```
{: codeblock}

If you load the following HTML document into a collection that uses an updated configuration, the CSS selectors match the appropriate attributes and values in the HTML.

```html
<html>
  <body>
    <div id="authors">
      <div class="author"><span class="first_name">Jane</span> <span class="last_name">Rain</span></div>
      <div class="author">Joe Snow</div>
  </div>
  <div class="chapter"><h1>The Rain in Spain</h1><p>falls mainly on the plain.</p></div>
  <div class="chapter"><h1>How I Learned to Stop Worrying</h1><p>and love my snowblower.</p></div>
  <span id="comments-section">
    <h4>Comments:</h4>
    <span id="comments-content-1">Rain gives me pain.</span>
    <span id="comments-content-2">All snow must go!</span>
  </span>
</body></html>
```
{: codeblock}

After the preceding HTML is ingested and enhanced, the {{site.data.keyword.dcp_short}} service returns the following JSON:

```json
{
  "extracted_metadata": { ... },
  "html": "...",
  "text": "...",
  "extracted_fields": {
    "authors": [ "Jane Rain", "Joe Snow" ],
    "authors_str": "Jane Rain\n\nJoe Snow",
    "chapters": [ "The Rain in Spain\n\nfalls mainly on the plain.", "How I Learned to Stop Worrying\n\nand love my snowblower." ],
    "comments": [ "Rain gives me pain.", "All snow must go!" ]
  }
}
```
{: codeblock}

After deciding which HTML elements you want to extract, you can then further modify the configuration file to specify the enrichments you want to apply to them.

#### Common selectors

Some common CSS selectors include the following:

  - `tag`: Matches the `tag` name
  - `.class`: Matches the value of `class`
  - `#id`: Matches the value of `id`
  - `[attribute]`: Matches any tag with the specified `attribute`, regardless of value
  - `[attribute=value]` or `[attribute="value"]`: Matches the specified `attribute` and `value`

