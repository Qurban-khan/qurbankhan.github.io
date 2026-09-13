# PakFresh Consumer Products Ltd. — Supply Chain Analytics Portfolio

![Dashboard screenshot](screenshots/project1_dashboard.jpg)

A synthetic FMCG supply-chain dataset and three end-to-end Power BI analytics
projects — built to demonstrate supply chain analysis, data modeling, DAX,
and business judgment on realistic, causally-consistent operational data.

**Note:** PakFresh Consumer Products Ltd. is a fictional company. All data is
synthetic, generated via simulation — modeled on realistic FMCG supply-chain
operations, not sourced from any real business.

---

## Why simulated data, and why this isn't just random numbers

Real company data isn't available for portfolio use — it's confidential. The
alternative most people reach for is randomly generated numbers, which
produces a dataset with no internal logic: sales, inventory, and purchase
orders that don't actually relate to each other the way they would in a real
business.

Instead, this dataset was built as a **simulation with real cause and
effect**: customer demand is generated first, then constrained by actual
inventory availability, which triggers real replenishment logic, which
depends on simulated supplier lead-time behavior — so a stockout in the data
can be traced all the way back through inventory position, to a specific
purchase order, to a specific supplier's delivery performance. That
traceability is what makes the analysis defensible rather than arbitrary.

**Company profile:** PakFresh Consumer Products Ltd., a mid-sized Pakistani
FMCG manufacturer/distributor. 400 SKUs across 6 categories, 40 suppliers,
750 customers, 4 warehouses, 4 sales channels. Analysis period: January 2024
– December 2026.

---

## Data model

Star schema — 6 dimensions, 5 fact tables.

| Table | Grain | Rows |
|---|---|---|
| FactSales | one order line (SKU × customer × date) | 322,013 |
| FactInventory | SKU × warehouse × week | 251,200 |
| FactPurchaseOrders | SKU × warehouse × replenishment event | 79,560 |
| FactPromotions | one promotion (SKU × channel × date range) | 4,252 |
| FactForecast | SKU × month | 13,845 |

Dimensions: DimDate, DimProduct, DimSupplier, DimCustomer, DimWarehouse,
DimChannel.

Full relationship map, known modeling caveats (date-key alignment, the
purchase-order delivery-date edge case at year-end, the monthly forecast
grain needing its own date table), and referential-integrity test results
are documented in [`/docs/data-model.md`](docs/data-model.md).

---

## Project 1: Inventory Intelligence & SKU Optimization

**Question:** where is inventory capital tied up, which SKUs drive revenue,
where are stockouts happening, and which SKUs need attention.

**Findings:**
- Revenue is less concentrated than the textbook 80/20 rule — **43% of SKUs
  generate 80% of profit**, not the usual ~20%. Risk and reward here are
  spread across a wider base of the catalog than a typical Pareto
  distribution would suggest.
- Inventory turns over **30.6x/year (11.95 days on hand)** — lean, well
  above typical FMCG benchmarks (8-15x) — paired with a **4.6% stockout
  rate** against a **95.9% fill rate**. A real trade-off, not free
  efficiency.
- Slow/Very-Slow-moving SKUs hold **23.7% of inventory value**, but only
  **1 SKU out of 400** crosses the excess-inventory threshold — the
  inventory problem here is specific and targeted, not systemic.
- Cross-referencing ABC (value) and XYZ (demand variability) classification
  surfaces a specific list of high-value-or-moderate, erratic-demand SKUs
  with above-average stockout rates — the actual priority list for planning
  attention, distinct from simply "the top revenue SKUs."

**Method notes:** ABC/XYZ cutoffs were set from this dataset's actual
revenue-concentration and CV distribution, not copied from textbook
thresholds. Near-expiry/expired inventory analysis was intentionally
excluded — the dataset tracks aggregate closing inventory, not batch/lot
level detail, so that metric would have been fabricated rather than derived.

---

## Project 2: Supplier & Procurement Performance

**Question:** which suppliers are reliable, which create service or quality
problems, and where is procurement cost leaking.

*(status: in progress)*

---

## Project 3: Demand Planning & Inventory Optimization

**Question:** how accurately can demand be forecast, and how should
replenishment quantities and safety stock be set.

*(status: in progress)*

---

## Tools

- **Data generation:** Python (pandas, numpy) — full simulation methodology
  documented in [`/docs/data-generation.md`](docs/data-generation.md)
- **Modeling & analysis:** Power BI Desktop (Power Query, star schema, DAX)

## Repo contents

- `/screenshots` — dashboard pages
- `/pbix` — Power BI file
- `/data` — sample data (small excerpt; full dataset generation is
  reproducible from the methodology doc, not included in full here due to
  size)
- `/docs` — data model, data generation methodology, and per-project
  write-ups

---

*Built as a portfolio project to demonstrate supply chain analytics,
Power BI, and DAX skills for Supply Chain Analyst / BI Analyst roles.*
