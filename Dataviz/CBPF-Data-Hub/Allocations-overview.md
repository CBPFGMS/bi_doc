# Allocations overview

Data visualisation hosted at: https://cbpf.data.unocha.org/allocations-overview

Data visualization in the staging site: https://cbpfgms.github.io/cbpf-bi-stag/allocations-overview

## Introduction

### Purpose

**Allocations overview** is a complex data visualization that shows how the allocations for each fund in their actual geographic positions, in two different ways: either as circles with different areas or as markers with different colors.

Geographically, the dataviz allows the user to see data for each fund or, more detailed, data for the different administration levels within that fund.

The data visualization also allows the user to see details for the projects of the selected fund/admin level, or even all the projects for the current selection.

### Data encoding

This data visualization consists in a dot map, with two different encodings: when _size_ is selected the data points are displayed as circles, where the area (not the radius) of each circle encodes total values. When _color_ is selected the data points are displayed as icons, with the color gradient of the icon encoding total values.

### Interactivity

1. At the top right corner of the dataviz there are four buttons:

    - **Share**: copies a link with all the current selections to the clipboard. Use that link to go to the Bookmark page;
    - **Image**: downloads a snapshot of the chart, as a .png file or as a .pdf file. You can also right-click anywhere in the chart to download a snapshot containing the tooltip;
    - **Csv**: downloads the data as a .csv file;
    - **Help**: shows an annotated layer with tips about how to use and how to understand the chart.

2. A series of dropdown menus with checkboxes allow the filtering of the data by:

    - Year
    - Fund
    - Partner type
    - Sector
    - Allocation type
    - Location: from _global_ to _admin level 6_

3. In the map itself, _+_ and _-_ buttons allow zoom, but the user can also zoom with the mouse over any part of the map.

4. Next to the legend, two buttons control the data encoding:

    - Size: the circles' area depict allocation values.
    - Color: the markers' colors depict allocation values

5. The _Show all projects_ button creates a list of projects for the current selection below the chart area.

6. Hovering over any data point brings detailed information, including a breakdown by beneficiary type. In the tooltip itself, the _Show projects_ button shows projects for the corresponding data point.

## Technical aspects

### Source code

-   Production: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbimap/src/d3chartpbimap.js

-   Staging: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbimap/src/d3chartpbimap-stg.js

### Data attributes

The code retrieves data attributes in a `<div>` with `id="d3chartcontainerpbimap"`. These data attributes are:

-   **`data-title`**: sets the title of the chart. If left empty the chart title defaults to _Gender with Age Marker (GAM)_.

-   **`data-year`**: defines the year depicted by the data visualisation when the page loads. The value has to be a string containing the year with century as a decimal number, such as:

    `"2018"`

    If the provided value is not a valid number the datavis will default to the current year. For the accepted values for the years please refer to the data API.

    Multiple years are allowed. In this case, the values have to be separated by comma, for instance:

    `"2016, 2017, 2018"`

    This value defines only the selected year when the page loads: the user can easily change the selected year by clicking the corresponding buttons. Also, the user can select more than one year.

-   **`data-cbpf`**: defines the selected CBPFs when the page loads. For showing all CBPFs, set the value to `"all"`. For individual CBPFs set the value accordingly, such as:

    `"Yemen"`.

    For more than one CBPF separate the values with commas, such as:

    `"Yemen, Sudan, Iraq"`.

    For the accepted values, please refer to the data API.

    If the value is not a valid one it defaults to `"all"`.

-   **`data-partner`**: defines the selected partners when the page loads. For showing all partners, set the value to `"all"`. Accepted values:

    -   `"National NGO"`
    -   `"International NGO"`
    -   `"UN Agency"`
    -   `"Others"`

    For more than one partner the values must be separated with commas. If the value is not a valid one it defaults to `"all"`.

-   **`data-cluster`**: defines the selected cluster when the page loads. For showing all clusters, set the value to `"all"`. Accepted values:

    -   `"Camp Coordination / Management"`
    -   `"Early Recovery"`
    -   `"Education"`
    -   `"Emergency Shelter and NFI"`
    -   `"Emergency Telecommunications"`
    -   `"Food Security"`
    -   `"Health"`
    -   `"Logistics"`
    -   `"Nutrition"`
    -   `"Protection"`
    -   `"Water, Sanitation and Hygiene"`
    -   `"Coordination and Support Services"`
    -   `"Multi-Cluster"`

    For more than one cluster the values must be separated with commas. If the value is not a valid one it defaults to `"all"`.

-   **`data-adminlevel`**: defines the selected location level when the page loads. The value must be a number, from `1` to `6`. For showing the global level, set the value to `"all"` or `0`. If the value is not a valid one it defaults to `"all"`.

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
-   https://cbpfgms.github.io/css/d3chartstylespbimap.css: contains styles for this dataviz.
-   https://use.fontawesome.com/releases/v5.6.3/css/all.css: FontAwesome styles.

### Libraries

-   d3.js, version 5.16. Source: https://cdnjs.cloudflare.com/ajax/libs/d3/5.16.0/d3.min.js
-   Leaflet, version 1.4.0. Source: https://cbpfgms.github.io/libraries/leaflet.js
-   Leaflet CSS. SOurce: https://cbpfgms.github.io/libraries/leaflet.css
-   html2canvas, version 1.0.0-rc.1. Source: https://cbpfgms.github.io/libraries/html2canvas.min.js
-   jsPdf, version 1.5. Source: https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.5.3/jspdf.min.js
-   Polyfills for old browsers: https://cdn.jsdelivr.net/npm/@ungap/url-search-params@0.1.2/min.min.js,
    https://cdn.jsdelivr.net/npm/promise-polyfill@7/dist/polyfill.min.js,
    https://cdnjs.cloudflare.com/ajax/libs/fetch/2.0.4/fetch.min.js;

### APIs

#### Data APIs:

-   Project summary: https://cbpfapi.unocha.org/vo2/odata/ProjectSummaryV2?

-   Project summary aggregated: https://cbpfapi.unocha.org/vo2/odata/ProjectSummaryAggV2?

-   Launched allocations: https://cbpfapi.unocha.org/vo2/odata/AllocationTypes?PoolfundCodeAbbrv=&$format=csv

#### Master Tables:

-   Funds: https://cbpfapi.unocha.org/vo2/odata/MstPooledFund?$format=csv

-   Sectors: https://cbpfapi.unocha.org/vo2/odata/MstClusters?$format=csv

-   Partners: https://cbpfapi.unocha.org/vo2/odata/MstOrgType?$format=csv

-   Allocation sources: https://cbpfapi.unocha.org/vo2/odata/MstAllocationSource?$format=csv

## Notes

This dataviz can be embedded in any page, the code automatically fetches all data, master tables, libraries and style sheets needed.
Just copy/paste the following snippet:

`<div id="d3chartcontainerpbimap" data-title="Allocations map" data-year="2025" data-cbpf="all" data-partner="all" data-cluster="all" data-adminlevel="all" data-showhelp="false" data-showlink="true" data-responsive="true" data-lazyload="true"></div><script type="text/javascript" src="https://cbpfgms.github.io/pbimap/src/d3chartpbimap.js"></script>`
