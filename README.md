# Microsoft Fabric Sales Analytics Project

## Project Overview

This project demonstrates an end-to-end analytics workflow using Microsoft Fabric and Power BI.
The goal was to build a simple sales analytics solution using a Lakehouse architecture, data transformation notebooks, SQL querying, dimensional modelling, and Power BI reporting.

The project uses the Sample Superstore dataset and follows modern data warehousing concepts including:

* Lakehouse architecture
* Star schema modelling
* Fact and dimension tables
* Surrogate keys
* Delta tables
* Power BI semantic modelling

---

# Architecture

```plaintext id="u1xv7e"
Raw CSV File
    ↓
Microsoft Fabric Lakehouse
    ↓
Fabric Notebook (Python/Pandas/Spark)
    ↓
Delta Tables
    ↓
Star Schema
    ↓
Power BI Semantic Model
    ↓
Power BI Report
```

---

# Technologies Used

* Microsoft Fabric
* Lakehouse
* Fabric Notebooks
* Python
* Pandas
* PySpark
* SQL
* Delta Lake
* Power BI
* DAX

---

# Step-by-Step Implementation

# 1. Created Microsoft Fabric Workspace

A new Fabric workspace was created to store all project assets.

Workspace:

```plaintext id="jlwmn8"
sales-fabric-project
```

---

# 2. Created Lakehouse

Inside the workspace, a new Lakehouse was created.

Lakehouse:

```plaintext id="wjglmo"
sales_lakehouse
```

The Lakehouse was used to store:

* raw files
* transformed tables
* Delta tables

---

# 3. Uploaded Raw Dataset

The Sample Superstore CSV dataset was uploaded into:

```plaintext id="49h90m"
Lakehouse → Files
```

This represents the:

# Bronze Layer

(raw/unprocessed data)

---

# 4. Created Fabric Notebook

A Fabric notebook was created and connected to the Lakehouse.

Notebook:

```plaintext id="m1g6z4"
sales_notebook
```

The Lakehouse was attached using:

```plaintext id="jlwmnd"
Add Lakehouse
```

---

# 5. Loaded CSV Data Using Python

The CSV file was loaded into a Pandas DataFrame.

```python id="jlwmni"
import pandas as pd

df = pd.read_csv(
    "Files/Sample - Superstore.csv",
    encoding="latin1"
)
```

---

# 6. Data Cleaning and Transformation

Basic transformations were performed:

* removed null values
* converted dates
* created Year column
* standardized column names

```python id="jlwmno"
df = df.dropna()

df['Order_Date'] = pd.to_datetime(df['Order_Date'])

df['Year'] = df['Order_Date'].dt.year

df.columns = (
    df.columns
      .str.strip()
      .str.replace(" ", "_")
)
```

This represents the:

# Silver Layer

(cleaned/transformed data)

---

# 7. Created Clean Delta Table

The cleaned dataset was converted into a Spark DataFrame and stored as a managed Delta table.

```python id="jlwmnt"
df_spark = spark.createDataFrame(df)

df_spark.write \
    .mode("overwrite") \
    .saveAsTable("clean_sales")
```

This allowed:

* SQL querying
* Power BI integration
* semantic model creation

---

# 8. Built Star Schema

The dataset was transformed into:

* fact table
* dimension tables

Dimension tables:

* dim_customer
* dim_product
* dim_date

Fact table:

* fact_sales

---

# 9. Created Surrogate Key for Product Dimension

A duplicate Product_ID issue was discovered in the source data.

To maintain one-to-many relationship integrity, a surrogate key was introduced.

```python id="jlwmnz"
dim_product['Product_Key'] = dim_product.index + 1
```

The surrogate key was then added to the fact table.

---

# 10. Saved Dimension and Fact Tables

Dimension and fact tables were stored as managed Delta tables.

```python id="jlwmo4"
dim_product_spark.write \
    .mode("overwrite") \
    .option("overwriteSchema", "true") \
    .saveAsTable("dim_product")
```

Schema overwrite was required because the table structure evolved during development.

---

# 11. Created Power BI Semantic Model

Power BI was connected to the Fabric semantic model using:

```plaintext id="jlwmo8"
Power BI semantic models
```

The following relationships were created:

```plaintext id="jlwmod"
fact_sales (*) → dim_customer (1)
fact_sales (*) → dim_product (1)
fact_sales (*) → dim_date (1)
```

This created a proper:

# Star Schema

---

# 12. Created DAX Measures

Example DAX measure:

```DAX id="jlwmoh"
Total Sales = SUM(fact_sales[Sales])
```

---

# 13. Built Power BI Report

The report included:

* KPI card for Total Sales
* Sales by Category bar chart
* Sales trend line chart
* Product performance table
* Region slicer

---

# Key Concepts Demonstrated

## Lakehouse Architecture

Combined file-based storage and structured analytics in Microsoft Fabric.

---

## Star Schema

Separated:

* facts (transactions)
* dimensions (descriptive attributes)

to optimize reporting and analytics.

---

## Surrogate Keys

Implemented surrogate keys to handle duplicate business keys in the product dimension.

---

## Delta Lake

Used managed Delta tables for:

* schema enforcement
* SQL access
* Power BI integration

---

## Direct Lake

Power BI connected directly to Fabric using Direct Lake mode.

---

# Challenges Encountered

## File Path Issues

Resolved notebook file access problems by attaching the Lakehouse and using Fabric file paths.

---

## Invalid Column Names

Spark/Delta Lake rejected spaces in column names.
Columns were standardized using snake_case formatting.

---

## Duplicate Business Keys

Duplicate Product_ID values were discovered in the source data and resolved using surrogate keys.

---

## Schema Evolution

Delta Lake schema mismatch errors occurred after adding new columns.
Resolved using:

```python id="jlwmom"
.option("overwriteSchema", "true")
```

---

# Outcomes

This project demonstrated:

* Microsoft Fabric fundamentals
* notebook-based transformations
* SQL analytics
* dimensional modelling
* Delta Lake concepts
* Power BI semantic modelling
* dashboard/report development

---

# Future Improvements

Possible future enhancements:

* Medallion architecture implementation
* incremental refresh
* Fabric pipelines
* Dataflows Gen2
* automated ETL orchestration
* advanced DAX measures
* row-level security (RLS)

---

# Learning Summary

This project provided hands-on experience with:

* Microsoft Fabric
* Lakehouse architecture
* Power BI
* Spark and Delta Lake
* data warehousing fundamentals
* star schema modelling
* semantic modelling
* business intelligence reporting
