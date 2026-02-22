# System Architecture

```text
Raw CSV
   ↓
Data Cleaning & Feature Engineering
   ↓
KPI Engine
   ↓
Trend Forecasting Model
   ↓
Streamlit Dashboard
   ↓
PDF Auto-Report Generator
   ↓
Internal Deployment
```

---

# 📂 Project Structure

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

# STEP 1 — Data Processing Layer

## `src/data_processing.py`

```python
import pandas as pd

def load_and_clean_data(file):
    df = pd.read_csv(file)

    df.drop_duplicates(inplace=True)

    df['Date'] = pd.to_datetime(df['Date'])

    df['Discount'] = df['Discount'].fillna(0)

    df['Revenue'] = df['Quantity'] * df['Price'] * (1 - df['Discount'])

    df['Month'] = df['Date'].dt.to_period('M')

    return df


def generate_kpis(df):
    total_revenue = df['Revenue'].sum()
    total_orders = df['Order_ID'].nunique()
    avg_order_value = total_revenue / total_orders

    monthly_kpi = df.groupby('Month')['Revenue'].sum().reset_index()

    return total_revenue, total_orders, avg_order_value, monthly_kpi
```

---

# STEP 2 — Trend Forecasting (Prophet Model)

Use **Prophet** for time-series forecasting.

## Install:

```
pip install prophet
```

---

## `src/forecasting.py`

```python
from prophet import Prophet
import pandas as pd

def forecast_sales(df):
    forecast_df = df.groupby('Date')['Revenue'].sum().reset_index()

    forecast_df.rename(columns={'Date': 'ds', 'Revenue': 'y'}, inplace=True)

    model = Prophet()
    model.fit(forecast_df)

    future = model.make_future_dataframe(periods=30)
    forecast = model.predict(future)

    return forecast
```

This adds:

* Trend component
* Seasonality
* Future prediction

---

# STEP 3 — Automated PDF Report

Generate executive-ready PDF reports using **ReportLab**.

## Install:

```
pip install reportlab
```

---

## `src/report_generator.py`

```python
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer
from reportlab.lib.styles import ParagraphStyle
from reportlab.lib import colors
from reportlab.lib.styles import getSampleStyleSheet

def generate_pdf(total_revenue, total_orders, avg_order_value):
    doc = SimpleDocTemplate("reports/monthly_report.pdf")
    elements = []

    styles = getSampleStyleSheet()

    elements.append(Paragraph("Monthly Sales Report", styles['Heading1']))
    elements.append(Spacer(1, 20))

    elements.append(Paragraph(f"Total Revenue: {total_revenue:.2f}", styles['Normal']))
    elements.append(Paragraph(f"Total Orders: {total_orders}", styles['Normal']))
    elements.append(Paragraph(f"Average Order Value: {avg_order_value:.2f}", styles['Normal']))

    doc.build(elements)
```

---

# STEP 4 — Streamlit Dashboard

Build the UI using **Streamlit**.

---

## `app.py`

```python
import streamlit as st
import matplotlib.pyplot as plt
import seaborn as sns

from src.data_processing import load_and_clean_data, generate_kpis
from src.forecasting import forecast_sales
from src.report_generator import generate_pdf

st.set_page_config(layout="wide")

st.title("📊 AI Sales Intelligence Dashboard")

uploaded_file = st.file_uploader("Upload Sales CSV", type=["csv"])

if uploaded_file:

    df = load_and_clean_data(uploaded_file)

    total_revenue, total_orders, avg_order_value, monthly_kpi = generate_kpis(df)

    col1, col2, col3 = st.columns(3)

    col1.metric("Total Revenue", f"{total_revenue:,.2f}")
    col2.metric("Total Orders", total_orders)
    col3.metric("Avg Order Value", f"{avg_order_value:,.2f}")

    st.subheader("Monthly Revenue Trend")

    fig, ax = plt.subplots()
    ax.plot(monthly_kpi['Month'].astype(str), monthly_kpi['Revenue'])
    plt.xticks(rotation=45)
    st.pyplot(fig)

    st.subheader("Correlation Heatmap")

    fig2, ax2 = plt.subplots()
    sns.heatmap(df.corr(numeric_only=True), annot=True, ax=ax2)
    st.pyplot(fig2)

    st.subheader("Forecast (Next 30 Days)")

    forecast = forecast_sales(df)

    fig3, ax3 = plt.subplots()
    ax3.plot(forecast['ds'], forecast['yhat'])
    st.pyplot(fig3)

    if st.button("Generate PDF Report"):
        generate_pdf(total_revenue, total_orders, avg_order_value)
        st.success("Report Generated Successfully!")
```

---

# STEP 5 — requirements.txt

```
streamlit
pandas
matplotlib
seaborn
prophet
reportlab
```

