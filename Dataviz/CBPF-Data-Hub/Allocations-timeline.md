# Allocations timeline

Data visualisation hosted at: https://cbpf.data.unocha.org/#allocationtimeline_heading

Data visualization in the staging site: https://cbpfgms.github.io/cbpf-bi-stag/#allocationtimeline_heading

## Introduction

### Purpose

**Allocations timeline** is a data visualization that shows a timeline of allocations for all funds, allowing the user to visualize the start and end of each allocation, and to quickly scan the allocations for a selected period.

The chart also shows a breakdown of planned, ongoing and closed allocations for the selected period.

### Data encoding

This data visualization is a modified Gantt chart, with time in the x axis and the funds as the categorical variable in the y axis. At the top of the Gantt chart there is a panel that allows brushing, where the user can drag the two handles (left and right) to specify the selected period.

### Interactivity

1. At the top right corner of the dataviz there are five buttons:

    - **Share**: copies a link with all the current selections to the clipboard. Use that link to go to the Bookmark page;
    - **Play**: Changes the year at a regular interval, allowing the user to visualise changes along time.
    - **Image**: downloads a snapshot of the chart, as a .png file or as a .pdf file. You can also right-click anywhere in the chart to download a snapshot containing the tooltip;
    - **Csv**: downloads the data as a .csv file;
    - **Help**: shows an annotated layer with tips about how to use and how to understand the chart.

2. The year buttons select the year depicted in the dataviz. Multiple years can be selected by double-clicking or pressing ALT while clicking.

3. The brush selection panel allows the user to specify a custom time period, when the year buttons are not enough. For using the brush panel, drag the handles left or right, highlighting the selected period and changing the chart below it.

4. Hovering over a project displays a tooltip with its general info, and a _Display details_ button shows detailed information for the project below the chart area.

## Technical aspects

### Source code

-   Production: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbiuac/src/d3chartpbiuac.js

-   Staging: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbiuac/src/d3chartpbiuac-stg.js

### Data attributes

The code retrieves data attributes in a `<div>` with `id="d3chartcontainerpbiuac"`. These data attributes are:

-   **`data-title`**: sets the title of the chart. If left empty the chart title defaults to _Allocations Timeline_.

-   **`data-year`**: defines the year depicted by the data visualisation when the page loads. The value has to be a string containing the year with century as a decimal number, such as:

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
-   https://cbpfgms.github.io/css/d3chartstylespbiuac.css: contains styles for this dataviz.
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

-   Allocations data: https://cbpfapi.unocha.org/vo2/odata/AllocationTypes?PoolfundCodeAbbrv=&$format=csv

#### Master Tables:

No master table used.

## Notes

This dataviz can be embedded in any page, the code automatically fetches all data, master tables, libraries and style sheets needed.
Just copy/paste the following snippet:

`<div id="d3chartcontainerpbiuac" data-title="Allocations Timeline" data-year="2025" data-showhelp="false" data-showlink="true" data-responsive="true" data-lazyload="true"></div><script type="text/javascript" src="https://cbpfgms.github.io/pbiuac/src/d3chartpbiuac.js"></script>`
