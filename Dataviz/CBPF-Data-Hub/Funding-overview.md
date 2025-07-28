# Funding overview

Data visualisation hosted at: https://cbpf.data.unocha.org/#fundingoverview_heading

Data visualization in the staging site: https://cbpfgms.github.io/cbpf-bi-stag/#fundingoverview_heading

## Introduction

### Purpose

**Funding overview** is a data visualization that shows the flow of contributions from donors to funds, illustrating simultaneously the total donated and earmarked for each donor and fund as well as the individual flows from donor to fund. It also has an option for displaying each donor and fund in their actual geographic locations.

### Data encoding

This data visualization consists in a static force directed chart. In this dataviz modality, the area (not the radius) of each circle depicts the amount donated or earmarked. The positions of the circles carry no information (except when _show map_ is selected). Connecting donors to funds, the width of each ribbon indicated the amount donated (their lengths carry no information).

### Interactivity

1. At the top right corner of the dataviz there are five buttons:

    - **Share**: copies a link with all the current selections to the clipboard. Use that link to go to the Bookmark page;
    - **Play**: Changes the year at a regular interval, allowing the user to visualise changes along time.
    - **Image**: downloads a snapshot of the chart, as a .png file or as a .pdf file. You can also right-click anywhere in the chart to download a snapshot containing the tooltip;
    - **Csv**: downloads the data as a .csv file;
    - **Help**: shows an annotated layer with tips about how to use and how to understand the chart.

2. The year buttons select the year depicted in the dataviz. Multiple years can be selected by double-clicking or pressing ALT while clicking.

3. Below the year buttons, a dropdown with checkboxes allows filtering the funds by their regions.

4. The _show map_ checkbox moves the circles (donors and funds) to their geographic positions over a background world map.

5. the _show names_ checkbox toggles the labels on and off, for better visualization.

6. Hovering over a donor highlights the funds that received donations from that donor. Similarly, hovering over a fund highlights the donors that donated to that fund.

7. On the right hand side a panel show detailed information for the selection, as well as donor and fund information when their circles are hovered over.

## Technical aspects

### Source code

-   Production: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbifdc/src/d3chartpbifdc.js

-   Staging: https://github.com/CBPFGMS/cbpfgms.github.io/blob/master/pbifdc/src/d3chartpbifdc-stg.js

### Data attributes

The code retrieves data attributes in a `<div>` with `id="d3chartcontainerpbifdc"`. These data attributes are:

-   **`data-title`**: sets the title of the chart. If left empty the chart title defaults to _Contributions flow_.

-   **`data-year`**: defines the year depicted by the data visualisation when the page loads. The value has to be a string containing the year with century as a decimal number, such as:

    `"2018"`

    If the provided value is not a valid number the datavis will default to the current year. For the accepted values for the years please refer to the data API.

    Multiple years are allowed. In this case, the values have to be separated by comma, for instance:

    `"2016, 2017, 2018"`

    This value defines only the selected year when the page loads: the user can easily change the selected year by clicking the corresponding buttons. Also, the user can select more than one year.

-   **`data-showmap`**: defines if the nodes in the force-directed graph are freely positioned or if they follow the geographic position of the respective country (in a map) . The value has to be a string. Accepted values:

    -   `"false"`: nodes are freely positioned on the chart area.
    -   `"true"`: nodes in the correct geographic position.

    If the value is not an accepted value, it defaults to `"false"`.

    This value defines only the selected position of the nodes when the page loads: the user can easily change it by clicking the corresponding button.

-   **`data-shownames`**: defines if the donor/CBPF name is visible. Hiding the name is useful in the map view, where the labels normally overlap . The value has to be a string. Accepted values:

    -   `"false"`: name not visible.
    -   `"true"`: name visible.

    If the value is not an accepted value, it defaults to `"false"`.

    This value defines only if the names are visible when the page loads: the user can easily change it by clicking the corresponding button.

-   **`data-regions`**: defines the selected CBPFs (and all their respective donors) by region, according to [OCHA's Regional Office coverage](https://www.unocha.org/where-we-work/ocha-presence). The value has to be a string. Accepted values:

    -   `"all"`: shows all CBPFs (and their respective donors).
    -   `"Latin America and the Caribbean"`
    -   `"West and Central Africa"`
    -   `"Southern and Eastern Africa"`
    -   `"Middle East and North Africa"`
    -   `"Asia and the Pacific"`

    You can specify more than one region. In that case, they should be separated by a comma, without any space. For instance:

    `"Latin America and the Caribbean,West and Central Africa"`

    If the value is not an accepted one, it will default to `"all"`.

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
-   https://cbpfgms.github.io/css/d3chartstylespbifdc.css: contains styles for this dataviz.
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

No master table used.

#### Other:

-   Donor flags: https://cbpfgms.github.io/img/assets/flags24.json

-   World map: https://raw.githubusercontent.com/CBPFGMS/cbpfgms.github.io/master/img/assets/worldmap.json 

## Notes

This dataviz can be embedded in any page, the code automatically fetches all data, master tables, libraries and style sheets needed.
Just copy/paste the following snippet:

`<div id="d3chartcontainerpbifdc" data-title="Contributions Flow" data-year="2025" data-showmap="false" data-shownames="true" data-regions="all" data-showhelp="false" data-showlink="true" data-responsive="true" data-lazyload="true"></div><script type="text/javascript" src="https://cbpfgms.github.io/pbifdc/src/d3chartpbifdc.js"></script>`
