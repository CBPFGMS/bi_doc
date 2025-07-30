# Allocation flow

Data visualization hosted at: https://cbpf.data.unocha.org/#netfunding_heading

Data visualization in the staging site: https://cbpfgms.github.io/cbpf-bi-stag/#netfunding_heading

## Introduction

### Purpose

**Allocation flow** is a data visualization that shows the flow of allocated values from the funds to the implementing partners, or direct partners, which are the direct recipients from the funds, and to sub-implementing partners, which receive funds from direct partners.

The implementing partners, or direct partners, may or may not transfer part of their funds to sub-implementing partners, or indirect partners. These sub-implementing partners can be the same or a different partner type (National NGO, International NGO, UN Agency, Red Cross/Red Crescent Society).

### Data encoding

This charts uses a sankey diagram for depicting both the flow from funds to the implementing partners, that sit in the middle of the chart, and from these implementing partners to the sub-implementing partners (if any), that sit on the right hand side of the chart. The width of each ribbon encodes the total donated/allocated.

As the funds may not be transferred to sub-implementing partners, for easiest comparison, the amount kept by the implementing partners is repeated on the right hand side of the sankey diagram. On that area, a color encoding is used: gray for direct partners, red for sub-implementing partners.

### Interactivity

1. At the top right corner of the dataviz there are five buttons:

    - **Share**: copies a link with all the current selections to the clipboard. Use that link to go to the Bookmark page;
    - **Play**: Changes the year at a regular interval, allowing the user to visualise changes along time.
    - **Image**: downloads a snapshot of the chart, as a .png file or as a .pdf file. You can also right-click anywhere in the chart to download a snapshot containing the tooltip;
    - **Csv**: downloads the data as a .csv file;
    - **Help**: shows an annotated layer with tips about how to use and how to understand the chart.

2. The _Select CBPF_ checkboxes allow filtering by fund.

3. The year buttons select the year depicted in the dataviz. Multiple years can be selected by double-clicking or pressing ALT while clicking.

4. The user can aggregate the values on the right hand side of the sankey diagram by partner level, meaning that direct and indirect partners will sit in their respective groups, or by partner type, meaning that each partner type (National NGO, International NGO, UN Agency, Red Cross/Red Crescent Society) will show a breakdown by implementing/sub-implementing partner.

## Technical aspects

### Source code

-   Production: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbinad/src/d3chartpbinad.js

-   Staging: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbinad/src/d3chartpbinad-stg.js

### Data attributes

The code retrieves data attributes in a `<div>` with `id="d3chartcontainerpbinad"`. These data attributes are:

-   **`data-title`**: sets the title of the chart. If left empty the chart title defaults to _Allocation flow (net funding)_.

-   **`data-year`**: defines the year depicted by the data visualization when the page loads. The value has to be a string containing the year with century as a decimal number, such as:

    `"2018"`

    If the provided value is not a valid number the datavis will default to the current year. For the accepted values for the years please refer to the data API.

    Multiple years are allowed. In this case, the values have to be separated by comma, for instance:

    `"2016, 2017, 2018"`

    This value defines only the selected year when the page loads: the user can easily change the selected year by clicking the corresponding buttons. Also, the user can select more than one year.

-   **`data-cbpf`**: defines the selected fund when the page loads. For not selecting any country set the value to `"none"`, or just leave it empty:

    `""`

    For individual countries set the value accordingly, such as:

    `"Yemen"`.

    For more than one country separate the values with commas, such as:

    `"Yemen, Sudan, Iraq"`.

-   **`data-aggregate`**: defines how the third level in the sankey diagram will be aggregated. Accepted values:

    -   `"level"`: aggregates the values by level, that is, implementing vs sub-implementing partners.
    -   `"type"`: aggregates the values by partner type, each one with a breakdown for partner level.

    If the value is neither `"true"` nor `"false"`, it defaults to `"type"`.

-   **`data-minpercen`tage**: defines the minimum value for under approval allocations percentage. If the under approval allocations percentage is above this minimum, _Total USD planned_ values are used for the visual, otherwise _Total Approved Budget_ values are used (refer to the data APIs).

-   **`data-responsive`**: defines if the SVG stretches to the width of the containing element. Accepted values:

    -   `"true"`: the SVG will stretch to the width of the element containing the code snippet.
    -   `"false"`: the SVG will be created with a fixed size, which is 900px width.

-   **`data-lazyload`**: defines if the animation starts when the SVG is visible. Accepted values:

    -   `"true"`: the animation starts only when the SVG is visible in the browser window.
    -   `"false"`: the animation starts when the page is loaded, regardless if the SVG is visible.

    If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

-   \*_`data-showhelp_`\*: shows the annotations explaining how to use the data visualization. Accepted values:

    -   `"true":` annotations shown when the page loads.
    -   `"false":` annotations not shown when the page loads. The user can easily show the annotations by clicking the "help" button.

    If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

-   \*_`data-showlink_`\*: shows the "for more information" link in the footer. Accepted values:

    -   `"true":` shows the link.
    -   `"false":` doesn't show the link.

    If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

### Language

Javascript, without JSDoc annotations

### CSS

The code loads three CSS files:

-   https://cbpfgms.github.io/css/d3chartstyles.css: contains general styles, used by other visuals.
-   https://cbpfgms.github.io/css/d3chartstylespbinad.css: contains styles for this dataviz.
-   https://use.fontawesome.com/releases/v5.6.3/css/all.css: FontAwesome styles.

### Libraries

-   d3.js, version 5.16. Source: https://cdnjs.cloudflare.com/ajax/libs/d3/5.16.0/d3.min.js
-   d3-sankey, version 0.12. Source: https://cdnjs.cloudflare.com/ajax/libs/d3-sankey/0.12.3/d3-sankey.js
-   html2canvas, version 1.0.0-rc.1. Source: https://cbpfgms.github.io/libraries/html2canvas.min.js
-   jsPdf, version 1.5. Source: https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.5.3/jspdf.min.js
-   Polyfills for old browsers: https://cdn.jsdelivr.net/npm/@ungap/url-search-params@0.1.2/min.min.js,
    https://cdn.jsdelivr.net/npm/promise-polyfill@7/dist/polyfill.min.js,
    https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.4/fetch.min.js;

### APIs

#### Data APIs:

-   Allocations data: https://cbpfapi.unocha.org/vo2/odata/AllocationTypes?PoolfundCodeAbbrv=&$format=csv
-   Implementing vs sub-implementing data: https://cbpfapi.unocha.org/vo2/odata/AllocationFlowByOrgType?PoolfundCodeAbbrv=&$format=csv

#### Master Tables:

-   Funds: https://cbpfapi.unocha.org/vo2/odata/MstPooledFund?$format=csv
-   Partners: https://cbpfapi.unocha.org/vo2/odata/MstOrgType?$format=csv
-   Sub-implementing partners: https://cbpfapi.unocha.org/vo2/odata/SubIPType?$format=csv

### Miscellaneous

#### Data filters

When loaded at the [production](https://cbpf.data.unocha.org/) or [staging](https://cbpfgms.github.io/cbpf-bi-stag/) sites, the code for this data visualization doesn't fetch the data from the APIs described above directly. Instead, on those sites, a different script fetches the data, checks for the types in the objects and then passes the data to the code. This filtering script is [documented here](../../Utils/CBPF-BI-filters.md).

_Note_: any error in the data types will be logged **only on the staging site** (check the browser's console). On the production site no error will be logged. On both sites, the object with the error is simply ignored.

## Notes

This dataviz can be embedded in any page, the code automatically fetches all data, master tables, libraries and style sheets needed.
Just copy/paste the following snippet:

`<div id="d3chartcontainerpbinad" data-title="Allocation flow (net funding)" data-year="2025" data-cbpf="all" data-aggregate="type" data-minpercentage="3" data-showhelp="false" data-showlink="true" data-responsive="true" data-lazyload="true"></div><script type="text/javascript" src="https://cbpfgms.github.io/pbinad/src/d3chartpbinad.js"></script>`
