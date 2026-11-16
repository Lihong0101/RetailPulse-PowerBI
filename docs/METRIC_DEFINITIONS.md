# Metric definitions and interpretation

The following formulas were supplied from the author's Power BI Desktop project. Their semantics are explained with the source data dictionary and QA checks.

| Measure | DAX / meaning | Caution |
|---|---|---|
| Total Revenue | `SUM(FactSales[NetRevenue])` | After line discounts, **before refunds**; `NetRevenue` is a prepared fact column |
| Total COGS | `CALCULATE(SUMX(FactSales, FactSales[Quantity] * FactSales[UnitCostEUR]), NOT ISBLANK(FactSales[UnitPriceEUR]))` | Excludes rows with null sale price from sold cost; not the cost of *all* sold units |
| Total Profit | `[Total Revenue] - [Total COGS]` | Gross profit before refunds, **not** operating or net profit |
| Adjusted Revenue | `[Total Revenue] - [Refund Amount]` | Order-date-cohort refund adjusted sales revenue |
| Units Sold | `SUM(FactSales[Quantity])` | Includes rows whose sale price is missing |
| Return Rate % | `DIVIDE([Returned Units], [Units Sold])` | Unit-level observed return rate, not an order-level return rate |
| Adjusted Profit | DAX text not supplied in QA conversation | Expected to reverse returned inventory cost; verify actual measure before publishing formal reconciliations |

## 2026 Jan–Oct reconciliation

- Sales revenue €975,771.81 − refunds €69,404.42 = refund-adjusted revenue €906,367.39.
- Gross profit €288,195.50 / revenue €975,771.81 = 29.5% (rounded).
- Returned units 878 / sold units 13,089 = 6.71% (rounded).
- Adjusted profit €268,206.01. Difference versus pre-return gross profit ≈ €19,989.49; use the actual Adjusted Profit DAX to attribute this reversal correctly.

## Period handling

- Report compares **Jan–Oct 2026** with **Jan–Oct 2025** where applicable.
- `FactReturns[OrderDate]` is related actively to `DimDate`, supporting sale-cohort return measures.
- `FactReturns[ReturnDate]` uses an inactive date relationship for the refund-timing comparison via `USERELATIONSHIP`.
- Returns after the recorded observation window are not captured. Recent cohorts have right-censoring.

## Not a formal financial statement

These measures are analytical approximations built for a synthetic BI exercise. They are not GAAP/IFRS revenue-recognition or inventory accounting results. A precise attribution of gross margin movements requires controlled price/cost/mix analysis; descriptive association does not establish causality.
