# Allocations

Data visualization hosted at: https://cerf.data.unocha.org/#Allocations

Data visualization in the staging site: https://cbpfgms.github.io/cerf-bi-stag/#Allocations

## Introduction

### Purpose

**Allocations** is a mixed of a choropleth map and a stacked bar chart. The choropleth map allow the user to see geographic trends and patterns in the allocations around the world, for the selected period and window, while the stacked bar chart allows ranking and comparison of the allocations by UN agency, also for the selected period and window.

### Data encoding

The choropleth map encodes the amount allocation by using a color variation: different shades of blue or yellow (depending on the selected window) indicate the amount allocated.

The stached bar chart is just like a regular bar chart, but when both windows (_Underfunded Emergencies_ and _Rapid Response_) are selected, those windows are stacked one on top of the other, as a single bar.

### Interactivity

1. At the top right corner of the dataviz there are four buttons:

    - **Share**: copies a link with all the current selections to the clipboard. Use that link to go to the Bookmark page;
    - **Play**: Changes the year at a regular interval, allowing the user to visualise changes along time.
    - **Image**: downloads a snapshot of the chart, as a .png file or as a .pdf file. You can also right-click anywhere in the chart to download a snapshot containing the tooltip;
    - **Csv**: downloads the data as a .csv file;

2. The year buttons select the year depicted in the dataviz. Multiple years can be selected by double-clicking or pressing ALT while clicking.

3. Next to the year buttons, three buttons allow the user to filter the data by window:

    - Total (Underfunded Emergencies plus Rapid Response)
    - Underfunded Emergencies
    - Rapid Response

4. Hovering over any country on the map brings a tooltip that displays detailed values, and a breakdown by sector. Hovering over any bar brings a tooltip that displays detailed values, and a breakdown by window.

## Technical aspects

### Source code

-   Production: https://github.com/UN-OCHA/cerfdata-unocha-org/blob/master/charts/allocationsoverview.js

-   Staging: https://github.com/CBPFGMS/cerf-bi-stag/blob/main/charts/allocationsoverview.js

### Data attributes

The code retrieves data attributes in a `<div>` with `id="d3chartcontainerallocover"`. These data attributes are:

-   **`data-title`**: sets the title of the chart. If left empty the chart title defaults to _Allocations_.

-   **`data-year`**: defines the year depicted by the data visualization when the page loads. The value has to be a string containing the year with century as a decimal number, such as:

    `"2018"`

    If the provided value is not a valid number the datavis will default to the current year. For the accepted values for the years please refer to the data API.

    Multiple years are allowed. In this case, the values have to be separated by comma, for instance:

    `"2016, 2017, 2018"`

    This value defines only the selected year when the page loads: the user can easily change the selected year by clicking the corresponding buttons. Also, the user can select more than one year.

-   **`data-cerfallocation`**: defines the allocation window used when the chart first loads. Accepted values:

    -   `Total`: Underfunded Emergencies plus Rapid Response
    -   `Rapid Response`
    -   `Underfunded Emergencies`

    If no value is provided, it defaults to `Total`.

-   **`data-shownames`**: defines if the `show names` checkbox is selected when the chart first loads. Accepted values:

    -   `"true"`: all country names are displayed, even if they overlap.
    -   `"false"`: the names are displayed only if there is no overlap with neighbor countries.

    If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

-   **`data-responsive`**: defines if the SVG stretches to the width of the containing element. Accepted values:

    -   `"true"`: the SVG will stretch to the width of the element containing the code snippet.
    -   `"false"`: the SVG will be created with a fixed size, which is 900px width.

-   **`data-lazyload`**: defines if the animation starts when the SVG is visible. Accepted values:

    -   `"true"`: the animation starts only when the SVG is visible in the browser window.
    -   `"false"`: the animation starts when the page is loaded, regardless if the SVG is visible.

    If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

-   **`data-showhelp_`**: shows the annotations explaining how to use the data visualization. Accepted values:

    -   `"true":` annotations shown when the page loads.
    -   `"false":` annotations not shown when the page loads. The user can easily show the annotations by clicking the "help" button.

    If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

-   **`data-showlink_`**: shows the "for more information" link in the footer. Accepted values:

    -   `"true":` shows the link.
    -   `"false":` doesn't show the link.

    If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

### Language

Javascript, without JSDoc annotations

### CSS

The code needs these CSS files for proper styling:

-   Production site: https://github.com/UN-OCHA/cerfdata-unocha-org/blob/master/assets/css/d3chartstyles.css

-   Staging site: https://github.com/CBPFGMS/cerf-bi-stag/blob/master/assets/css/d3chartstyles.css

It also needs Font Awesome styles: `<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.min.css">`

### Libraries

-   d3.js, version 5.16. Source: https://cdnjs.cloudflare.com/ajax/libs/d3/5.16.0/d3.min.js
-   topojson, version 3.0. Source: https://cdnjs.cloudflare.com/ajax/libs/topojson/3.0.2/topojson.min.js
-   html2canvas, version 1.0.0-rc.1. Source: https://cbpfgms.github.io/libraries/html2canvas.min.js
-   jsPdf, version 1.5. Source: https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.5.3/jspdf.min.js

### APIs

#### Data APIs:

-   Allocations data: https://cbpfgms.github.io/pfbi-data/cerf/cerf_allocationSummary_byorg.csv

#### Master Tables:

-   Funds: https://cbpfgms.github.io/pfbi-data/mst/MstCountry.json
-   Allocation types: https://cbpfgms.github.io/pfbi-data/mst/MstAllocation.json
-   UN Agencies: https://cerfgms-webapi.unocha.org/v1/agency/All.json
-   Organization types: https://cbpfgms.github.io/pfbi-data/mst/MstOrganization.json
-   Sectors: https://cbpfgms.github.io/pfbi-data/mst/MstCluster.json

#### Other

-   World map: https://cbpfgms.github.io/pfbi-data/map/unworldmap.json
