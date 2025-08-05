# Progress dashboard

Data visualization hosted at: https://cbpf.data.unocha.org/results

Data visualization in the staging site: https://cbpfgms.github.io/cbpf-bi-stag/results

## Introduction

### Purpose

**Progress dashboard** is a collection of data visualizations that shows the allocations corresponding to a given year, with focus on _under implementation_ and _programmatically closed_ allocations.

When selecting a year, as well as the other filters (fund, allocation source, allocation type and allocation status), the dashboard shows a series of small tables and data visualizations:

-   Top figures with total allocations, allocations under implementation, number of partners and number of projects, for each allocation year.
-   A grouped pictogram chart showing targeted and reached people with breakdown by beneficiary gender and age (women, girls, men and boys).
-   A grouped bar chart showing targeted and reached people for each beneficiary type (Internally Displaced People,Host Communities,Other, Returnees,Refugees).
-   A grouped bar chart showing targeted and reached people for each organization type.
-   A grouped bar chart showing targeted and reached people for each sector.
-   A grouped bar chart showing targeted and reached people with disabilities, with breakdown by beneficiary gender and age (women, girls, men and boys).
-   A donut chart indicating the percentage of GBV (_Gender-Based Violence_) budget for targeted and reached people.
-   A series of area charts showing allocations by emergency groups.
-   A mix of grouped bar chart and donut chart showing CVA (_Cash and Voucher Assistance_) allocations, with breakdown by CVA type and sector.
-   An option for generating a table with the global indicators for all allocations under the current filter.

### Data encoding

This dashboard uses grouped pictograms and grouped bar charts, where in each group one bar (or pictogram) represents targeted people and another bar (or pictogram) represents reached people.

The donut charts indicate amount or percentage relative to a full circle (360°).

The area chart uses time for the x axis and allocated amounts for the y axis.

### Interactivity

1. At the top of the dashboard a scroolspy indicates the visible charts in the dashboard, and when clicked the page jumps to that chart.

2. Below the scrollspy, four dropdowns allow filtering by:

    - Year
    - Fund
    - Allocation type
    - Allocation source

3. The status area breaks down all allocations in the selection in _under implementation_ and _programmatically closed_, and allows filtering as well.

4. For each chart, two icons on the top right corner allow:

    - Downloading the data as CSV
    - Downloading or copying to the clipboard a screenshot of the chart

5. For the _Beneficiary type_, _Organization_ and _Sector_ charts checkboxes allow the filtering by gender and age (women, girls, men and boys).

6. In the _Emergency groups_ chart the user can select two data encodings:

    - Overview: a simple bar chart, aggregating all values in the year selection.
    - Time series: an area chart depicting time on the x axis

    Also, the user can aggregate all emergency types or can separate them by emergency group.

7. In the _CVA_ chart a switch allows the user to choose between dollar values or number of people, both for targeted and reached numbers. For each CVA type, when hovered over, a tooltip with a breakdown by sector is displayed.

8. All charts show detailed figures when hovered over.

## Technical aspects

### Source code

This is the source code for the project: https://github.com/CBPFGMS/cbpfgms.github.io/tree/master/progress/src/

There is no production and staging source codes. The build is hosted in the staging site for development purposes.

### Data attributes

The code looks for the following data attributes in a `div` with `id="progressroot"`:

-   **`data-year`**: defines the year depicted by the data visualisation when the page loads. Only one year is allowed. If no value is provided, the current year is used.

-   **`data-fundtype`**: defines the fund used by the code. The values are:

    -   `1`: CBPF
    -   `2`: CERF
        If no value is provided, both CERF and CBPF data will be fetched.

-   **`data-startyear`**: defines the first year fetched in the data, the last year being the current year. If no value is provided all data will be fetched.

### Language

Typescript, with React components in `.tsx` files.

### HTML

The build requires a div with `id="progressroot"` for rendering the React components.

### CSS

For better readability, this dashboard uses Montserrat:

```
<link
	rel="stylesheet"
	crossorigin="anonymous"
	href="https://fonts.googleapis.com/css?family=Montserrat:400,400i,700,700i,600,600i"
/>
```

### Libraries

This is a React project built with Vite. The `package.json` describes the required dev and build dependencies:

```
{
	"name": "progress",
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
		"@emotion/react": "^11.11.4",
		"@emotion/styled": "^11.11.5",
		"@gromy/react-swipeable-views": "^0.15.1",
		"@mui/icons-material": "^5.15.16",
		"@mui/material": "^6.1.0",
		"@react-spring/web": "^9.7.3",
		"d3": "^7.9.0",
		"html-to-image": "^1.11.11",
		"idb": "^8.0.0",
		"react": "^18.2.0",
		"react-dom": "^18.2.0",
		"react-intersection-observer": "^9.10.2",
		"react-tooltip": "^5.26.4",
		"zod": "^3.22.4"
	},
	"devDependencies": {
		"@types/d3": "^7",
		"@types/react": "^18.2.66",
		"@types/react-dom": "^18.2.22",
		"@types/react-swipeable-views": "^0",
		"@typescript-eslint/eslint-plugin": "^7.2.0",
		"@typescript-eslint/parser": "^7.2.0",
		"@vitejs/plugin-react-swc": "^3.5.0",
		"eslint": "^8.57.0",
		"eslint-plugin-react-hooks": "^4.6.0",
		"eslint-plugin-react-refresh": "^0.4.6",
		"typescript": "^5.2.2",
		"vite": "^5.2.0"
	}
}
```

### APIs

All the API calls, be it data or master tables, are validated using Zod schemas. The validation is made for each object in the data array, not for the whole data, meaning that only the invalid object will be ignored.

The staging site logs any invalid object as a warning (but not the production site).

Some strings are validated using regex:

```
const splitRegex = /^(\d*\|\d*\|\d*\|\d*\|\d*)$/,
	dateRegex =
		/\b([1-9]|0[1-9]|1[012])\/([1-9]|0[1-9]|1[0-9]|2[0-9]|3[01])\/20\d\d\b/;
```

#### Data APIs:

-   Projects summary: https://cbpfapi.unocha.org/vo3/odata/GlobalGenericDataExtract?SPCode=PF_PROJ_SUMMARY

    Zod schema for the projects summary data:

    ```
    z.object({
    	FundType: z.union([z.literal(1), z.literal(2)]),
    	PooledFundId: z.number().int().nonnegative(),
    	AllocationtypeId: z.number().int().nonnegative(),
    	ChfId: z.number().int().nonnegative(),
    	ChfProjectCode: z.string(),
    	OrgId: z.number().int().nonnegative(),
    	PrjDuration: z.string(),
    	EndDate: z.string().regex(dateRegex, "Invalid date format"),
    	Budget: z.number().nonnegative(),
    	BenM: z.number().int().nonnegative(),
    	BenW: z.number().int().nonnegative(),
    	BenB: z.number().int().nonnegative(),
    	BenG: z.number().int().nonnegative(),
    	GMId: z.union([z.number(), z.string()]).nullable(),
    	GAMId: z.number().nullable(),
    	GlbPrjStatusId: z.number().int().nonnegative(),
    	GlobalUniqueOrgId: z.number().int().nonnegative(),
    	DisabilityMarkerId: z.number().int().nonnegative().nullable(),
    	GenderEqualityMarkerId: z.number().int().nonnegative().nullable(),
    	GBVMarkerId: z.number().int().nonnegative().nullable(),
    	GAMRefNumber: z.string().nullable(),
    	PartnerProjectRisk: z.string().nullable(),
    	PartnerRisk: z.string().nullable(),
    	DisabledM: z.number().int().nonnegative().nullable(),
    	DisabledW: z.number().int().nonnegative().nullable(),
    	DisabledB: z.number().int().nonnegative().nullable(),
    	DisabledG: z.number().int().nonnegative().nullable(),
    	AchM: z.number().int().nonnegative().nullable(),
    	AchW: z.number().int().nonnegative().nullable(),
    	AchB: z.number().int().nonnegative().nullable(),
    	AchG: z.number().int().nonnegative().nullable(),
    	BenMSplit: z
    		.string()
    		.regex(splitRegex, "Invalid split string format")
    		.nullable(),
    	BenWSplit: z
    		.string()
    		.regex(splitRegex, "Invalid split string format")
    		.nullable(),
    	BenBSplit: z
    		.string()
    		.regex(splitRegex, "Invalid split string format")
    		.nullable(),
    	BenGSplit: z
    		.string()
    		.regex(splitRegex, "Invalid split string format")
    		.nullable(),
    	BenTotSplit: z
    		.string()
    		.regex(splitRegex, "Invalid split string format")
    		.nullable(),
    	AchMSplit: z
    		.string()
    		.regex(splitRegex, "Invalid split string format")
    		.nullable(),
    	AchWSplit: z
    		.string()
    		.regex(splitRegex, "Invalid split string format")
    		.nullable(),
    	AchBSplit: z
    		.string()
    		.regex(splitRegex, "Invalid split string format")
    		.nullable(),
    	AchGSplit: z
    		.string()
    		.regex(splitRegex, "Invalid split string format")
    		.nullable(),
    	AchTotSplit: z
    		.string()
    		.regex(splitRegex, "Invalid split string format")
    		.nullable(),
    	AchDisabledM: z.number().int().nonnegative().nullable(),
    	AchDisabledW: z.number().int().nonnegative().nullable(),
    	AchDisabledB: z.number().int().nonnegative().nullable(),
    	AchDisabledG: z.number().int().nonnegative().nullable(),
    	GBVBudget: z.number().nonnegative().nullable(),
    	AchGBVBudget: z.number().nonnegative().nullable(),
    	GBVPeopleTgt: z.number().int().nonnegative().nullable(),
    	AchGBVPeople: z.number().int().nonnegative().nullable(),
    	GendEqBudget: z.number().nonnegative().nullable(),
    	AchGendEqBudget: z.number().nonnegative().nullable(),
    	GendEqPeopleTgt: z.number().int().nonnegative().nullable(),
    	AchGendEqPeople: z.number().int().nonnegative().nullable(),
    	ProtBudget: z.number().nonnegative().nullable(),
    	AchProtBudget: z.number().nonnegative().nullable(),
    	ProtPeopleTgt: z.number().int().nonnegative().nullable(),
    	AchProtPeople: z.number().int().nonnegative().nullable(),
    	RptCode: z.union([z.literal(1), z.literal(2)]).nullable(),
    	StartDate: z.string().regex(dateRegex, "Invalid date format"),
    	PrjApprDate: z.string().regex(dateRegex, "Invalid date format"),
    	CVATotPeople: z.number().int().nonnegative().nullable(),
    	AchCVATotPeople: z.number().int().nonnegative().nullable(),
    })
    ```

-   Sectors data: https://cbpfapi.unocha.org/vo3/odata/GlobalGenericDataExtract?SPCode=PF_RPT_CLST_BENEF

    Zod schema for sectors data:

    ```
    z.object({
    	PooledFundName: z.string(),
    	PooledFundId: z.number().int().nonnegative(),
    	AllocationTypeId: z.number().int().nonnegative(),
    	ChfId: z.number().int().nonnegative(),
    	ChfProjectCode: z.string(),
    	CountryClusterId: z.number().int().nonnegative(),
    	GlobalClusterId: z.number().int().nonnegative(),
    	Percentage: z.number().nonnegative(),
    	CALCBudgetByCluster: z.number().nonnegative(),
    	TargetMen: z.number().int().nonnegative().nullable(),
    	TargetWomen: z.number().int().nonnegative().nullable(),
    	TargetBoys: z.number().int().nonnegative().nullable(),
    	TargetGirls: z.number().int().nonnegative().nullable(),
    	ActualMen: z.number().int().nonnegative().nullable(),
    	ActualWomen: z.number().int().nonnegative().nullable(),
    	ActualBoys: z.number().int().nonnegative().nullable(),
    	ActualGirls: z.number().int().nonnegative().nullable(),
    	GlobalInstanceStatusId: z.number().int().nonnegative().nullable(),
    	SubmissionDate: z
    		.string()
    		.regex(dateRegex, "Invalid date format")
    		.nullable(),
    });
    ```

-   Global indicators data: https://cbpfgms.github.io/pfbi-data/cbpf/results/ByGender_Disability.csv

    Zod schema for global indicators data:

    ```
    z.object({
    	FundTypeId: z.union([z.literal(1), z.literal(2)]),
    	PooledFundId: z.number().int().nonnegative(),
    	CHFId: z.number().int().nonnegative(),
    	CHFProjectCode: z.string(),
    	ClstrId: z.number().int().nonnegative().nullable(),
    	GlbClstrId: z.number().int().nonnegative(),
    	GlbIndicId: z.number().int().nonnegative(),
    	CommClstrId: z.number().int().nonnegative().nullable(),
    	GlbIndicType: z.union([z.literal(1), z.literal(2)]).nullable(),
    	TgtM: z.number().nonnegative().nullable(),
    	TgtW: z.number().nonnegative().nullable(),
    	TgtB: z.number().nonnegative().nullable(),
    	TgtG: z.number().nonnegative().nullable(),
    	TgtTotal: z.number().nonnegative(),
    	AchM: z.number().nonnegative().nullable(),
    	AchW: z.number().nonnegative().nullable(),
    	AchB: z.number().nonnegative().nullable(),
    	AchG: z.number().nonnegative().nullable(),
    	AchTotal: z.number().nonnegative().nullable(),
    });
    ```

-   Emergencies data: https://cbpfgms.github.io/pfbi-data/cbpf/results/ByType.csv

    Zod schema for emergencies data:

    ```
    z.object({
    	CHFId: z.number().int().nonnegative(),
    	EmergencyTypeId: z.number().int().nonnegative(),
    	EmergencyPercent: z.number().min(0).max(100),
    	PooledFundId: z.number().int().nonnegative(),
    	PooledFundName: z.string(),
    	CHFProjectCode: z.string(),
    	OrganizationName: z.string().nullable(),
    	OrganizationAcronym: z.string().nullable(),
    	OrganizationType: z.string().nullable(),
    	AllocationTypeName: z.string().nullable(),
    	AllocationYear: z.number().int().nonnegative(),
    	ProjectStatus: z.string().nullable(),
    	ProjectBudget: z.number().nonnegative(),
    	EmergencyTypeName: z.string().nullable(),
    });
    ```

-   CVA data: https://cbpfgms.github.io/pfbi-data/cbpf/results/ByGender_Disability.csv

    Zod schema for CVA data:

    ```
    z.object({
    	PooledFundId: z.number().int().nonnegative(),
    	CHFId: z.number().int().nonnegative(),
    	ChfProjectCode: z.string(),
    	OrganizationTypeId: z.number().int().nonnegative(),
    	AllocationYear: z.number().int().nonnegative(),
    	CVATypeId: z.number().int().nonnegative(),
    	ClusterId: z.number().int().nonnegative(),
    	TransferAmount: z.number().nonnegative().nullable(),
    	TotalAmtTransferred: z.number().nonnegative().nullable(),
    	PeopleTargeted: z.number().int().nonnegative().nullable(),
    	PeopleReached: z.number().int().nonnegative().nullable(),
    	GlobalClusterId: z.number().int().nonnegative(),
    });
    ```

#### Master Tables:

-   Allocation types master: https://cbpfapi.unocha.org/vo2/odata/AllocationTypes
-   Organizations master: https://cbpfapi.unocha.org/vo3/odata/GlobalGenericDataExtract?SPCode=PF_ORG_SUMMARY
-   Project status master: https://cbpfapi.unocha.org/vo3/odata/GlobalGenericDataExtract?SPCode=PF_GLB_STATUS
-   Beneficiary types master: "https://cbpfgms.github.io/pfbi-data/cbpf/results/MstBeneficiaryType.csv"
-   Funds master: "https://cbpfapi.unocha.org/vo2/odata/MstPooledFund?$format=csv"
-   Allocation sources master: "https://cbpfapi.unocha.org/vo2/odata/MstAllocationSource?$format=csv"
-   Organization types master: "https://cbpfapi.unocha.org/vo2/odata/MstOrgType?$format=csv"
-   Sectors master: "https://cbpfapi.unocha.org/vo2/odata/MstClusters?$format=csv"
-   Global indicators master: "https://cbpfapi.unocha.org/vo3/odata/GlobalGenericDataExtract?SPCode=GLB_INDIC_MST&GlobalIndicatorType=&$format=csv"
-   Emergencies master: "https://cbpfapi.unocha.org/vo3/odata/GlobalGenericDataExtract?SPCode=EMERG_TYPE_MST&$format=csv"
-   CVA master: **currently hardcoded**

For developing this project locally, copy the project folder and do:

`yarn install` or just `yarn`

For running it locally, do:

`yarn dev`

For building it, do:

`yarn build`
