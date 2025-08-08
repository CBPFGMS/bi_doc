# Contributions

Data visualization hosted at: https://cerf.data.unocha.org/#Contributions

Data visualization in the staging site: https://cbpfgms.github.io/cerf-bi-stag/#Contributions

## Introduction

### Purpose

**Contributions** is a small multiples chart that displays all contributions of every individual donor along the years, with focus to the total donated considering all years.

### Data encoding

The data visualization is a small multiples, a series of small charts with the same encodings, next to each other.

Each individual chart is a bar chart, where each bar represents a single year, and a line chart superimposed, for facilitating a quick glimpse of the donations trend. Next to the last completed year there is a number indicating the total donated for that donor, for all years. The current year, because incomplete, is displayed as a hatched bar separated from the other bars.

### Interactivity

1. At the top right corner of the dataviz there are three buttons:

    - **Share**: copies a link with all the current selections to the clipboard. Use that link to go to the Bookmark page;
    - **Image**: downloads a snapshot of the chart, as a .png file or as a .pdf file. You can also right-click anywhere in the chart to download a snapshot containing the tooltip;
    - **Csv**: downloads the data as a .csv file;

2. The first group of radio buttons selects between showing only the top 20 donors (for all time) or all donors.

3. The second group of radio buttons sorts the small multiples by the donated amount (all time) or by alphabetical order.

4. Clicking any small multiple brings a regular bar chart, where individual values for each contribution year are detailed.

## Technical aspects

### Source code

-   Production: https://github.com/UN-OCHA/cerfdata-unocha-org/blob/master/charts/contributionschart.js

-   Staging: https://github.com/CBPFGMS/cerf-bi-stag/blob/main/charts/contributionschart.js

### Data attributes

The code retrieves data attributes in a `<div>` with `id="d3chartcontainerallocover"`. These data attributes are:

-   **`data-donors`**: defines what are the donors displayed when the chart first loads. Accepted values:

    -   `top`: shows only the top 20 donors (considering all time donations)
    -   `all`: shows all donors.

-   **`data-order`**: defines the sorting order of the small multiples when the chart first loads. Accepted values:

    -   `contributions`: sorts the small multiples by contribution values (considering all time donations)
    -   `alphabetical`: sorts the small multiples by the donors' names alphabetically.

-   **`data-lazyload`**: defines if the animation starts when the SVG is visible. Accepted values:

    -   `"true"`: the animation starts only when the SVG is visible in the browser window.
    -   `"false"`: the animation starts when the page is loaded, regardless if the SVG is visible.

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
-   html2canvas, version 1.0.0-rc.1. Source: https://cbpfgms.github.io/libraries/html2canvas.min.js
-   jsPdf, version 1.5. Source: https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.5.3/jspdf.min.js

### APIs

#### Data APIs:

-   Contributions data: https://cbpfgms.github.io/pfbi-data/cerf_sample_data/CERF_ContributionTotal.csv

#### Master Tables:

No master table used

#### Other

-   Donor flags (production site): https://github.com/UN-OCHA/cerfdata-unocha-org/blob/master/assets/img/flags24.json

-   Donor flags (staging site): https://github.com/CBPFGMS/cerf-bi-stag/blob/master/assets/img/flags24.json
