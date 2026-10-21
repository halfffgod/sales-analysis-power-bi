# Data model and Power Query notes

This document organizes the transformations and model requirements from the supplied project brief.

## Power Query and data preparation

### Global settings
- Turn off **Autodetect new relationships after data is loaded**.
- Set import locale to **English (United States)**.

### Customers
- Promote headers and validate types.
- `customer id`: whole number.
- `customer acct num` and `customer postal code`: text.
- Add `full name` by combining first and last name with a space.
- Add `birth year` from `birthdate` as a whole number.
- Add `has children`: `N` when `total children = 0`, otherwise `Y`.

### Products
- Promote headers and validate types.
- `product id`: whole number; `product sku`: text.
- `product retail price` and `product cost`: decimal numbers.
- Add `discount price` as 90% of retail price; use fixed decimal and round to two decimal places.
- Replace nulls with zero in `recyclable` and `low fat`.

### Stores
- Promote headers and validate types; `store id` and `region id` are whole numbers.
- Add `full address` by joining city, state, and country with comma-space separators.
- Add `area code` using the characters before the first hyphen in `store phone`.

### Regions
- Promote headers and set `region id` to whole number.

### Calendar
- Promote headers and create `Start of Week` (Monday), `Name of Day`, `Start of Month`, `Name of Month`, `Quarter of Year`, and `Year`.
- In the model, mark Calendar as the date table and select its date column.

### Returns
- Promote headers.
- Set all ID columns and quantity to whole number.

### Transactions folder
- Combine `Transactions_1997.xlsx` and `Transactions_1998.xlsx` from a folder.
- Keep the `Content` column to access each workbook, use `Excel.Workbook` to extract workbook contents, retain and promote the data tables' headers, then expand the combined rows.
- Set both date columns to Date and all ID columns and quantity to Whole Number.
- The assignment asks to disable **Include in report refresh** for the seven source queries and then Close & Apply. Review this setting before changing refresh behavior in a portfolio copy.

## Relationships

Use one-to-many relationships with single-direction filtering from lookup tables to fact tables:

| Lookup table | Fact / related table | Relationship |
|---|---|---|
| Customers | Transactions | Customer key |
| Products | Transactions | Product key |
| Stores | Transactions | Store key |
| Calendar | Transactions | Transaction date → Calendar date (active) |
| Calendar | Transactions | Stock date → Calendar date (inactive) |
| Products | Returns | Product key |
| Calendar | Returns | Date key |
| Stores | Returns | Store key |
| Regions | Stores | Region key |

Use the actual key-column names from the supplied model when checking these relationships. Do not create a direct relationship between `Transactions` and `Returns`; they share lookup dimensions. The Stores-to-Regions relationship forms the snowflake part of the model.

## Model formatting and data categories

- Format date fields as `m/d/yyyy`.
- Format product retail price, product cost, and discount price as Currency ($ English).
- Customers: categorize customer city as City, customer postal code as Postal Code, and customer country as Country/Region.
- Stores: categorize store city as City, store state as State or Province, store country as Country/Region, and full address as Address.
- Create the `Weekend`, `End of Month`, `Short Country`, and `Years Since Remodel` calculated columns described in `dax-measures.md`.

## Report page

The assignment specifies a report page named **Sales Analysis** with:
1. A title text box.
2. A Top 30 product-brand matrix with conditional formatting.
3. Three KPI visuals for transactions, profit, and returns against previous-month measures.
4. A map showing transactions by store city.
5. A store-country slicer with Select All.
6. A country/state/city treemap with drill-down.
7. A weekly revenue column chart filtered to 1998.
8. A revenue-vs-target gauge showing the latest start of month.
9. Matrix-to-treemap interaction disabled.

These are assignment requirements. Validate the exact report state and refresh configuration in Power BI Desktop before presenting them as independently tested.
