# Final QA and deployment validation log

**Date:** 2026-11-16 | **Dataset:** synthetic RetailPulse starter kit

## Source integrity

- 22 monthly sales CSVs, 2025-01 to 2026-10.
- Original sales rows: 21,105; after deduplication of five duplicate LineIDs: **21,100**.
- 22 sale-price entries marked N/A retained as null (do not silently remove).
- 1,491 records in `returns.csv`; unique ReturnID count of 1,491 observed during source analysis.
- Desktop Power Query preview successfully displayed Sales_Raw after paths were repaired following local re-download.

## Validated totals (Jan–Oct 2026)

| Market | Total Revenue | Units Sold | Refund Amount | Adjusted Revenue | Total Profit | Adjusted Profit | Gross Margin % | Return Rate % |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| France | €387,375.07 | 5,196 | €28,752.82 | €358,622.25 | €116,165.09 | €107,650.73 | 30.0% | 6.95% |
| Germany | €299,935.67 | 4,002 | €18,737.08 | €281,198.59 | €88,355.56 | €83,097.26 | 29.5% | 5.72% |
| Italy | €120,437.48 | 1,615 | €7,257.90 | €113,179.58 | €35,397.21 | €33,364.80 | 29.4% | 5.88% |
| Spain | €168,023.58 | 2,276 | €14,656.62 | €153,366.96 | €48,277.63 | €44,093.21 | 28.7% | 8.48% |
| Total | €975,771.81 | 13,089 | €69,404.42 | €906,367.39 | €288,195.50 | €268,206.01 | 29.5% | 6.71% |

## Service / security / refresh

- Service report was published and displayed with market slicer and at least the executive dynamic insight.
- RLS `France_Manager` and `Germany_Manager` roles appeared in Service and role simulation was reported successful.
- Three local file credentials previously showed connection errors, later no warning in settings.
- **On-demand refresh: Completed** — start `2026-11-16 18:53:25`; end `2026-11-16 18:53:37` (as displayed in the Service refresh history).
- Weekly scheduled refresh configuration was suggested; **not independently confirmed completed**.

## Final pre-publication checks (still recommended)

- [ ] Re-export all four dashboard screenshots after latest deduplication; ensure market selection and subtitles match.
- [ ] Confirm all four dynamic insight measures in each market with no hard-coded France text.
- [ ] Check `Adjusted Profit` DAX and returned-cost reversal against underlying `ReturnedCOGSEUR`.
- [ ] Hide backup and support pages from report navigation as appropriate.
- [ ] Verify final PBIX you distribute is the newest **locally saved and refreshed** version, not the earlier QA upload.
- [ ] Confirm whether scheduled refresh is enabled and whether a **scheduled** (not on-demand) run succeeds, before claiming it.
- [ ] If including a link to Power BI Service, confirm sharing permissions and never publish RLS-protected content using Publish to web.
