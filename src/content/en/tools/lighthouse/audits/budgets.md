project_path: /web/tools/_project.yaml
description: Official documentation for the "Performance Budgets" and "Keep Request Counts And File Sizes Small" Lighthouse audits.
book_path: /web/tools/_book.yaml

{# wf_updated_on: 2019-05-01 #}
{# wf_published_on: 2019-04-30 #}
{# wf_blink_components: Platform>DevTools #}

# Performance Budgets (Keep Request Counts And File Sizes Small) {: .page-title }

## Overview {: #overview }

<aside class="note">
  The <b>Performance Budgets</b> and <b>Keep Request Counts And File Sizes Small</b> audits both
  link to this page.
</aside>

After putting in the hard work of improving your site performance, use a *performance budget* to prevent
your site performance from regressing over time.

If you haven't defined a budget file the **Budgets** section of your report lists the total number of requests
and file size of scripts, images, and so on. The **Budgets** section is only available when running Lighthouse
from the command line.

<figure>
  <img src="images/budgetsdefault.png"
       alt="A default budget report."/>
  <figcaption>
    <b>Figure X</b>. A default budget report.
  </figcaption>
</figure>

This same information also appears under the **Keep Request Counts And File Sizes Small** audit. This
audit is available in all Lighthouse runtime environments.

<figure>
  <img src="images/requestcounts.png"
       alt="The Keep Request Counts And File Sizes Small audit."/>
  <figcaption>
    <b>Figure X</b>. The <b>Keep Request Counts And File Sizes Small</b> audit.
  </figcaption>
</figure>

Example JSON output:

```json
"resource-budget": {
  "id": "resource-budget",
  "title": "Keep request counts and file sizes small",
  "description": "To set budgets for the quantity and size of page resources, add a budget.json file.",
  "score": null,
  "scoreDisplayMode": "informative",
  "details": {
    "type": "table",
    "headings": [
      {
        "key": "label",
        "itemType": "text",
        "text": "Resource Type"
      },
      {
        "key": "count",
        "itemType": "numeric",
        "text": "Requests"
      },
      {
        "key": "size",
        "itemType": "bytes",
        "text": "File Size"
      }
    ],
    "items": [
      {
        "label": "Total",
        "count": 21,
        "size": 704743
      },
      {
        "label": "Script",
        "count": 2,
        "size": 444321
      },
      {
        "label": "Third-party",
        "count": 13,
        "size": 166841
      },
      {
        "label": "Image",
        "count": 6,
        "size": 135096
      },
      {
        "label": "Stylesheet",
        "count": 2,
        "size": 78948
      },
      {
        "label": "Font",
        "count": 2,
        "size": 29119
      },
      {
        "label": "Document",
        "count": 2,
        "size": 14588
      },
      {
        "label": "Other",
        "count": 7,
        "size": 2671
      },
      {
        "label": "Media",
        "count": 0,
        "size": 0
      }
    ]
  }
}
```

You can define a budget file to impose explicit limits on some or all of these categories. After defining
your budget file the new **Over Budget** column tells you whether you're exceeding any of your self-defined limits.

<figure>
  <img src="images/budgetscustom.png"
       alt="A custom budget report."/>
  <figcaption>
    <b>Figure X</b>. A custom budget report.
  </figcaption>
</figure>

When you define a budget file, the **Performance Budgets** audit only reports on the categories that you've specified.
But you can still access the information for the rest of the categories from the **Keep Request Counts And File Sizes Small**
audit.

Example JSON output:

```json
"resource-budget": {
  "id": "resource-budget",
  "title": "Keep request counts and file sizes small",
  "description": "To set budgets for the quantity and size of page resources, add a budget.json file.",
  "score": null,
  "scoreDisplayMode": "informative",
  "details": {
    "type": "table",
    "headings": [
      {
        "key": "label",
        "itemType": "text",
        "text": "Resource Type"
      },
      {
        "key": "count",
        "itemType": "numeric",
        "text": "Requests"
      },
      {
        "key": "size",
        "itemType": "bytes",
        "text": "File Size"
      },
      {
        "key": "countOverBudget",
        "itemType": "text",
        "text": ""
      },
      {
        "key": "sizeOverBudget",
        "itemType": "bytes",
        "text": "Over Budget"
      }
    ],
    "items": [
      {
        "label": "Script",
        "count": 2,
        "size": 444321,
        "sizeOverBudget": 316321
      },
      {
        "label": "Total",
        "count": 21,
        "size": 650255,
        "sizeOverBudget": 138255
      },
      {
        "label": "Image",
        "count": 6,
        "size": 80392
      },
      {
        "label": "Third-party",
        "count": 13,
        "size": 112138,
        "countOverBudget": "13 requests"
      }
    ]
  }
}
```

See the following pages to learn more about performance budgets:

* [Start Performance Budgeting](https://addyosmani.com/blog/performance-budgets/){: .external }
* [Performance budgets 101](https://web.dev/performance-budgets-101/){: .external }
* [Your first performance budget](https://web.dev/your-first-performance-budget/){: .external }
* [Incorporate performance budgets into your build process](https://web.dev/incorporate-performance-budgets-into-your-build-tools/){: .external }

## Setup {: #setup }

Follow the instructions below to configure Lighthouse to report when you're over budget. As mentioned
before, this is only possible when running Lighthouse from the command line. The **Keep Request Counts
And File Sizes Small** audit is always available in all Lighthouse runtime environments and requires no configuration.

### Define a budget file {: #define }

By convention the budget file is usually called `budget.json` but you can call it whatever you like.
The example below sets the following budgets:

* 125 kilobytes for all scripts
* 50 kilobytes for all stylesheets
* 100 network requests total

```json
[
  {
    "path": "/",
    "resourceSizes": [
      {
        "resourceType": "script",
        "budget": 125
      },
      {
        "resourceType": "stylesheet",
        "budget": 50
      }
    ],
    "resourceCounts": [
      {
        "resourceType": "total",
        "budget": 100
      }
    ]
  }
]
```

The table below describes each of the `budget.json` properties.

<table>
  <tbody>
    <tr>
      <th>Name</th>
      <th>Type</th>
      <th>Valid Value(s)</th>
      <th>Description</th>
    </tr>
    <tr>
      <td><code>path</code></td>
      <td>String</td>
      <td>
        A path pattern adhering to the
        <a href="/search/reference/robots_txt#url-matching-based-on-path-values">robots.txt format</a>.
      </td>
      <td>
        The <code>resourceSizes</code> and <code>resourceCounts</code> budgets apply to all
        pages matching this pattern.
      </td>
    </tr>
    <tr>
      <td><code>resourceSizes</code></td>
      <td>Array</td>
      <td>
        An array of objects. Each object must have a <code>resourceType</code> and <code>budget</code> property.
      </td>
      <td>
        The <code>budget</code> value specifies the total size limit in kilobytes for the category that's specified
        in <code>resourceType</code>.
      </td>
    </tr>
    <tr>
      <td><code>resourceCounts</code></td>
      <td>Array</td>
      <td>
        An array of objects. Each object must have a <code>resourceType</code> and <code>budget</code> property.
      </td>
      <td>
        The <code>budget</code> value specifies the total resource count limit for the category that's specified
        in <code>resourceType</code>.
      </td>
    </tr>
    <tr>
      <td><code>resourceType</code></td>
      <td>String</td>
      <td>
        <code>total</code>, <code>document</code>, <code>script</code>, <code>stylesheet</code>, <code>image</code>,
        <code>media</code>, <code>font</code>, <code>other</code>, or <code>third-party</code>.
      </td>
      <td>
        <code>total</code> measures all page resources. <code>document</code> measures HTML document requests.
        <code>other</code> includes XHR or Fetch requests, and data transfers over WebSocket connections.
        <code>third-party</code> measures all resources from third-party domains.
      </td>
    </tr>
    <tr>
      <td><code>budget</code></td>
      <td>Integer</td>
      <td>Any positive integer.</td>
      <td>The file size limit or resource count limit.</td>
    </tr>
  </tbody>
</table>

### Pass the budget file as a flag {: #flags }

When running Lighthouse from the command line, pass the `--budgetPath` flag followed by the path to your
budget file in order to calculate whenever a category is over budget.

    lighthouse https://youtube.com --budgetPath=./simple-budget.json

Pass the `--output=json` flag followed by `--output-path=./report.json` to save your report results as JSON
in the current working directory.

    lighthouse https://youtube.com --budgetPath ./simple-budget.json --output=json --output-path=report.json

If you were to assign your JSON report results to a variable named `report` you could access your
**Keep Request Counts And File Sizes Small** and **Performance Budgets** data from 
`report.audits['resource-summary']` and `report.audits['resource-budget']`, respectively.

## Recommendations {: #recommendations }

Explore the related audits and guides below to learn how to stay within your budget for each `resourceType`
category.

* `document`
    * [Uses An Excessive DOM Size](/web/tools/lighthouse/audits/dom-size)
    * [Enable Text Compression](/web/tools/lighthouse/audits/text-compression)
* `script`
    * [Reduce JavaScript payloads with code-splitting](https://web.dev/reduce-javascript-payloads-with-code-splitting/)
    * [Remove unused code](https://web.dev/remove-unused-code/)
    * [Minify and compress network payloads](https://web.dev/reduce-network-payloads-using-text-compression/)
    * [Enable Text Compression](/web/tools/lighthouse/audits/text-compression)
    * [JavaScript Bootup Time Is Too High](/web/tools/lighthouse/audits/bootup)
* `stylesheet`
    * [Defer non-critical CSS](https://web.dev/defer-non-critical-css/)
    * [Enable Text Compression](/web/tools/lighthouse/audits/text-compression)
    * [Minify CSS](/web/tools/lighthouse/audits/minify-css)
* `image`
    * [Use Imagemin to compress images](https://web.dev/use-imagemin-to-compress-images/)
    * [Replace animated GIFs with video](https://web.dev/replace-gifs-with-videos/)
    * [Use lazysizes to lazyload images](https://web.dev/use-lazysizes-to-lazyload-images/)
    * [Serve responsive images](https://web.dev/serve-responsive-images/)
    * [Serve images with correct dimensions](https://web.dev/serve-images-with-correct-dimensions/)
    * [Use WebP images](https://web.dev/serve-images-webp/)
    * [Optimize Images](/web/tools/lighthouse/audits/optimize-images)
* `media`
    * [Replace animated GIFs with video](https://web.dev/replace-gifs-with-videos/)

## More information {: #extra }

[src]: https://github.com/GoogleChrome/lighthouse/blob/master/lighthouse-core/audits/resource-budget.js

Sources:

* [Audit source][src]{: .external }

## Feedback {: #feedback }

{% include "web/_shared/helpful.html" %}
