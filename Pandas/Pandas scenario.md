## **Retail Revenue Integrity & Reporting Pipeline**

You are a Junior Data Engineer in a mid-sized retail company operating across multiple regions.

The CFO urgently emails your team:

> “The revenue numbers in our dashboard do not match finance reports.
> I need verified revenue by region and top-performing products within 2 hours.
> Use the exported system data attached.”

You receive a raw CSV export generated from multiple store systems.

The systems were not standardized.

Your responsibility:

Design and implement a **data engineering pipeline** that:

* Cleans the dataset
* Validates business logic
* Engineers required fields
* Generates accurate KPIs
* Outputs a dashboard-ready dataset

---

# 📄 DATASET

```
order_id,order_date,customer,product,quantity,price,region
1001,01-02-24,Alice,Laptop,1,55000,north
1002,2024/02/01,Bob,laptop,2,55000,North
1003,02/01/2024,Charlie,Phone,-1,20000,SOUTH
1003,02/01/2024,Charlie,Phone,-1,20000,SOUTH
1004,2024-02-03,,Tablet,1,15000,West
1005,2024-02-04,David,Laptop,1,55000,NORTH
1006,03-02-2024,Eva,Phone,3,20000,south
1007,2024/02/05,Frank,Tablet,1,15000,West
1008,2024-02-06,Gina,Laptop,2,55000,North
```

This dataset includes:

* Duplicate rows
* Negative quantity
* Missing customer
* Inconsistent casing
* Mixed date formats
* Revenue distortions

---

# 📁 PROJECT STRUCTURE 

```
pandas_data_pipeline/
│
├── main.py
├── data/
│   └── raw_sales_data.csv
│
├── modules/
│   ├── __init__.py
│   ├── loader.py
│   ├── cleaner.py
│   ├── transformer.py
│   └── analyzer.py
│
└── output/
```

# 🎯 FINAL OUTCOMES EXPECTED

## 1️⃣ Clean Dataset

File:

```
output/clean_sales_data.csv
```

This file must:

* Contain no duplicates
* Contain no negative quantities
* Contain no missing customers
* Have standardized region names
* Have standardized product names
* Have properly formatted datetime column
* Include new engineered fields:

  * revenue
  * month
  * year
  * high_value flag

---

## 2️⃣ Verified Business KPIs

The program must print:

* Total Revenue
* Revenue by Region
* Top Product
* Total Valid Orders
