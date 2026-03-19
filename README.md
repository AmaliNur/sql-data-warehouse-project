# 🏗️ SQL Data Warehouse Project

A modern data warehouse built with SQL Server, implementing a **Medallion Architecture** (Bronze → Silver → Gold) with full ETL pipelines, data modelling, and quality checks.

---

## 📐 Architecture

```
Source Systems (CSV Files)
        │
        ▼
┌───────────────┐
│    BRONZE     │  Raw ingestion — no transformations
│  (Staging)    │
└───────┬───────┘
        │  Cleanse & standardize
        ▼
┌───────────────┐
│    SILVER     │  Cleaned, typed, deduplicated data
│  (Cleaned)    │
└───────┬───────┘
        │  Enrich & model
        ▼
┌───────────────┐
│     GOLD      │  Star schema — ready for analytics
│  (Analytics)  │
└───────────────┘
        │
        ▼
  Reports & Dashboards
```

**Gold Layer — Star Schema**
```
dim_customers ──┐
                ├──▶ fact_sales
dim_products  ──┘
```

---

## 🛠️ Tech Stack

| Tool | Purpose |
|---|---|
| SQL Server | Database engine |
| T-SQL | ETL logic, stored procedures, views |
| SSMS | Development & query execution |
| CSV Files | Source data (CRM & ERP systems) |

---

## 📁 Project Structure

```
├── datasets/          # Source CSV files (CRM & ERP)
├── docs/
│   └── data_catalog.md    # Column-level documentation for Gold layer
├── scripts/
│   ├── init_database.sql  # Creates DB and schemas
│   ├── bronze/            # DDL + load procedure for Bronze
│   ├── silver/            # DDL + load procedure for Silver
│   └── gold/              # Views for dimension & fact tables
└── tests/
    ├── quality_checks_silver.sql
    └── quality_checks_gold.sql
```

---

## 🚀 How to Run

> **Prerequisites:** SQL Server installed, SSMS or equivalent client, source CSV files available locally.

**1. Initialize the database**
```sql
-- Run: scripts/init_database.sql
-- Creates the DataWarehouse database and bronze/silver/gold schemas
```

**2. Create tables and views**
```sql
-- Run in order:
-- scripts/bronze/ddl_bronze.sql
-- scripts/silver/ddl_silver.sql
-- scripts/gold/ddl_gold.sql
```

**3. Load the data**
```sql
-- Update file paths in proc_load_bronze.sql to match your local setup, then:
EXEC bronze.load_bronze;
EXEC silver.load_silver;
-- Gold layer is view-based and updates automatically
```

**4. Validate**
```sql
-- Run quality checks to verify data integrity:
-- tests/quality_checks_silver.sql
-- tests/quality_checks_gold.sql
```

---

## 📊 Data Model

The Gold layer exposes three objects for analytics:

- **`gold.dim_customers`** — Customer details enriched with geographic and demographic data
- **`gold.dim_products`** — Product catalog with category hierarchy and pricing
- **`gold.fact_sales`** — Transactional sales records linked to both dimensions

See [`docs/data_catalog.md`](docs/data_catalog.md) for full column-level documentation.

---

## 📄 License

MIT © 2026 Amali Nuritdinov
