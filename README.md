# Sales Analysis | Power BI Project

An interactive sales and profitability report built in **Microsoft Power BI Desktop**. The project brings together sales transactions, product, customer, store, region, calendar, and returns data to explore business performance across time and locations.

## Business objective

Understand sales performance and product returns through a single report, with measures for revenue, cost, profit, margin, transaction volume, and return rate. The report is designed to let a viewer compare performance by product brand, store location, and time period.

### Business questions

- How do transaction volume, profit, profit margin, and return rate vary by product brand?
- How are transactions distributed across store countries, states, and cities?
- How did weekly revenue trend during 1998?
- How does the latest month's revenue compare with a target based on a 5% lift over the previous month's revenue?
- How do current-period transaction, profit, and return KPIs compare with the previous calendar month?

## Report preview

Open [`report/Sales Analysis.pbix`](report/Sales%20Analysis.pbix) in Power BI Desktop to explore the report. 
The screenshot below: 
<img width="1998" height="1122" alt="Screenshot 2026-10-21 135522" src="https://github.com/user-attachments/assets/15a3d1cd-a7f0-4cda-a7ef-f89fc9322553" />

## Dashboard features

The report page, **Sales Analysis**, includes:

- **Brand performance matrix** — Total Transactions, Total Profit, Profit Margin, and Return Rate by product brand; Top 30 brands by transaction count; data bars and color-scale conditional formatting.
- **KPI visuals** — current-month transactions, profit, and returns compared with previous-month measures.
- **Store map** — transaction volume by store city.
- **Country slicer** — interactive store-country filter with Select All.
- **Geographic treemap** — transaction volume by country, state, and city, with drill-down.
- **Weekly revenue chart** — revenue trend filtered to 1998.
- **Revenue vs. target gauge** — revenue compared with a target set at 105% of the previous month's revenue.
- **Visual interactions** — the matrix is configured not to filter the treemap.

## Tools and skills demonstrated

- Power Query: importing CSV and Excel data, promoting headers, assigning data types, creating derived columns, replacing null values, and combining files from a folder.
- Data modeling: one-to-many relationships, single-direction filtering, lookup and fact tables, a snowflake relationship from Stores to Regions, and an inactive date relationship for stock date.
- DAX: calculated columns, iterator-based revenue and cost measures, profitability metrics, return rate, and previous-month time-intelligence measures.
- Report design: matrix conditional formatting, KPI visuals, map, slicer, treemap, column chart, gauge, drill-down, filtering, and visual interaction control.

## Repository structure

```text
sales-analysis/
├── README.md
├── .gitignore
├── report/
│   └── Sales Analysis.pbix
├── data/
│   └── raw/
│       └── README.md
├── docs/
│   ├── data-model-and-power-query.md
│   └── dax-measures.md
└── screenshots/
    └── README.md
```

## Data files

The project brief specifies these source files:

| File | Role |
|---|---|
| `Customers.csv` | Customer attributes and customer keys |
| `Products.csv` | Product attributes, prices, and costs |
| `Stores.csv` | Store attributes and region keys |
| `Regions.csv` | Region lookup |
| `Calendar.csv` | Date dimension |
| `Returns.csv` | Returned-product records and quantities |
| `Transactions_1997.xlsx` | Sales transaction data for 1997 |
| `Transactions_1998.xlsx` | Sales transaction data for 1998 |


## Data model overview

The intended model follows a star/snowflake pattern:

- `Customers` → `Transactions`
- `Products` → `Transactions`
- `Stores` → `Transactions`
- `Calendar` → `Transactions` through the transaction date (active) and stock date (inactive)
- `Products` → `Returns`
- `Calendar` → `Returns`
- `Stores` → `Returns`
- `Regions` → `Stores`

Lookup tables filter fact tables through one-to-many, single-direction relationships. `Transactions` and `Returns` are connected through shared dimensions rather than directly to each other. More detail is in [`docs/data-model-and-power-query.md`](docs/data-model-and-power-query.md).

## Key metrics

| Metric | Definition |
|---|---|
| Quantity Sold | Sum of transaction quantity |
| Quantity Returned | Sum of return quantity |
| Total Transactions | Number of rows in the transaction fact table |
| Total Returns | Number of rows in the returns fact table |
| Return Rate | Quantity Returned ÷ Quantity Sold |
| Total Revenue | Sum of transaction quantity × product retail price |
| Total Cost | Sum of transaction quantity × product cost |
| Total Profit | Total Revenue − Total Cost |
| Profit Margin | Total Profit ÷ Total Revenue |
| Revenue Target | Previous-month revenue × 1.05 |

Illustrative DAX patterns are documented in [`docs/dax-measures.md`](docs/dax-measures.md). Check table and column names against the PBIX before reusing formulas in another model.

## How to open the project

1. Download or clone this repository.
2. Open `report/Sales Analysis.pbix` with **Power BI Desktop**.
3. If Power BI prompts for missing source paths, place the source files in `data/raw/` and update the source paths in Power Query as needed.
4. Select **Transform data** to review the queries, then use **Close & Apply** if changes are required.
5. Refresh the model and inspect the report page.

Power BI Desktop is required to inspect or edit the PBIX. A GitHub preview does not provide the full interactive Power BI experience.

## Reproducing the transformations

The documented Power Query steps and model design are in [`docs/data-model-and-power-query.md`](docs/data-model-and-power-query.md). The source brief includes combining the 1997 and 1998 Excel workbooks from a folder, standardizing types, and creating derived fields such as full name, birth year, discount price, full address, area code, calendar attributes, and weekend flag.

## Limitations and verification

- The uploaded PBIX is included as provided; it has not been edited or rebuilt.
- The source CSV/XLSX files were not included in the upload used to create this repository, so the package cannot independently validate refresh behavior.
- Formulas in the DAX documentation are reference implementations based on the assignment brief, not an assertion that every formula exactly matches the model's existing measures.
- No revenue, profit, return-rate, or geographic findings are reported without validating the underlying data.
- For public GitHub publication, confirm that you are allowed to share the PBIX and source data, and remove any personal, confidential, or licensed information.

## Next portfolio improvement

Add a screenshot of the report to `screenshots/`, then write a short findings section based on the actual model output (for example, leading brands, monthly revenue movement, or locations with unusual return rates). Only include findings you can verify from the report.
