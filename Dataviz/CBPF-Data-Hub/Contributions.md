# Contributions

Data visualisation hosted at: https://cbpf.data.unocha.org/#contribution_heading

Data visualization in the staging site: https://cbpfgms.github.io/cbpf-bi-stag/#contribution_heading

## Introduction

### Purpose

**Contributions** is a data visualization that shows, for a selected period, the amount donated by all donors, as well as the amount earmarked for all funds. It allows a quick visual ranking and sorting of both donors and funds regarding donations.

### Data encoding

This data visualization consists in two separated lollipop charts, one for the donors on the left hand side and another one for the funds on the right hand side. The length of each lollipop depicts the amount donated/earmarked.

### Interactivity

1. At the top right corner of the dataviz there are five buttons:

    - **Share**: copies a link with all the current selections to the clipboard. Use that link to go to the Bookmark page;
    - **Play**: Changes the year at a regular interval, allowing the user to visualise changes along time.
    - **Image**: downloads a snapshot of the chart, as a .png file or as a .pdf file. You can also right-click anywhere in the chart to download a snapshot containing the tooltip;
    - **Csv**: downloads the data as a .csv file;
    - **Help**: shows an annotated layer with tips about how to use and how to understand the chart.

2. The year buttons select the year depicted in the dataviz. Multiple years can be selected by double-clicking or pressing ALT while clicking.

3. Hovering over a donor on the left hand side lollipop filters the funds on the right hand side for donations from that donor only. Clicking on the donor maintains the selection (clicking again deselects), which is useful for selecting more than one donor.

## Technical aspects

### Source code

-   Production: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbiclc/src/d3chartpbiclc.js

-   Staging: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbiclc/src/d3chartpbiclc-stg.js

### Data attributes

The code retrieves data attributes in a `<div>` with `id="d3chartcontainerpbiclc"`. These data attributes are:

-   **`data-title`**: sets the title of the chart. If left empty the chart title defaults to _CBPF Contributions_.

-   **`data-year`**: defines the year depicted by the data visualisation when the page loads. The value has to be a string containing the year with century as a decimal number, such as:

    `"2018"`

    If the provided value is not a valid number the datavis will default to the current year. For the accepted values for the years please refer to the data API.

    Multiple years are allowed. In this case, the values have to be separated by comma, for instance:

    `"2016, 2017, 2018"`

    This value defines only the selected year when the page loads: the user can easily change the selected year by clicking the corresponding buttons. Also, the user can select more than one year.

-   **`data-contribution`**: defines the contribution type depicted by the data visualisation when the page loads. The value has to be a string. Accepted values:

    -   `"total"`: shows the total contributions (paid + pledge).
    -   `"paid"`: shows only the paid contributions.
    -   `"pledge"`: shows only the pledged values.

    If the value is not an accepted value, it defaults to `"total"`.

-   **`data-selectedcountry`**: defines the selected country (donor or CBPF) when the page loads. For not selecting any country set the value to `"none"`, or just leave it empty:

    `""`

    For individual countries set the value accordingly, such as:

    `"Yemen"`.

    For more than one country separate the values with commas, such as:

    `"Yemen, Sudan, Iraq"`.

    In such cases, the list must contain only donors or only CBPFs.

    If the selected country is both a donor and a CBPF, define which one will be selected by using `"@"` followed by `"donor"` or `"fund"`. For instance, `"Ukraine@donor"` will select Ukraine as a donor, while `"Ukraine@fund"` will select Ukraine as a fund. If the selected country is both a donor and a CBPF but there is no indication regarding which one should be selected (`"@fund"` or `"@donor"`), the value will default to `"none"`.

    For the accepted values, please refer to the data API. If the value is not a valid one it defaults to `"none"`. This value defines only the selected country when the page loads: the CBPFs (and donors) can be easily selected by clicking on the lollipops.

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
-   https://cbpfgms.github.io/css/d3chartstylespbiclc.css: contains styles for this dataviz.
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

-   Contributions data: https://cbpfapi.unocha.org/vo2/odata/ContributionTotal?$format=csv&ShowAllPooledFunds=1

#### Master Tables:

-   Regional funds: https://cbpfgms.github.io/pfbi-data/mst/MstRhpf.json

#### Other:

-   Donor flags: https://cbpfgms.github.io/img/assets/flags24.json

### Miscellaneous

#### Data filters

When loaded at the [production](https://cbpf.data.unocha.org/) or [staging](https://cbpfgms.github.io/cbpf-bi-stag/) sites, the code for this data visualization doesn't fetch the data from the APIs described above directly. Instead, on those sites, a different script fetches the data, checks for the types in the objects and then passes the data to the code. This filtering script is [documented here](../../Utils/CBPF-BI-filters.md).

_Note_: any error in the data types will be logged **only on the staging site** (check the browser's console). On the production site no error will be logged. On both sites, the object with the error is simply ignored.

## Notes

This dataviz can be embedded in any page, the code automatically fetches all data, master tables, libraries and style sheets needed.
Just copy/paste the following snippet:

`<div id="d3chartcontainerpbiclc" data-title="CBPF Contributions" data-year="2025" data-contribution="total" data-selectedcountry="none" data-showhelp="false" data-showlink="true" data-responsive="true" data-lazyload="true"></div><script type="text/javascript" src="https://cbpfgms.github.io/pbiclc/src/d3chartpbiclc.js"></script>`
