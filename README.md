# E-Commerce Intelligence Platform

**Applied Data Science · Data Preparation · Relational Data Analysis · Predictive Analytics**

**Status:** Work in progress — initial data exploration and relational data preparation.

## Overview

This project explores how transactional e-commerce data can be transformed into a structured analytical dataset suitable for reliable statistical analysis and, eventually, machine learning.

The work uses the **Brazilian E-Commerce Public Dataset by Olist**, which contains approximately 100,000 orders placed between 2016 and 2018. The dataset connects orders with products, sellers, customers, payments, deliveries, and other information across multiple relational tables.

The immediate focus is on **understanding the data model and preserving the correct unit of analysis**. In particular, order-level analysis requires care because one order may contain several items and payment records.

> **Current scope:** This repository is an evolving academic/personal data science project. The public code currently covers early exploratory analysis and an initial orders–products join. It does **not** yet provide a finished predictive model, an operational API, or validated performance results.

## Data source

**Dataset:** [Brazilian E-Commerce Public Dataset by Olist (Kaggle)](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)

The project currently reads six Olist CSV files:

| Table | Role |
|---|---|
| `olist_orders_dataset.csv` | Order identifiers, statuses, and purchase/delivery dates |
| `olist_order_items_dataset.csv` | Items within orders, product and seller keys, item prices |
| `olist_customers_dataset.csv` | Customer identifiers and geographic attributes |
| `olist_products_dataset.csv` | Product metadata |
| `olist_sellers_dataset.csv` | Seller identifiers and geographic attributes |
| `olist_order_payments_dataset.csv` | Payment information associated with orders |

Other Olist datasets, such as reviews and geolocation, may be considered in later stages but are **not currently integrated** in the published notebook.

The original Olist dataset is distributed separately under its own terms (listed on Kaggle as **CC BY-NC-SA 4.0**). Raw CSV files are **not included** in this repository.

## Research and engineering questions

- How should information from multiple relational tables be joined without introducing unintended duplicate rows?
- When should features be aggregated to the order level instead of joining raw item-level or payment-level records?
- Which data-quality checks are needed before using e-commerce transactions for statistical or predictive modeling?
- How could order, seller, customer, and delivery information eventually support interpretable predictive features?

## Current implementation

The notebook [`notebook/01_data_understanding.ipynb`](notebook/01_data_understanding.ipynb) currently includes:

1. Loading six Olist tables with **Pandas**.
2. Inspecting dataset dimensions, column types, and representative rows.
3. Checking missing values across the loaded tables.
4. Examining repeated `order_id` values in the order-items table, where multiple rows per order are expected.
5. Joining order items with product attributes using `product_id`.
6. Beginning order-level aggregation, including line-item counts and summed item prices.

The repository is still at an **exploration / early feature-construction stage**. Aggregation and subsequent modeling steps should not be regarded as finished or validated.

### Why the data granularity matters

The Olist tables have different natural units of observation. For example, the orders table has one record per order, whereas the order-items table may have multiple records for the same `order_id`.

Joining tables without checking those relationships can multiply rows and distort metrics such as revenue, counts, and outcome proportions. A central objective of this project is therefore to build features using explicit join keys, integrity checks, and appropriate group-by operations.

## Repository structure

```text
ecommerce-intelligence-platform/
├── README.md
├── requirements.txt                 # Currently a placeholder
├── .gitignore
└── notebook/
    └── 01_data_understanding.ipynb
```

## Running the existing notebook

The published notebook uses **Python**, **NumPy**, and **Pandas** in a Jupyter environment.

1. Download the Olist dataset from [Kaggle](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce).
2. Extract the relevant CSV files into a local directory, for example `data/raw/` inside the project.
3. Install the packages needed for the current notebook:

   ```bash
   python -m pip install numpy pandas jupyterlab
   ```

4. Open `notebook/01_data_understanding.ipynb` in JupyterLab.
5. **Update the CSV file paths before running it.** The current notebook contains absolute Windows paths from the original development environment and does not yet use portable relative paths.

   A portable alternative for a notebook inside `notebook/` (when Jupyter runs from that directory) is:

   ```python
   from pathlib import Path
   data_dir = Path("../data/raw")
   orders = pd.read_csv(data_dir / "olist_orders_dataset.csv")
   ```

   Relative paths depend on the working directory in which Jupyter executes the notebook.

**Note:** The notebook still contains an error output from an earlier execution of the aggregation cell. The saved cell text and the displayed historical traceback may not match. A clean end-to-end rerun has not yet been verified. The `requirements.txt` file is currently empty and will be completed as the project develops.

## Planned development

The following are **planned**, not completed features:

- Make data loading portable and document a reproducible environment.
- Define an order-level analytical table with verified one-to-many join and aggregation rules.
- Add data-quality and missingness checks before feature engineering.
- Engineer temporal, product, customer, seller, and delivery features.
- Explore delivery-related prediction tasks and establish a transparent train/test evaluation protocol.
- Compare appropriate baseline models and report uncertainty, interpretability, and limitations.
- Improve documentation and tests as the implementation matures.

## Limitations

- No trained or validated predictive model is currently published.
- The public repository contains only the initial notebook, not a completed end-to-end pipeline.
- Input files must be downloaded separately from the original dataset provider.
- Existing exploratory outputs should not be interpreted as final analytical conclusions.
- Claims about business or operational impact would require further modeling and evaluation.

## Author and attribution

**Marouane Zemrani**  
Final-year engineering student, Data Science specialization  
EMINES — Mohammed VI Polytechnic University (UM6P), Morocco

**Dataset acknowledgment:** Olist, *Brazilian E-Commerce Public Dataset by Olist* ([source](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)).
