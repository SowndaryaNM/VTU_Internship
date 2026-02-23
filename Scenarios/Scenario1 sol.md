# Project Structure

```
sales_dashboard/
│
├── data/
│   └── sales.csv
│
├── reports/
│
├── src/
│   ├── data_processing.py
│   ├── forecasting.py
│   ├── report_generator.py
│
├── app.py
└── requirements.txt
```

---

# sales_data.csv

```csv
date,region,product,category,units_sold,unit_price
2026-01-01,North,Laptop,Electronics,15,75000
2026-01-01,South,Mobile,Electronics,40,20000
2026-01-02,East,Tablet,Electronics,25,30000
2026-01-02,West,Headphones,Accessories,60,3000
2026-01-03,North,Monitor,Electronics,20,15000
2026-01-03,South,Keyboard,Accessories,80,1500
2026-01-04,East,Mouse,Accessories,100,800
2026-01-04,West,Laptop,Electronics,10,72000
2026-01-05,North,Mobile,Electronics,50,21000
2026-01-05,South,Tablet,Electronics,30,32000
2026-01-06,East,Headphones,Accessories,75,3500
2026-01-06,West,Monitor,Electronics,18,16000
2026-01-07,North,Keyboard,Accessories,90,1700
2026-01-07,South,Mouse,Accessories,110,900
2026-01-08,East,Laptop,Electronics,12,76000
2026-01-08,West,Mobile,Electronics,45,19500
2026-01-09,North,Tablet,Electronics,28,31000
2026-01-09,South,Headphones,Accessories,85,3200
2026-01-10,East,Monitor,Electronics,22,15500
2026-01-10,West,Keyboard,Accessories,95,1600
```

**#**src\data_processing.py**#**
import pandas as pd

def load_sales_data(filepath):
    df = pd.read_csv(filepath)

    # Remove duplicates
    df = df.drop_duplicates()

    # Standardize date format
    df['date'] = pd.to_datetime(df['date'])

    # Handle missing values
    df = df.fillna({
        'units_sold': 0,
        'unit_price': 0
    })

    # Feature Engineering
    df['revenue'] = df['units_sold'] * df['unit_price']
    df['month'] = df['date'].dt.to_period('M')

    return df


def calculate_kpis(df):
    total_revenue = df['revenue'].sum()
    total_orders = df.shape[0]
    avg_order_value = total_revenue / total_orders

    revenue_by_category = df.groupby('category')['revenue'].sum()
    revenue_by_region = df.groupby('region')['revenue'].sum()

    monthly_revenue = df.groupby('month')['revenue'].sum().sort_index()
    mom_growth = monthly_revenue.pct_change().fillna(0)

    return {
        "total_revenue": total_revenue,
        "total_orders": total_orders,
        "avg_order_value": avg_order_value,
        "revenue_by_category": revenue_by_category,
        "revenue_by_region": revenue_by_region,
        "monthly_revenue": monthly_revenue,
        "mom_growth": mom_growth
    }


