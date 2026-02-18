
# PROJECT STRUCTURE

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

---

# data/raw_sales_data.csv

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

---

# modules/**init**.py

(Leave empty)

---

# modules/loader.py

```python
import pandas as pd

def load_data():
    """
    Loads raw sales data from CSV file.
    """
    df = pd.read_csv("data/raw_sales_data.csv")
    return df
```

---

# modules/cleaner.py

```python
import pandas as pd

def clean_data(df):
    """
    Cleans the raw dataset:
    - Removes duplicates
    - Removes negative quantity
    - Removes missing customers
    - Standardizes product and region casing
    - Converts date column to datetime
    """

    # Remove duplicate rows
    df = df.drop_duplicates()

    # Remove negative quantity rows
    df = df[df['quantity'] > 0]

    # Remove missing customers
    df = df.dropna(subset=['customer'])

    # Standardize product names
    df['product'] = df['product'].str.lower().str.strip()

    # Standardize region names
    df['region'] = df['region'].str.title().str.strip()

    # Convert order_date to datetime
    df['order_date'] = pd.to_datetime(df['order_date'], errors='coerce')

    # Drop rows where date conversion failed
    df = df.dropna(subset=['order_date'])

    return df
```

---

# modules/transformer.py

```python
def transform_data(df):
    """
    Feature Engineering:
    - Adds revenue column
    - Extracts month and year
    - Adds high_value flag
    """

    # Create revenue column
    df['revenue'] = df['quantity'] * df['price']

    # Extract month and year
    df['month'] = df['order_date'].dt.month
    df['year'] = df['order_date'].dt.year

    # High value order flag
    df['high_value'] = df['revenue'] > 50000

    return df
```

---

# modules/analyzer.py

```python
def generate_kpis(df):
    """
    Generates business KPIs:
    - Total revenue
    - Revenue by region
    - Top product
    - Total valid orders
    """

    total_revenue = df['revenue'].sum()

    revenue_by_region = (
        df.groupby('region')['revenue']
        .sum()
        .sort_values(ascending=False)
    )

    top_product = (
        df.groupby('product')['revenue']
        .sum()
        .idxmax()
    )

    total_orders = df['order_id'].nunique()

    return {
        "Total Revenue": total_revenue,
        "Revenue by Region": revenue_by_region.to_dict(),
        "Top Product": top_product,
        "Total Valid Orders": total_orders
    }
```

---

# main.py

```python
import os
from modules.loader import load_data
from modules.cleaner import clean_data
from modules.transformer import transform_data
from modules.analyzer import generate_kpis

def main():

    # Ensure output folder exists
    os.makedirs("output", exist_ok=True)

    # Step 1: Load raw data
    df = load_data()

    # Step 2: Clean data
    df = clean_data(df)

    # Step 3: Transform data
    df = transform_data(df)

    # Step 4: Generate KPIs
    kpis = generate_kpis(df)

    # Step 5: Save clean dataset
    df.to_csv("output/clean_sales_data.csv", index=False)

    # Step 6: Print KPIs
    print("\n=== Business KPIs ===\n")
    for key, value in kpis.items():
        print(f"{key}: {value}")

if __name__ == "__main__":
    main()
```

---


# EXPECTED TERMINAL OUTPUT

Example:

```
=== Business KPIs ===

Total Revenue: 385000
Revenue by Region: {'North': 275000, 'South': 60000, 'West': 30000}
Top Product: laptop
Total Valid Orders: 6
```

---

# OUTPUT GENERATED

Inside:

```
output/clean_sales_data.csv
```
