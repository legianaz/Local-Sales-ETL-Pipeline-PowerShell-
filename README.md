# Local Sales ETL Pipeline (PowerShell)

A 6-stage ETL pipeline that turns a raw sales CSV into a self-contained HTML
dashboard plus summary CSVs. Everything runs locally. No network calls, no
external tools, no database. The source file is never modified.

## Stages
1. Extract   - read the source, check it has rows and columns
2. Clean     - trim spaces, fill blanks, fix formats, drop empty rows, dedupe
3. Validate  - quality gate; stops the pipeline on bad data with a clear reason
4. Transform - derive the amount, parse dates, reshape to clean records
5. Aggregate - totals, averages, top 10 products, top 10 customers, by month/day
6. Load      - build the HTML dashboard and copy final CSVs to the output folder

## Run it
```powershell
.\run_pipeline.ps1 -InputPath "PUT YOUR CSV PATH HERE"
```

If the auto-detection guesses a column wrong, name them:
```powershell
.\run_pipeline.ps1 -InputPath "PUT YOUR CSV PATH HERE" `
    -AmountColumn "Revenue" -ProductColumn "Item" -CustomerColumn "Client" -DateColumn "InvoiceDate" `
    -DateColumns "InvoiceDate" -NumberColumns "Revenue" -NameColumns "Client"
```

## Output
A timestamped folder containing:
- sales_report.html      the dashboard (double-click to open)
- summary.csv, top_products.csv, top_customers.csv, sales_by_month.csv, sales_by_day.csv
- run.log                full timestamped audit trail
- pipeline_work\         the numbered file from every stage, for inspection

## Governance notes
- No data leaves the machine. All work stays in the output folder.
- The source file is read-only to the pipeline.
- Every step is written to disk and logged, so the whole run is auditable.
- If validation fails, the pipeline stops and produces no misleading report.
  Fix the source file and run again.

## Requirements
PowerShell 5.1 (built into Windows 10/11) or PowerShell 7+. Nothing to install.
