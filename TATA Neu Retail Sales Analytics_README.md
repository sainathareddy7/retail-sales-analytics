
# TATA Neu Retail Sales Analytics (Pandas CLI)

This repository provides a clean, reusable **Python script** that reproduces and extends the analyses from your retail sales assignment (two CSVs joined by `OrderID`). It exposes a command‑line interface (CLI) to compute totals, category/subcategory summaries, top products/customers, geo and shipping analyses, ranking, and a consolidated Markdown report.

The project expects two input files derived from the original notebook:

- `TATA_TB1.csv` – orders & customer geography (`OrderID`, `OrderDate`, `CustomerName`, `City`, `Country`, `State`, `Region`, `Segment`, `ShipDate`, `ShipMode`, `lon`, `lat`, ...)
- `TATA_TB2.csv` – line items & amounts (`OrderID`, `ProductName`, `Discount`, `Sales`, `Profit`, `OrderQuantity`, `Category`, `SubCategory`)

> All columns above are inferred from the uploaded notebook export and mirrored here so your script works out‑of‑the‑box with the same data.  

## Quickstart
```bash
# 1) Optional: virtual environment
python -m venv .venv
# macOS/Linux
. .venv/bin/activate
# Windows
# .venv\Scripts\activate

# 2) Install
pip install pandas

# 3) Run a full report (CSV + Markdown under ./output)
python retail_sales_analytics.py --input-dir . --export --output-dir ./output report

# 4) Examples
python retail_sales_analytics.py totals
python retail_sales_analytics.py category-summaries
python retail_sales_analytics.py subcategory-filter --min-sales 100000
python retail_sales_analytics.py top-products --n 10
python retail_sales_analytics.py top-customers --n 10
python retail_sales_analytics.py region-segment
python retail_sales_analytics.py geo-summary
python retail_sales_analytics.py shipmode
python retail_sales_analytics.py rank-customer --rank 7
python retail_sales_analytics.py favorites-per-customer --k 3
```

## CLI
```text
python retail_sales_analytics.py [-h]
  [--input-dir INPUT_DIR] [--tb1 TB1] [--tb2 TB2]
  [--export] [--output-dir OUTPUT_DIR]
  {totals,date-range,category-summaries,subcategory-filter,top-products,top-customers,region-segment,geo-summary,customer-sales-threshold,city-sales-threshold,shipmode,rank-customer,favorites-per-customer,report}
  ...
```

### Commands
- `totals` – dataset sizes, distinct entities, total sales/profit/order qty
- `date-range` – oldest & latest `OrderDate`
- `category-summaries` – totals per `Category` and per (`Category`,`SubCategory`)
- `subcategory-filter` – subcategories with `total_sales > min-sales`
- `top-products` – Top **N** products by sales (with quantities)
- `top-customers` – Top **N** customers by total sales & orders
- `region-segment` – totals by `Region`×`Segment`
- `geo-summary` – totals by `Country`→`State`→`City`
- `customer-sales-threshold` – customers with sales `> X`
- `city-sales-threshold` – cities with sales `> X`
- `shipmode` – totals by `ShipMode` (sales asc, profit desc)
- `rank-customer` – customer at a given dense rank by total sales
- `favorites-per-customer` – top **K** products per customer by sales
- `report` – runs a default set, exports CSV/Markdown, and builds `REPORT.md`

## Data Handling
- Column names normalized to `snake_case`
- `OrderDate` parsed to datetime; new `year` (yr) derived
- Numeric columns converted with `errors='coerce'`

## License
MIT
