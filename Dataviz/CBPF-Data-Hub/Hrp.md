# CBPF vs HRP

Data visualisation hosted at: https://cbpf.data.unocha.org/allocations-overview

Data visualization in the staging site: https://cbpfgms.github.io/cbpf-bi-stag/allocations-overview

## Introduction

### Purpose

**CBPF vs HRP** is a data visualization that shows how targeted and actual CBPF funds compare against HRP requirements and HRP funding. The chart has two components, one grouped bar chart for regular funds and, below it, a similar chart for non-HRP funds (if any).

### Data encoding

This data visualization consists in a mix of grouped bar charts (for each fund) and stacked bar chart (for HRP requirements and funding), as well as donut charts for depicting percentages.

### Interactivity

1. At the top right corner of the dataviz there are five buttons:

    - **Share**: copies a link with all the current selections to the clipboard. Use that link to go to the Bookmark page;
    - **Play**: Changes the year at a regular interval, allowing the user to visualise changes along time.
    - **Image**: downloads a snapshot of the chart, as a .png file or as a .pdf file. You can also right-click anywhere in the chart to download a snapshot containing the tooltip;
    - **Csv**: downloads the data as a .csv file;
    - **Help**: shows an annotated layer with tips about how to use and how to understand the chart.

2. The year buttons select the year depicted in the dataviz. Multiple years can be selected by double-clicking or pressing ALT while clicking.

3. Below the top figures, a series of radio buttons allow sorting the grouped bar chart by:

    - CBPF Funding
    - CBPF Percentage of the Target Achieved
    - HRP Funding
    - HRP Requirements
    - Alphabetically

4. Hovering over any grouped bars brings a tooltip with detailed information.

## Technical aspects

### Source code

-   Production: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbihrp/src/d3chartpbihrp.js

-   Staging: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbihrp/src/d3chartpbihrp-stg.js

### Data attributes

The code retrieves data attributes in a `<div>` with `id="d3chartcontainerpbihrp"`. These data attributes are:

-   **`data-title`**: sets the title of the chart. If left empty the chart title defaults to _Gender with Age Marker (GAM)_.

-   **`data-year`**: defines the year depicted by the data visualisation when the page loads. The value has to be a string containing the year with century as a decimal number, such as:

    `"2018"`

    If the provided value is not a valid number the datavis will default to the current year. For the accepted values for the years please refer to the data API.

    Multiple years are allowed. In this case, the values have to be separated by comma, for instance:

    `"2016, 2017, 2018"`

    This value defines only the selected year when the page loads: the user can easily change the selected year by clicking the corresponding buttons. Also, the user can select more than one year.

-   **`data-sortby`**: defines the criterion for sorting the bars (descending) when the page loads. The value has to be a string. Accepted values:

    -   `"CBPF Funding"`: sorts the bars by CBPF Funding.
    -   `"cbpfpercentage"`: sorts the bars by the percentage of the target achieved.
    -   `"hrpfunding"`: sorts the bars by HRP Funding.
    -   `"hrprequirements"`: sorts the bars by HRP Requirements.
    -   `"cbpftarget"`: sorts the bars by CBPF Target.
    -   `"alphabetically"`: sorts the bars alphabetically.

    If the value is not an accepted value, it defaults to `"CBPF Funding"`.

-   **`data-responsive`**: defines if the SVG stretches to the width of the containing element. Accepted values:

    -   `"true"`: the SVG will stretch to the width of the element containing the code snippet.
    -   `"false"`: the SVG will be created with a fixed size, which is 900px width.

-   **`data-lazyload`**: defines if the animation starts when the SVG is visible. Accepted values:

    -   `"true"`: the animation starts only when the SVG is visible in the browser window.
    -   `"false"`: the animation starts when the page is loaded, regardless if the SVG is visible.

    If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

-   **`data-showhelp`**: shows the annotations explaining how to use the data visualisation. Accepted values:

    -   `"true":` annotations shown when the page loads.
    -   `"false":` annotations not shown when the page loads. The user can easily show the annotations by clicking the "help" button.

    If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

-   **`data-showlink`**: shows the "for more information" link in the footer. Accepted values:

    -   `"true":` shows the link.
    -   `"false":` doesn't show the link.

    If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

### Language

Javascript, without JSDoc annotations

### CSS

The code loads three CSS files:

-   https://cbpfgms.github.io/css/d3chartstyles.css: contains general styles, used by other visuals.
-   https://cbpfgms.github.io/css/d3chartstylespbihrp.css: contains styles for this dataviz.
-   https://use.fontawesome.com/releases/v5.6.3/css/all.css: FontAwesome styles.

### Libraries

-   d3.js, version 5.16. Source: https://cdnjs.cloudflare.com/ajax/libs/d3/5.16.0/d3.min.js
-   html2canvas, version 1.0.0-rc.1. Source: https://cbpfgms.github.io/libraries/html2canvas.min.js
-   jsPdf, version 1.5. Source: https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.5.3/jspdf.min.js
-   Polyfills for old browsers: https://cdn.jsdelivr.net/npm/@ungap/url-search-params@0.1.2/min.min.js,
    https://cdn.jsdelivr.net/npm/promise-polyfill@7/dist/polyfill.min.js,
    https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.4/fetch.min.js;

### APIs

#### Data APIs:

-   HRP data: https://cbpfapi.unocha.org/vo2/odata/HRPCBPFFundingSummary?PoolfundCodeAbbrv=&$format=csv

#### Master Tables:

-   Regional funds: https://cbpfgms.github.io/pfbi-data/mst/MstRhpf.json

## Notes

This dataviz can be embedded in any page, the code automatically fetches all data, master tables, libraries and style sheets needed.
Just copy/paste the following snippet:

`<div id="d3chartcontainerpbihrp" data-title="CBPF Target vs HRP" data-year="2025" data-sortby="CBPF Funding" data-showhelp="false" data-showlink="true" data-responsive="true" data-lazyload="true"></div><script type="text/javascript" src="https://cbpfgms.github.io/pbihrp/src/d3chartpbihrp.js"></script>`
