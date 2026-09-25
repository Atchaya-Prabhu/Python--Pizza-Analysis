# 🍕 Python Pizza Sales Analysis

## 📊 Project Overview

This project performs an end-to-end analysis of a real-world **Pizza Sales dataset** using **Python, Pandas, NumPy, Matplotlib, and Seaborn**.

The objective is to analyze pizza sales performance, identify customer ordering patterns, calculate key business KPIs, and generate actionable insights through data visualization.

The analysis covers **sales trends, order behavior, pizza categories, pizza sizes, ingredients, and individual pizza performance**.

---

## 🎯 Business Objectives

The main objectives of this project are to:

* Analyze overall pizza sales performance
* Calculate key business KPIs
* Identify the busiest days and hours for orders
* Analyze revenue and quantity trends
* Understand sales contribution by pizza category
* Analyze sales by pizza size and category
* Identify the most frequently used ingredients
* Identify the top-performing pizzas
* Identify pizzas generating the lowest revenue
* Visualize business trends to support data-driven decisions

---

## 🛠️ Technologies Used

| Technology           | Purpose                                        |
| -------------------- | ---------------------------------------------- |
| **Python**           | Data analysis and processing                   |
| **Pandas**           | Data cleaning, transformation, and aggregation |
| **NumPy**            | Numerical calculations                         |
| **Matplotlib**       | Data visualization                             |
| **Seaborn**          | Statistical visualization                      |
| **Jupyter Notebook** | Development and analysis environment           |
| **GitHub**           | Version control and project sharing            |

---

## 📁 Dataset

**Dataset:** `pizza_sales.csv`

The dataset contains pizza order-level information including:

* Order ID
* Order Date
* Order Time
* Pizza Name
* Pizza Category
* Pizza Size
* Quantity
* Total Price
* Pizza Ingredients

---

## 🔍 Project Workflow

### 1. Import Libraries

The project begins by importing the required Python libraries:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
```

---

### 2. Load the Dataset

The pizza sales CSV file is loaded into a Pandas DataFrame:

```python
df = pd.read_csv("pizza_sales.csv")
```

---

### 3. Data Exploration

The dataset is explored using:

* `head()`
* `tail()`
* `shape`
* `columns`
* `info()`
* `dtypes`
* `describe()`

This helps understand the dataset structure, number of records, available fields, data types, and numerical distributions.

---

# 📈 Key Performance Indicators (KPIs)

The following business KPIs are calculated:

### 💰 Total Revenue

Total revenue generated from all pizza sales.

```python
total_revenue = df["total_price"].sum()
```

### 🍕 Total Pizzas Sold

Total number of pizzas sold.

```python
total_pizzas_sold = df["quantity"].sum()
```

### 🧾 Total Orders

Number of unique customer orders.

```python
total_orders = df["order_id"].nunique()
```

### 💵 Average Order Value

Average revenue generated per order.

```python
avg_order_value = total_revenue / total_orders
```

### 🍕 Average Pizzas per Order

Average number of pizzas purchased per order.

```python
avg_pizzas_per_order = total_pizzas_sold / total_orders
```

---

# 🧂 Ingredient Analysis

The project analyzes the ingredients used across pizzas.

The ingredient column is split and transformed using Pandas:

```python
ingredient = (
    df["pizza_ingredients"]
    .str.split(",")
    .explode()
    .str.strip()
    .value_counts()
    .reset_index()
)
```

This allows the project to identify the **most frequently used ingredients** across the pizza menu.

---

# 📅 Time-Based Analysis

## Orders by Day of Week

The order date is converted into a datetime format and the day of the week is extracted.

```python
df["order_date"] = pd.to_datetime(
    df["order_date"],
    dayfirst=True
)

df["day_name"] = df["order_date"].dt.day_name()
```

The analysis compares the number of orders received on:

* Monday
* Tuesday
* Wednesday
* Thursday
* Friday
* Saturday
* Sunday

---

## 💰 Revenue by Day

Revenue is aggregated by day of the week to understand which days generate the highest sales.

```python
orders_by_day_revenue = (
    df.groupby("day_name", observed=False)["total_price"]
    .sum()
)
```

---

## 🍕 Quantity Sold by Day

The project also analyzes the total number of pizzas sold on each day.

```python
orders_by_day_qty = (
    df.groupby("day_name", observed=False)["quantity"]
    .sum()
)
```

---

# 🕐 Hourly Sales Analysis

The order time is converted into a datetime format and the order hour is extracted.

```python
df["order_time"] = pd.to_datetime(
    df["order_time"],
    format="%H:%M:%S"
)

df["order_hour"] = df["order_time"].dt.hour
```

The project analyzes:

* Orders by hour
* Revenue by hour

This helps identify the busiest periods of the day.

---

# 📆 Monthly Sales Analysis

The project extracts the month from the order date:

```python
df["month_name"] = df["order_date"].dt.month_name()
```

Monthly order trends are then visualized to identify changes in sales activity throughout the year.

---

# 🍕 Pizza Category Analysis

The project calculates the percentage contribution of each pizza category to total sales.

```python
category_sales = (
    df.groupby("pizza_category")["total_price"]
    .sum()
)

category_pct = (
    category_sales /
    category_sales.sum()
) * 100
```

A pie/donut chart is used to visualize the sales contribution of each category.

---

# 📏 Pizza Size Analysis

A pivot table is created to analyze sales across:

* Pizza Category
* Pizza Size

```python
sales_pivot = df.pivot_table(
    index="pizza_category",
    columns="pizza_size",
    values="total_price",
    aggfunc="sum",
    fill_value=0
)
```

The results are converted into percentages and displayed using a heatmap.

---

# 🍕 Total Pizzas Sold by Category

The project analyzes the total quantity of pizzas sold for each category.

```python
pizzas_by_category = (
    df.groupby("pizza_category")["quantity"]
    .sum()
)
```

This helps compare the volume of pizzas sold across different categories.

---

# 🏆 Top 5 Best-Selling Pizzas

The project identifies the top 5 pizzas using three different business metrics.

### 1. Top 5 by Quantity

```python
pizzas_by_name_qty = (
    df.groupby("pizza_name")["quantity"]
    .sum()
)

top5_qty = (
    pizzas_by_name_qty
    .sort_values(ascending=False)
    .head(5)
)
```

### 2. Top 5 by Number of Orders

```python
pizzas_by_name_orders = (
    df.groupby("pizza_name")["order_id"]
    .nunique()
)

top5_orders = (
    pizzas_by_name_orders
    .sort_values(ascending=False)
    .head(5)
)
```

### 3. Top 5 by Revenue

```python
pizzas_by_name_revenue = (
    df.groupby("pizza_name")["total_price"]
    .sum()
)

top5_revenue = (
    pizzas_by_name_revenue
    .sort_values(ascending=False)
    .head(5)
)
```

Using multiple metrics provides a more complete view of pizza performance.

---

# 📉 Bottom 5 Pizzas by Revenue

The project also identifies pizzas generating the lowest total revenue.

```python
bottom5_revenue = (
    pizzas_by_name_revenue
    .sort_values(ascending=True)
    .head(5)
)
```

This can help identify products that may require further investigation regarding pricing, demand, menu placement, or customer preferences.

---

# 📊 Visualizations

The project includes several visualizations:

* 📊 Total Orders by Day of Week
* 💰 Total Revenue by Day of Week
* 🍕 Total Quantity Sold by Day
* 🕐 Total Orders by Hour
* 💵 Total Revenue by Hour
* 📆 Total Orders by Month
* 🥧 Percentage of Sales by Pizza Category
* 🔥 Sales Percentage by Pizza Category and Size
* 📊 Total Pizzas Sold by Category
* 🏆 Top 5 Pizzas by Quantity
* 🏆 Top 5 Pizzas by Orders
* 💰 Top 5 Pizzas by Revenue
* 📉 Bottom 5 Pizzas by Revenue

---

# 💡 Business Insights

The analysis can be used to answer questions such as:

* Which days generate the most orders?
* Which hours have the highest customer demand?
* Which months show stronger sales activity?
* Which pizza categories contribute the most revenue?
* Which pizza sizes generate the highest sales?
* Which ingredients are most commonly used?
* Which pizzas are the best sellers?
* Which pizzas generate the most revenue?
* Which pizzas generate the lowest revenue?

These insights can support decisions around **menu optimization, inventory planning, staffing, promotions, and product strategy**.

---

# 📂 Project Structure

```text
Python--Pizza-Analysis/
│
├── pizza_sales.csv
├── Pizza_Sales_Analysis.ipynb
├── README.md
└── visualizations/
```

---

# 🚀 How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/Atchaya-Prabhu/Python--Pizza-Analysis.git
```

### 2. Navigate to the project

```bash
cd Python--Pizza-Analysis
```

### 3. Install required libraries

```bash
pip install pandas numpy matplotlib seaborn
```

### 4. Open the Jupyter Notebook

```bash
jupyter notebook
```

### 5. Run the analysis

Open the notebook and execute the cells sequentially.

---

# 📌 Skills Demonstrated

This project demonstrates practical experience with:

* Python for data analysis
* Pandas data manipulation
* NumPy calculations
* Data cleaning
* Feature engineering
* GroupBy analysis
* Pivot tables
* KPI development
* Time-series analysis
* Business analysis
* Data visualization
* Exploratory Data Analysis (EDA)
* Translating raw data into business insights

---

# 👩‍💻 Author

**Atchaya Prabhu**

Data Analyst | Business Intelligence | Data Engineering

GitHub: [Atchaya-Prabhu](https://github.com/Atchaya-Prabhu)

---

⭐ If you found this project useful, feel free to star the repository!
