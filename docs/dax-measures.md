# DAX measures and calculated columns

The formulas below are **reference patterns** based on the supplied assignment. Check the exact table/column names in the PBIX and adjust them if needed. These formulas are not a claim that the existing PBIX uses identical expressions.

Assumptions used in examples:
- Fact table: `'Transactions'`
- Returns table: `'Returns'`
- Product dimension: `'Products'`
- Date dimension: `'Calendar'` with a date column named `[date]`
- Both fact tables have a `[quantity]` column
- Relationships from dimensions to fact tables are configured as described in the model documentation

## Measures

```DAX
Quantity Sold =
SUM ( 'Transactions'[quantity] )
```

```DAX
Quantity Returned =
SUM ( 'Returns'[quantity] )
```

```DAX
Total Transactions =
COUNTROWS ( 'Transactions' )
```

```DAX
Total Returns =
COUNTROWS ( 'Returns' )
```

```DAX
Return Rate =
DIVIDE ( [Quantity Returned], [Quantity Sold] )
```

Format as percentage.

```DAX
Total Revenue =
SUMX (
    'Transactions',
    'Transactions'[quantity] * RELATED ( 'Products'[product retail price] )
)
```

```DAX
Total Cost =
SUMX (
    'Transactions',
    'Transactions'[quantity] * RELATED ( 'Products'[product cost] )
)
```

```DAX
Total Profit =
[Total Revenue] - [Total Cost]
```

```DAX
Profit Margin =
DIVIDE ( [Total Profit], [Total Revenue] )
```

Format as percentage.

```DAX
Last Month Transactions =
CALCULATE (
    [Total Transactions],
    DATEADD ( 'Calendar'[date], -1, MONTH )
)
```

```DAX
Last Month Revenue =
CALCULATE (
    [Total Revenue],
    DATEADD ( 'Calendar'[date], -1, MONTH )
)
```

```DAX
Last Month Profit =
CALCULATE (
    [Total Profit],
    DATEADD ( 'Calendar'[date], -1, MONTH )
)
```

```DAX
Last Month Returns =
CALCULATE (
    [Total Returns],
    DATEADD ( 'Calendar'[date], -1, MONTH )
)
```

```DAX
Revenue Target =
[Last Month Revenue] * 1.05
```

Format monetary measures as Currency ($ English). `DATEADD` time intelligence requires a correctly configured date table and a suitable date filter context. The exact behavior of “previous month” depends on the visual's date context.

## Calculated columns

### Calendar[Weekend]

```DAX
Weekend =
IF (
    WEEKDAY ( 'Calendar'[date], 2 ) > 5,
    "Y",
    "N"
)
```

With return type 2, Monday = 1 and Sunday = 7.

### Calendar[End of Month]

```DAX
End of Month =
EOMONTH ( 'Calendar'[date], 0 )
```

Set the data type/format to Date.

### Customers[Short Country]

```DAX
Short Country =
UPPER ( LEFT ( 'Customers'[customer country], 3 ) )
```

### Stores[Years Since Remodel]

```DAX
Years Since Remodel =
DATEDIFF ( 'Stores'[store remodel date], TODAY (), YEAR )
```

Replace `[store remodel date]` with the exact remodel-date field name in the model. `DATEDIFF` counts year boundaries; it does not necessarily represent completed anniversaries.

## Formatting checklist

- `Return Rate`, `Profit Margin`: Percentage.
- `Total Revenue`, `Total Cost`, `Total Profit`, `Revenue Target`, and previous-month monetary measures: Currency ($ English).
- Quantity and count measures: Whole number.
- Review blank/zero handling and filter context in Power BI before using these measures in a published analysis.
