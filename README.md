# Ticket Sales Data Engineering Pipeline

## Overview
An automated data cleaning, schema enforcement, and analytical ingestion pipeline built to clean a transactional dataset (~2,500 rows) covering ticket sales from March to May 2026.

## Data Quality Audit Log (Журнал проблем)

| # | Identified Issue | Root Cause Analysis | Applied Engineering Resolution |
|---|---|---|---|
| 1 | Duplicate Records | Multi-pass transmission artifacts | Removed 39 redundant rows via `.drop_duplicates()` |
| 2 | Malformed Columns | Header spacing variations (` Ticket ID`) | Applied `.str.strip().str.lower()` and converted to `snake_case` |
| 3 | Casing Discrepancies | Fragmented user string inputs (`Konsert`, `KONSERT`) | Standardized categorical groups using uniform `.str.lower()` bounds |
| 4 | Name Fragmentation | Punctuation differences (`Sevinch Mo'minova` vs `sevinch mominova`) | Erased string apostrophes and trailing spaces via Regex |
| 5 | Currency Pollution | Text values embedded in numbers (`451 283 so'm`) | Extracted digits with `r"[^\d]"` and cast column to numeric types |
| 6 | Inverted Quantities | Negative sales counts entered (`-3`) | Converted numeric values to logical entries via `.abs()` calculation |
| 7 | Mixed Timestamps | Variable date templates (`YYYY-MM-DD` and `DD.MM.YYYY`) | Parsed date sequences systematically using `pd.to_datetime(..., format="mixed")` |
| 8 | Status Variance | Non-uniform statuses (`YAKUNLANGAN` vs `yakunlangan`) | Normalized state spaces via lowercasing string attributes |

## Core Analytical Findings

### 1. Revenue by Event Type (Decreasing Order)
* **konsert**: 945,782,973 UZS
* **futbol**: 291,647,940 UZS
* **stendap**: 153,214,918 UZS

### 2. Top 5 Artists by Rating
* **muhabbat shamayeva**: 4.28
* **ulugbek rahmatullayev**: 4.27
* **doniyor otajonov**: 4.26
* **jasur umirov**: 4.26
* **navruz gala shou**: 4.25

## Pipeline Architecture
```text
├── .gitignore
├── README.md
├── requirements.txt
├── data/
│   └── concert_tickets.csv
│   └── concert_tickets_cleaned.csv
└── notebooks/
    └── ticket_sales_cleaning.ipynb
```

## Future Roadmap

- [ ] **Code Refactoring & Readability:** Modularize the cleaning pipeline into explicit design patterns, enhance inline documentation, and strictly enforce PEP 8 compliance.
- [ ] **Advanced Data Visualization:** Integrate modern visualization libraries (such as Seaborn and Matplotlib) to construct interactive distribution and revenue performance dashboards.
