# Allocations

Data visualisation hosted at: https://cbpf.data.unocha.org/#allocation_heading

## Introduction

### Purpose

**Allocations** is a data visualization that shows the values allocated for each fund in the selected period.

The chart also shows a breakdown of the allocated amount in the selection by partner type (National NGO, International NGO, UN Agency, Red Cross/Red Crescent Society).

### Data encoding

This data visualization actually contains two visual encodings: on the left hand side the amount allocated by fund is displayed as a lollipop chart, where the length of the lollipop chart encodes the allocated value. On the right hand side a breakdown by partner type is depicted in a parallel coordinates chart, where the vertical position of each circle depicts the amount allocated by partner type.

### Interactivity

1. At the top right corner of the dataviz there are five buttons:

    - **Share**: copies a link with all the current selections to the clipboard. Use that link to go to the Bookmark page;
    - **Play**: Changes the year at a regular interval, allowing the user to visualise changes along time.
    - **Image**: downloads a snapshot of the chart, as a .png file or as a .pdf file. You can also right-click anywhere in the chart to download a snapshot containing the tooltip;
    - **Csv**: downloads the data as a .csv file;
    - **Help**: shows an annotated layer with tips about how to use and how to understand the chart.

2. The year buttons select the year depicted in the dataviz. Multiple years can be selected by double-clicking or pressing ALT while clicking.

3. The buttons for partner types allows the user selecting a specific partner type, or all partners. Multiple selection is not available for partner types.

4. At the bottom os the chart, a _Net funding_ checkbox allows the user to visualize the breakdown including sub-implementing partners. For a better description of implementing and sub-implementing partners, please check the [Allocation flow](./Allocation-flow.md) chart.

5. Next to the _Net funding_ checkbox, a _Show total_ checkbox allows the user to visualize the breakdown by partner type for all funds, regardless the user selection.

## Technical aspects

### Data attributes

The code retrieves data attributes in a `<div>` with `id="d3chartcontainerpbialp"`. These data attributes are:

-   **`data-title`**: sets the title of the chart. If left empty the chart title defaults to _Allocations by Organization Type_.

-   **`data-year`**: defines the year depicted by the data visualisation when the page loads. The value has to be a string containing the year with century as a decimal number, such as:

    `"2018"`

    If the provided value is not a valid number the datavis will default to the current year. For the accepted values for the years please refer to the data API.

    Multiple years are allowed. In this case, the values have to be separated by comma, for instance:

    `"2016, 2017, 2018"`

    This value defines only the selected year when the page loads: the user can easily change the selected year by clicking the corresponding buttons. Also, the user can select more than one year.

-   **`data-selectedcbpfs`**: defines the selected fund when the page loads. For not selecting any country set the value to `"none"`, or just leave it empty:

    `""`

    For individual countries set the value accordingly, such as:

    `"Yemen"`.

    For more than one country separate the values with commas, such as:

    `"Yemen, Sudan, Iraq"`.

-   **`data-partner`**: defines the partner type depicted by the data visualisation when the page loads. The value has to be a string. Accepted values:

    -   `"total"` or `"all"`: shows all partners.
    -   `"International NGO"`: shows only the allocations for International NGOs.
    -   `"National NGO"`: shows only the allocations for National NGOs.
    -   `"Red Cross/Crescent Movement"` or `"Others"`: shows only the allocations for Red Cross/Crescent Movement.
    -   `"UN Agency"`: shows only the allocations for UN Agencies.

If the value is not an accepted value, it defaults to `"total"`.

This value defines only the selected partner when the page loads: the user can easily change the selected partner by clicking the corresponding buttons.

-   **`data-showaverage`**: defines if the average line is shown in the parallel coordinates chart. Accepted values:

    -   `"true"`: the average line is shown.
    -   `"false"`: the average line is not shown.

If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

-   **`data-netfunding`**: Shows Net Funding. Accepted values:

    -   `"true"`: shows Net Funding by default when the page loads.
    -   `"false"`: does not show Net Funding.

The user can easily change this selection by clicking the "Net Funding" checkbox.

If the value is neither `"true"` nor `"false"`, it defaults to `"false"`.

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
-   https://cbpfgms.github.io/css/d3chartstylespbialp.css: contains styles for this dataviz.
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

-   Launched allocations data: https://cbpfapi.unocha.org/vo2/odata/AllocationTypes?PoolfundCodeAbbrv=&$format=csv
-   Allocations data: https://cbpfapi.unocha.org/vo2/odata/AllocationBudgetTotalsByYearAndFund?&ShowAllPooledFunds=1&FundingType=3&$format=csv

#### Master Tables:

-   Regional funds: https://cbpfgms.github.io/pfbi-data/mst/MstRhpf.json

## Notes

This dataviz can be embedded in any page, the code automatically fetches all data, master tables, libraries and style sheets needed.
Just copy/paste the following snippet:

`<div id="d3chartcontainerpbialp" data-title="Allocations by Organization Type" data-year="2024" data-minpercentage="3" data-partner="total" data-showaverage="true" data-selectedcbpfs="none" data-netfunding="true" data-responsive="true" data-lazyload="true" data-showhelp="false" data-showlink="true"></div><script type="text/javascript" src="https://cbpfgms.github.io/pbialp/src/d3chartpbialp.js"></script>`
