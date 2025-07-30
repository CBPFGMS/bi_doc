# Progress dashboard

Data visualization hosted at: https://cbpf.data.unocha.org/results

Data visualization in the staging site: https://cbpfgms.github.io/cbpf-bi-stag/results

## Introduction

### Purpose

**Progress dashboard** is a collection of data visualizations that shows the allocations corresponding to a given _report year_. This is not the same of allocation year: a given report year may contain data for allocations in several different years. For instance, the report year 2025 contains data for 2024, 2023 etc, up to 2018.

When selecting a report year, as well as the other filters (fund, allocation source, allocation type), the dashboard shows a series of small tables and data visualizations:

-   Top figures with total allocations, number of partners and number of projects, for each allocation year contained in that report year.
-   A grouped pictogram chart showing targeted and reached people with breakdown by beneficiary gender and age (women, girls, men and boys).
-   A grouped bar chart showing targeted and reached people for each beneficiary type (Internally Displaced People,Host Communities,Other, Returnees,Refugees).
-   A grouped bar chart showing targeted and reached people for each organization type.
-   A grouped bar chart showing targeted and reached people for each sector.
-   A world map showing targeted and reach people for each allocation location.

### Data encoding

This uses grouped pictograms and grouped bar charts, where in each group one bar (or pictogram) represents targeted people and another bar (or pictogram) represents reached people.

The world map shows allocations by geographic position, where the size of the circle (not its radius) encodes the number of targeted people. A color gradient for the circles encodes the percentage of reached people (note: that percentage can be greater than 100%, that is, it's possible that the number of reached people is bigger than the original number of targeted people).

### Interactivity

1. At the top of the dashboard the user can select the report year, either via dropdown or clicking the arrows. Only one report year can be selected.

2. Below the year selection dropdown a scroolspy indicates the visible charts in the dashboard, and when clicked the page jumps to that chart.

3. Three dropdowns allow filtering by:

    - Fund
    - Allocation type
    - Allocation source

4. The button _Click for expanding the overview section_ shows a mini bar chart with the breakdown of allocation years for the selected report year. The bar chart can show:

    - Allocations
    - Number of partners
    - Number of projects

5. For each chart, two icons on the top right corner allow:

    - Downloading the data as CSV
    - Downloading or copying to the clipboard a screenshot of the chart

6. All charts, including the world map, show detailed figures when hovered over.

## Technical aspects

### Source code

This is the source code for the project: https://github.com/CBPFGMS/cbpfgms.github.io/tree/master/resultsdash/src/

There is no production and staging source codes. The build is hosted in the staging site for development purposes.

### Data attributes

There is no data attribute required by the code.

### Language

Typescript, with React components in `.tsx` files.

### HTML

The build requires a div with `id="resultsroot"` for rendering the React components.

### CSS

For correct displaying the world map, the Leaflet CSS link should be added to the page:

```
<link
	rel="stylesheet"
	href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css"
	integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY="
	crossorigin=""
/>
```

### Libraries

This is a React project built with Vite. The `package.json` describes the required dev and build dependencies:

```
{
	"name": "resultsdash",
	"private": true,
	"version": "0.0.0",
	"type": "module",
	"scripts": {
		"dev": "vite",
		"build": "tsc && vite build",
		"lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
		"preview": "vite preview"
	},
	"dependencies": {
		"@emotion/react": "^11.11.1",
		"@emotion/styled": "^11.11.0",
		"@mui/icons-material": "^5.14.3",
		"@mui/material": "^5.14.5",
		"@react-spring/web": "^9.7.3",
		"@types/d3": "^7.4.0",
		"@types/parse-json": "^4.0.0",
		"d3": "^7.8.5",
		"leaflet": "^1.9.4",
		"react": "^18.2.0",
		"react-dom": "^18.2.0",
		"react-intersection-observer": "^9.5.2",
		"react-leaflet": "^4.2.1",
		"react-tooltip": "^5.21.1",
		"vite-plugin-svgr": "^3.2.0",
		"zod": "^3.22.4",
		"html-to-image": "^1.11.11"
	},
	"devDependencies": {
		"@types/leaflet": "^1.9.4",
		"@types/react": "^18.2.15",
		"@types/react-dom": "^18.2.7",
		"@typescript-eslint/eslint-plugin": "^6.0.0",
		"@typescript-eslint/parser": "^6.0.0",
		"@vitejs/plugin-react-swc": "^3.3.2",
		"eslint": "^8.45.0",
		"eslint-plugin-react-hooks": "^4.6.0",
		"eslint-plugin-react-refresh": "^0.4.3",
		"typescript": "^5.6.2",
		"vite": "^4.4.5"
	},
	"packageManager": "yarn@4.2.1"
}
```

### APIs

All the API calls, be it data or master tables, are validated using Zod schemas. The validation is made for each object in the data array, not for the whole data, meaning that only the invalid object will be ignored.

The staging site logs any invalid object as a warning (but not the production site).

For all Zod schemas, these constants are used:

```
const numberOfFunds = 252,
	numberOfAllocationSources = 4,
	numberOfOrganizationTypes = 4,
	numberOfBeneficiaryTypes = 23,
	numberOfSectors = 17;

const beneficiariesSchema = z.number().nullable();
```

#### Data APIs:

-   Sectors data: https://cbpfgms.github.io/pfbi-data/cbpf/results/ByCluster.csv

    Zod schema for the sectors data:

    ```
    z.object({
    	PooledFundId: z.number().min(1).max(numberOfFunds),
    	AllocationYear: z.number(),
    	ReportApprovedDate: z.date(),
    	AllocationtypeId: z.number(),
    	AllocationSourceId: z.number().min(1).max(numberOfAllocationSources),
    	ClusterId: z.number().min(1).max(numberOfSectors),
    	ClusterBudget: z.number().min(0),
    	TargetedMen: z.number().nullable(),
    	TargetedWomen: z.number().nullable(),
    	TargetedBoys: z.number().nullable(),
    	TargetedGirls: z.number().nullable(),
    	ReachedMen: z.number().nullable(),
    	ReachedWomen: z.number().nullable(),
    	ReachedBoys: z.number().nullable(),
    	ReachedGirls: z.number().nullable(),
    });
    ```

-   Disability data: https://cbpfgms.github.io/pfbi-data/cbpf/results/ByGender_Disability.csv

    Zod schema for disability data:

    ```
    z.object({
    	PooledFundId: z.number().min(1).max(numberOfFunds),
    	AllocationYear: z.number(),
    	ReportApprovedDate: z.date(),
    	AllocationtypeId: z.number(),
    	AllocationSourceId: z.number().min(1).max(numberOfAllocationSources),
    	NumbofProjects: z.number(),
    	TotalNumbPartners: z.number(),
    	Budget: z.number().min(0),
    	TargetedMen: z.number().nullable(),
    	TargetedWomen: z.number().nullable(),
    	TargetedBoys: z.number().nullable(),
    	TargetedGirls: z.number().nullable(),
    	ReachedMen: z.number().nullable(),
    	ReachedWomen: z.number().nullable(),
    	ReachedBoys: z.number().nullable(),
    	ReachedGirls: z.number().nullable(),
    	DisabledMen: z.number().nullable(),
    	DisabledWomen: z.number().nullable(),
    	DisabledBoys: z.number().nullable(),
    	DisabledGirls: z.number().nullable(),
    	ReachedDisabledMen: z.number().nullable(),
    	ReachedDisabledWomen: z.number().nullable(),
    	ReachedDisabledBoys: z.number().nullable(),
    	ReachedDisabledGirls: z.number().nullable(),
    });
    ```

-   Location data: https://cbpfgms.github.io/pfbi-data/cbpf/results/ByGender_Disability.csv

    Zod schema for location data:

    ```
    z.object({
    	PooledFundId: z.number().min(1).max(numberOfFunds),
    	AllocationYear: z.number(),
    	ApprovedDate: z.date(),
    	LocationID: z.number(),
    	AllocationtypeId: z.number(),
    	AllocationSourceId: z.number().min(1).max(numberOfAllocationSources),
    	TargetMen: z.number().nullable(),
    	TargetWomen: z.number().nullable(),
    	TargetBoys: z.number().nullable(),
    	TargetGirls: z.number().nullable(),
    	ReachedMen: z.number().nullable(),
    	ReachedWomen: z.number().nullable(),
    	ReachedBoys: z.number().nullable(),
    	ReachedGirls: z.number().nullable(),
    });
    ```

-   Allocation type data: https://cbpfgms.github.io/pfbi-data/cbpf/results/ByType.csv

    Zod schema for allocation type data:

    ```
    z.object({
    	PooledFundId: z.number().min(1).max(numberOfFunds),
    	AllocationYear: z.number(),
    	ReportApprovedDate: z.date(),
    	BeneficiaryTypeId: z.number().min(1).max(numberOfBeneficiaryTypes),
    	AllocationtypeId: z.number(),
    	AllocationSourceId: z.number().min(1).max(numberOfAllocationSources),
    	TargetMen: z.number().nullable(),
    	TargetWomen: z.number().nullable(),
    	TargetBoys: z.number().nullable(),
    	TargetGirls: z.number().nullable(),
    	ReachedMen: z.number().nullable(),
    	ReachedWomen: z.number().nullable(),
    	ReachedBoys: z.number().nullable(),
    	ReachedGirls: z.number().nullable(),
    });
    ```

-   Organization data: https://cbpfgms.github.io/pfbi-data/cbpf/results/ByGender_Disability.csv

    Zod schema for organization data:

    ```
    z.object({
    	PooledFundId: z.number().min(1).max(numberOfFunds),
    	AllocationYear: z.number(),
    	ReportApprovedDate: z.date(),
    	AllocationtypeId: z.number(),
    	AllocationSourceId: z.number().min(1).max(numberOfAllocationSources),
    	OrganizationType: z.number().min(1).max(numberOfOrganizationTypes),
    	NumbofProjects: z.number(),
    	TotalNumbPartners: z.number(),
    	Budget: z.number().min(0),
    	TargetedMen: z.number().nullable(),
    	TargetedWomen: z.number().nullable(),
    	TargetedBoys: z.number().nullable(),
    	TargetedGirls: z.number().nullable(),
    	ReachedMen: z.number().nullable(),
    	ReachedWomen: z.number().nullable(),
    	ReachedBoys: z.number().nullable(),
    	ReachedGirls: z.number().nullable(),
    });
    ```

-   Approved allocations data: https://cbpfgms.github.io/pfbi-data/cbpf/results/ByGender_Disability.csv

    Zod schema for approved allocations data:

    ```
    z.object({
    	AllocationYear: z.number(),
    	ApprovedBudget: z.number().min(0),
    	ApprovedReserveBudget: z.number().min(0),
    	ApprovedReserveBudgetPercentage: z.number().min(0).max(100),
    	ApprovedStandardBudget: z.number().min(0),
    	ApprovedStandardBudgetPercentage: z.number().min(0).max(100),
    	FundingType: z.number(),
    	OrganizationType: z.string(),
    	PipelineBudget: z.number().min(0),
    	PipelineReserveBudget: z.number().min(0),
    	PipelineReserveBudgetPercentage: z.number().min(0).max(100),
    	PipelineStandardBudget: z.number().min(0),
    	PipelineStandardBudgetPercentage: z.number().min(0).max(100),
    	PooledFundName: z.string(),
    	PooledFundId: z.number().min(1).max(numberOfFunds).optional(),
    });
    ```

#### Master Tables:

-   Location master: https://cbpfgms.github.io/pfbi-data/cbpf/results/locationMst.csv

-   Beneficiaries master: https://cbpfgms.github.io/pfbi-data/cbpf/results/MstBeneficiaryType.csv

-   Allocation types master: https://cbpfgms.github.io/pfbi-data/cbpf/results/MstAllocationType.csv

-   Funds master: https://cbpfgms.github.io/pfbi-data/mst/MstCountry.json

-   Allocation sources master: https://cbpfgms.github.io/pfbi-data/mst/MstAllocation.json

-   Organization types master: https://cbpfgms.github.io/pfbi-data/mst/MstOrganization.json

-   Sectors master: https://cbpfgms.github.io/pfbi-data/mst/MstCluster.json

## Notes

For developing this project locally, copy the project folder and do:

`yarn install` or just `yarn`

For running it locally, do:

`yarn dev`

For building it, do:

`yarn build`
