# CERF by year

Data visualization hosted at: https://cerf.data.unocha.org/#

Data visualization in the staging site: https://cbpfgms.github.io/cerf-bi-stag/#

## Introduction

### Purpose

**CERF by year** is a mixed chart that shows both contributions and allocations, having as the central point those two totals. The contributions are divided by donors, and the allocations are divided on the first level by their UN Agencies. For all UN Agencies, the second level divides those values by sector.

This dataviz allows a the user to quickly compare the contributions and allocations figures, as well as comparing the breakdown for donors (contributions) and allocation partners plus sectors (allocations).

### Data encoding

This charts uses a sankey diagram for depicting the contribution and allocation values. The width of each ribbon encodes the total donated/allocated.

### Interactivity

1. At the top right corner of the dataviz there are five buttons:

    - **Share**: copies a link with all the current selections to the clipboard. Use that link to go to the Bookmark page;
    - **Image**: downloads a snapshot of the chart, as a .png file or as a .pdf file. You can also right-click anywhere in the chart to download a snapshot containing the tooltip;
    - **Csv**: downloads the data as a .csv file;
    - **Help**: shows an annotated layer with tips about how to use and how to understand the chart.

2. The year buttons select the year depicted in the dataviz. Multiple years can be selected by double-clicking or pressing ALT while clicking.

3. Hovering over any ribbon (links) shows the detailed values, with breakdowns where applicable. Hovering over the nodes (donors, agencies or sectors) highlights the links associated to that node, and displays detailed values.

## Technical aspects

### Source code

-   Production: https://github.com/UN-OCHA/cerfdata-unocha-org/blob/master/charts/sankeydiagram.js

-   Staging: https://github.com/CBPFGMS/cerf-bi-stag/blob/main/charts/sankeydiagram.js

### Data attributes

The code retrieves data attributes in a `<div>` with `id="d3chartcontainercesank"`. These data attributes are:

-   **`data-title`**: sets the title of the chart. If left empty the chart title defaults to _Sankey diagram_.

-   **`data-year`**: defines the year depicted by the data visualization when the page loads. The value has to be a string containing the year with century as a decimal number, such as:

    `"2018"`

    If the provided value is not a valid number the datavis will default to the current year. For the accepted values for the years please refer to the data API.

    Multiple years are allowed. In this case, the values have to be separated by comma, for instance:

    `"2016, 2017, 2018"`

    This value defines only the selected year when the page loads: the user can easily change the selected year by clicking the corresponding buttons. Also, the user can select more than one year.

-   **`data-responsive`**: defines if the SVG stretches to the width of the containing element. Accepted values:

    -   `"true"`: the SVG will stretch to the width of the element containing the code snippet.
    -   `"false"`: the SVG will be created with a fixed size, which is 900px width.

-   **`data-lazyload`**: defines if the animation starts when the SVG is visible. Accepted values:

    -   `"true"`: the animation starts only when the SVG is visible in the browser window.
    -   `"false"`: the animation starts when the page is loaded, regardless if the SVG is visible.

    If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

### Language

Javascript, without JSDoc annotations

### CSS

The code needs these CSS files for proper styling:

- Production site: https://github.com/UN-OCHA/cerfdata-unocha-org/blob/master/assets/css/d3chartstyles.css

- Staging site: https://github.com/CBPFGMS/cerf-bi-stag/blob/master/assets/css/d3chartstyles.css 

It also needs Font Awesome styles: `<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">`

### Libraries

-   d3.js, version 5.16. Source: https://cdnjs.cloudflare.com/ajax/libs/d3/5.16.0/d3.min.js
-   d3-sankey, version 0.12. Source: https://cdnjs.cloudflare.com/ajax/libs/d3-sankey/0.12.3/d3-sankey.js
-   html2canvas, version 1.0.0-rc.1. Source: https://cbpfgms.github.io/libraries/html2canvas.min.js
-   jsPdf, version 1.5. Source: https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.5.3/jspdf.min.js

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
