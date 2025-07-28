# CBPF by year

Data visualisation hosted at: https://cbpf.data.unocha.org/?#byyear_heading

Data visualization in the staging site: https://cbpfgms.github.io/cbpf-bi-stag/#byyear_heading

## Introduction

### Purpose

**CBPF by year** is a mixed chart that shows both contributions and allocations, having as the central point those two totals. The contributions are divided by donors, and the allocations are divided on the first level in Standard and Reserve. For both allocation sources, the second level divides those values by funds.

This dataviz allows a the user to quickly compare the contributions and allocations figures, as well as comparing the breakdown for donors (contributions) and allocation sources plus funds (allocations).

### Data encoding

This charts uses a sankey diagram for depicting the contribution and allocation values. The width of each ribbon encodes the total donated/allocated.

### Interactivity

1. At the top right corner of the dataviz there are five buttons:

    - **Share**: copies a link with all the current selections to the clipboard. Use that link to go to the Bookmark page;
    - **Play**: Changes the year at a regular interval, allowing the user to visualise changes along time.
    - **Image**: downloads a snapshot of the chart, as a .png file or as a .pdf file. You can also right-click anywhere in the chart to download a snapshot containing the tooltip;
    - **Csv**: downloads the data as a .csv file;
    - **Help**: shows an annotated layer with tips about how to use and how to understand the chart.

2. The _Select CBPF_ checkboxes allow filtering by fund.

3. The year buttons select the year depicted in the dataviz. Multiple years can be selected by double-clicking or pressing ALT while clicking.

## Technical aspects

### Source code

-   Production: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/cbsank/src/d3chartcbsank.js

-   Staging: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/cbsank/src/d3chartcbsank-stg.js

### Data attributes

The code retrieves data attributes in a `<div>` with `id="d3chartcontainercbsank"`. These data attributes are:

-   **`data-title`**: sets the title of the chart. If left empty the chart title defaults to _CBPF By Year_.

-   **`data-year`**: defines the year depicted by the data visualisation when the page loads. The value has to be a string containing the year with century as a decimal number, such as:

    `"2018"`

    If the provided value is not a valid number the datavis will default to the current year. For the accepted values for the years please refer to the data API.

    Multiple years are allowed. In this case, the values have to be separated by comma, for instance:

    `"2016, 2017, 2018"`

    This value defines only the selected year when the page loads: the user can easily change the selected year by clicking the corresponding buttons. Also, the user can select more than one year.

-   **`data-fund`**: defines the selected fund when the page loads. For not selecting any country set the value to `"none"`, or just leave it empty:

    `""`

    For individual countries set the value accordingly, such as:

    `"Yemen"`.

    For more than one country separate the values with commas, such as:

    `"Yemen, Sudan, Iraq"`.

-   **`data-minpercentage`**: defines the minimum value for under approval allocations percentage. If the under approval allocations percentage is above this minimum, _Total USD planned_ values are used for the visual, otherwise _Total Approved Budget_ values are used (refer to the data APIs).

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
-   https://cbpfgms.github.io/css/d3chartstylescbsank.css: contains styles for this dataviz.
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

-   Contributions data: https://cbpfgms.github.io/pfbi-data/contributionSummarySankey.csv
-   Allocations data: https://cbpfapi.unocha.org/vo2/odata/AllocationTypes?PoolfundCodeAbbrv=&$format=csv

#### Master Tables:

-   Donors: https://cbpfgms.github.io/pfbi-data/mst/MstDonor.json
-   Funds: https://cbpfgms.github.io/pfbi-data/mst/MstCountry.json
-   Allocation sources: https://cbpfgms.github.io/pfbi-data/mst/MstAllocation.json

## Notes

This dataviz can be embedded in any page, the code automatically fetches all data, master tables, libraries and style sheets needed.
Just copy/paste the following snippet:

`<div id="d3chartcontainercbsank" data-title="" data-year="2025" data-fund="" data-minpercentage="3" data-responsive="true" data-lazyload="true"></div><script type="text/javascript" src="https://cbpfgms.github.io/cbsank/src/d3chartcbsank.js"></script>`
