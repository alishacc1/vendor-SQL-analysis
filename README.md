# Vendor SQL Analysis

A comprehensive data analysis project for vendor performance evaluation using SQL queries, Python data processing, and Jupyter notebooks. This project ingests vendor data from CSV files into a SQLite database, performs analytical queries, and generates actionable insights about vendor performance.

## Project Overview

This project processes and analyzes vendor data across multiple dimensions including:
- **Purchases**: Vendor purchase transactions and pricing
- **Sales**: Sales performance by vendor and product brand
- **Invoicing**: Vendor invoice and freight cost tracking
- **Performance Metrics**: Profitability, stock turnover, and sales ratios

## Project Structure

```
Vendor-SQL-Analysis/
├── ingestion_db.py                    # Data ingestion script - loads CSVs into SQLite
├── get_vendor_summary.py              # Vendor summary generation and analysis
├── exploratory_data_analysis.ipynb    # EDA notebook with data exploration
├── vendor_performance_analysis.ipynb  # Performance metrics and analysis
├── logs/                              # Application logs
├── data/                              # Input CSV files (not included)
└── inventory.db                       # SQLite database (generated)
```

## Core Components

### 1. **ingestion_db.py**
Loads raw CSV files from the `data/` directory into a SQLite database.

**Features:**
- Scans `data/` directory for CSV files
- Converts each CSV to a DataFrame
- Ingests data into SQLite tables using the filename (without extension) as the table name
- Includes logging for tracking ingestion process
- Timing metrics for performance monitoring

**Usage:**
```bash
python ingestion_db.py
```

### 2. **get_vendor_summary.py**
Creates comprehensive vendor summary tables by merging purchase, sales, and freight data.

**Key Functions:**

- **`create_vendor_summary(conn)`**: Generates a complete vendor summary using complex SQL CTEs (Common Table Expressions):
  - Aggregates freight costs by vendor
  - Summarizes purchases with pricing and volumes
  - Summarizes sales quantities and dollars
  - Joins all three summaries on vendor and brand
  
- **`clean_data(df)`**: Data cleaning and feature engineering:
  - Converts data types (Volume to float)
  - Fills missing values with 0
  - Strips whitespace from text columns
  - Creates calculated metrics:
    - `GrossProfit`: Sales dollars minus purchase dollars
    - `ProfitMargin`: Profit as percentage of sales
    - `StockTurnover`: Sales quantity divided by purchase quantity
    - `SalesToPurchaseRatio`: Sales dollars to purchase dollars ratio

- **`ingest_db(df, table_name, engine)`**: Ingests DataFrames into database tables

**Usage:**
```bash
python get_vendor_summary.py
```

Creates a `vendor_sales_summary` table with comprehensive vendor analytics.

### 3. **exploratory_data_analysis.ipynb**
Jupyter notebook for exploratory data analysis containing:
- Data loading and inspection
- Statistical summaries
- Distribution analysis
- Data quality checks
- Visualization of key metrics

### 4. **vendor_performance_analysis.ipynb**
Comprehensive performance analysis notebook featuring:
- Vendor performance rankings
- Brand-wise analysis
- Profitability metrics
- Sales trends and patterns
- Stock turnover analysis
- Visual dashboards and comparisons

## Database Schema

The project uses SQLite with the following key tables:

| Table Name | Source | Purpose |
|-----------|--------|---------|
| `purchases` | CSV input | Purchase transactions with vendor, brand, and pricing data |
| `purchase_prices` | CSV input | Pricing and volume reference data |
| `sales` | CSV input | Sales transactions by vendor and brand |
| `vendor_invoice` | CSV input | Invoice records including freight costs |
| `vendor_sales_summary` | Generated | Comprehensive vendor performance metrics |

## Key Metrics & KPIs

The analysis generates the following performance indicators:

- **Gross Profit**: Total sales dollars minus total purchase dollars
- **Profit Margin**: Profitability as a percentage of sales
- **Stock Turnover**: How many times inventory is sold and replaced
- **Sales-to-Purchase Ratio**: Efficiency of converting purchases to sales
- **Freight Cost Analysis**: Distribution and impact of shipping costs

## Prerequisites

- Python 3.7+
- Required libraries:
  - `pandas`
  - `sqlalchemy`
  - `sqlite3` (included with Python)
  - `jupyter` (for running notebooks)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/SansKarI2004/Vendor-SQL-Analysis.git
cd Vendor-SQL-Analysis
```

2. Install required packages:
```bash
pip install pandas sqlalchemy jupyter
```

3. Create a `data/` directory and add your CSV files:
```bash
mkdir data
# Add your vendor-related CSV files to the data/ directory
```

## Usage Workflow

1. **Data Ingestion**: Load CSV files into database
   ```bash
   python ingestion_db.py
   ```

2. **Generate Summary**: Create vendor summary table with cleaned data and metrics
   ```bash
   python get_vendor_summary.py
   ```

3. **Exploratory Analysis**: Open and run the EDA notebook
   ```bash
   jupyter notebook exploratory_data_analysis.ipynb
   ```

4. **Performance Analysis**: Review the vendor performance analysis notebook
   ```bash
   jupyter notebook vendor_performance_analysis.ipynb
   ```

## Logging

Application logs are stored in the `logs/` directory:
- `ingestion_db.log`: Data ingestion process logs
- `get_vendor_summary.log`: Vendor summary generation logs

Logs include timestamps, log levels (DEBUG, INFO), and detailed messages for debugging and monitoring.

## Output

After running the pipeline, the `inventory.db` SQLite database will contain:
- Original data tables from CSV imports
- `vendor_sales_summary`: Main analysis table with computed metrics

This summary table can be exported to CSV or visualized using the Jupyter notebooks.

## Contributing

Feel free to submit issues and enhancement requests!

## License

This project is open source and available under the MIT License.

## Author

alisha waghmare

---

**Last Updated**: November 2024
