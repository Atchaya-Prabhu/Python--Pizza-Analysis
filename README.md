# Python--Pizza-Analysis
This project performs a complete end-to-end analysis of a real-world Pizza Sales dataset using Python, Pandas, and visualization libraries. It showcases data cleaning, feature engineering, KPI calculations, and multiple business insights using charts.
"""
Pizza Sales Analysis
Dataset: pizza_sales.csv
Author: Atchaya
"""

# ==============================
# Imports
# ==============================
import warnings

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

warnings.filterwarnings("ignore")

# ==============================
# Load Data
# ==============================
df = pd.read_csv(r"C:/Users/ATCHAYA/Desktop/PIZZA SALES/pizza_sales.csv")

# ==============================
# Basic Exploration / Metadata
# ==============================
print("First 5 rows:")
print(df.head())

print("\nLast 5 rows:")
print(df.tail())

print("\nMetadata (rows, columns): ", df.shape)
print("Total Rows: ", df.shape[0])
print("Total Columns: ", df.shape[1])

print("\nColumns:")
print(df.columns)

print("\nInfo:")
df.info()

print("\nData types:")
print(df.dtypes)

print("\nSummary statistics (numeric columns):")
print(df.describe())

# ==============================
# KPI Calculations
# ==============================
total_revenue = df["total_price"].sum()
total_pizzas_sold = df["quantity"].sum()
total_orders = df["order_id"].nunique()
avg_order_value = total_revenue / total_orders
avg_pizzas_per_order = total_pizzas_sold / total_orders

print(f"\nThe Total Revenue is: ${total_revenue:,.2f}")
print(f"The Total Pizzas Sold is: {total_pizzas_sold:,}")
print(f"The Total Orders is: {total_orders:,}")
print(f"The Average Order Value is: ${avg_order_value:,.2f}")
print(f"The Average Pizzas per Order is: {avg_pizzas_per_order:,.2f}")

# ==============================
# Ingredient Analysis
# ==============================
ingredient = (
    df["pizza_ingredients"]
    .str.split(",")
    .explode()
    .str.strip()
    .value_counts()
    .reset_index()
)

ingredient.columns = ["ingredient", "count"]

print("\nTop 15 ingredients:")
print(ingredient.head(15))


# ==============================
# Daily Orders – Total Orders
# ==============================
df["order_date"] = pd.to_datetime(df["order_date"], dayfirst=True)
df["day_name"] = df["order_date"].dt.day_name()

weekday_order = ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"]
df["day_name"] = pd.Categorical(df["day_name"], categories=weekday_order, ordered=True)

orders_by_day = df.groupby("day_name", observed=False)["order_id"].nunique()

ax = orders_by_day.plot(kind="bar", figsize=(8, 5), color="green", edgecolor="black")
plt.title("Total Orders by Day of Week")
plt.xlabel("Day of Week")
plt.ylabel("Number of Orders")
plt.xticks(rotation=45)

for i, val in enumerate(orders_by_day):
    plt.text(i, val + 20, f"{val:,.0f}", ha="center", va="bottom", fontsize=9, fontweight="bold")

plt.tight_layout()
plt.show()


# ==============================
# Daily Trend – Total Revenue
# ==============================
orders_by_day_revenue = df.groupby("day_name", observed=False)["total_price"].sum()

ax = orders_by_day_revenue.plot(kind="bar", figsize=(8, 5), color="red", edgecolor="black")
plt.title("Total Revenue by Day of Week")
plt.xlabel("Day of Week")
plt.ylabel("Total Revenue ($)")
plt.xticks(rotation=45)

for i, val in enumerate(orders_by_day_revenue):
    plt.text(i, val + 20, f"{val:,.0f}", ha="center", va="bottom", fontsize=9, fontweight="bold")

plt.tight_layout()
plt.show()


# ==============================
# Daily Trend – Total Quantity Sold
# ==============================
orders_by_day_qty = df.groupby("day_name", observed=False)["quantity"].sum()

ax = orders_by_day_qty.plot(kind="bar", figsize=(8, 5), color="gold", edgecolor="black")
plt.title("Total Quantity Sold by Day of Week")
plt.xlabel("Day of Week")
plt.ylabel("Total Quantity")
plt.xticks(rotation=45)

for i, val in enumerate(orders_by_day_qty):
    plt.text(i, val + 20, f"{val:,.0f}", ha="center", va="bottom", fontsize=9, fontweight="bold")

plt.tight_layout()
plt.show()


# ==============================
# Hourly Trend – Total Orders
# ==============================
df["order_time"] = pd.to_datetime(df["order_time"], format="%H:%M:%S")
df["order_hour"] = df["order_time"].dt.hour

orders_by_hour_orders = df.groupby("order_hour", observed=False)["order_id"].nunique()

ax = orders_by_hour_orders.plot(kind="bar", figsize=(8, 5), color="maroon", edgecolor="black")
plt.title("Total Orders by Hour of Day")
plt.xlabel("Hour of Day (24-Hour Format)")
plt.ylabel("Number of Orders")
plt.xticks(rotation=0)

for i, val in enumerate(orders_by_hour_orders):
    plt.text(i, val + 5, f"{val:,.0f}", ha="center", va="bottom", fontsize=9, fontweight="bold")

plt.tight_layout()
plt.show()


# ==============================
# Hourly Trend – Total Revenue
# ==============================
orders_by_hour_revenue = df.groupby("order_hour", observed=False)["total_price"].sum()

ax = orders_by_hour_revenue.plot(kind="bar", figsize=(8, 5), color="blue", edgecolor="black")
plt.title("Total Revenue by Hour of Day")
plt.xlabel("Hour of Day (24-Hour Format)")
plt.ylabel("Total Revenue ($)")
plt.xticks(rotation=0)

for i, val in enumerate(orders_by_hour_revenue):
    plt.text(i, val + 5, f"{val:,.0f}", ha="center", va="bottom", fontsize=9, fontweight="bold")

plt.tight_layout()
plt.show()


# ==============================
# Monthly Trend – Total Orders
# ==============================
df["month_name"] = df["order_date"].dt.month_name()

month_order = [
    "January",
    "February",
    "March",
    "April",
    "May",
    "June",
    "July",
    "August",
    "September",
    "October",
    "November",
    "December",
]

df["month_name"] = pd.Categorical(df["month_name"], categories=month_order, ordered=True)

orders_by_month = df.groupby("month_name", observed=False)["order_id"].nunique()

plt.figure(figsize=(10, 5))
plt.fill_between(orders_by_month.index, orders_by_month.values, color="orange", alpha=0.6)
plt.plot(orders_by_month.index, orders_by_month.values, color="black", linewidth=2, marker="o")

plt.title("Total Orders by Month")
plt.xlabel("Month")
plt.ylabel("Number of Orders")
plt.xticks(rotation=45)

for i, val in enumerate(orders_by_month):
    plt.text(i, val + 20, f"{val:,.0f}", ha="center", va="bottom", fontsize=9, fontweight="bold")

plt.tight_layout()
plt.show()


# ==============================
# % of Sales by Pizza Category (Pie Chart)
# ==============================
category_sales = df.groupby("pizza_category")["total_price"].sum()
category_pct = category_sales / category_sales.sum() * 100

plt.figure(figsize=(10, 8))
colors = plt.get_cmap("tab20").colors
plt.pie(
    category_pct,
    labels=category_pct.index,
    autopct="%1.1f%%",
    startangle=90,
    colors=colors,
    wedgeprops={"edgecolor": "black", "width": 0.4},
)
plt.title("Percentage of Sales by Pizza Category")
plt.show()


# ==============================
# % Sales by Pizza Size & Category (Heatmap)
# ==============================
sales_pivot = df.pivot_table(
    index="pizza_category",
    columns="pizza_size",
    values="total_price",
    aggfunc="sum",
    fill_value=0,
)

sales_pct = sales_pivot / sales_pivot.sum().sum() * 100

plt.figure(figsize=(10, 6))
sns.heatmap(sales_pct, annot=True, fmt=".1f", cmap="YlOrRd", linewidths=0.5)
plt.title("% Sales by Pizza Category and Size")
plt.ylabel("Pizza Category")
plt.xlabel("Pizza Size")
plt.show()


# ==============================
# Total Pizza Sold by Pizza Category (Bar Chart)
# ==============================
pizzas_by_category = df.groupby("pizza_category")["quantity"].sum()

colors = list(plt.get_cmap("tab20").colors)
colors = colors[: len(pizzas_by_category)]

pizzas_by_category.plot(kind="bar", figsize=(8, 5), color=colors, edgecolor="black")
plt.title("Total Pizzas Sold by Pizza Category")
plt.xlabel("Pizza Category")
plt.ylabel("Total Pizzas Sold")
plt.xticks(rotation=45)

for i, val in enumerate(pizzas_by_category):
    plt.text(i, val + 5, f"{val:,.0f}", ha="center", va="bottom", fontsize=9, fontweight="bold")

plt.tight_layout()
plt.show()


# ==============================
# Top 5 Best Selling Pizzas – Total Quantity
# ==============================
pizzas_by_name_qty = df.groupby("pizza_name")["quantity"].sum()
top5_qty = pizzas_by_name_qty.sort_values(ascending=False).head(5)

ax = top5_qty.plot(kind="bar", figsize=(8, 5), color="grey", edgecolor="black")
plt.title("Top 5 Pizzas Sold (by Quantity)")
plt.xlabel("Pizza Name")
plt.ylabel("Total Pizzas Sold")
plt.xticks(rotation=45)

for i, val in enumerate(top5_qty):
    plt.text(i, val + 2, f"{val:,.0f}", ha="center", va="bottom", fontsize=9, fontweight="bold")

plt.tight_layout()
plt.show()


# ==============================
# Top 5 Best Selling Pizzas – Total Orders
# ==============================
pizzas_by_name_orders = df.groupby("pizza_name")["order_id"].nunique()
top5_orders = pizzas_by_name_orders.sort_values(ascending=False).head(5)

ax = top5_orders.plot(kind="bar", figsize=(8, 5), color="red", edgecolor="black")
plt.title("Top 5 Pizzas Ordered (by Unique Orders)")
plt.xlabel("Pizza Name")
plt.ylabel("Total Orders")
plt.xticks(rotation=45)

for i, val in enumerate(top5_orders):
    plt.text(i, val + 2, f"{val:,.0f}", ha="center", va="bottom", fontsize=9, fontweight="bold")

plt.tight_layout()
plt.show()


# ==============================
# Top 5 Best Selling Pizzas – Total Revenue
# ==============================
pizzas_by_name_revenue = df.groupby("pizza_name")["total_price"].sum()
top5_revenue = pizzas_by_name_revenue.sort_values(ascending=False).head(5)

ax = top5_revenue.plot(kind="bar", figsize=(8, 5), color="blue", edgecolor="black")
plt.title("Top 5 Pizzas by Revenue")
plt.xlabel("Pizza Name")
plt.ylabel("Total Revenue ($)")
plt.xticks(rotation=45)

for i, val in enumerate(top5_revenue):
    plt.text(i, val + 2, f"{val:,.0f}", ha="center", va="bottom", fontsize=9, fontweight="bold")

plt.tight_layout()
plt.show()


# ==============================
# Bottom 5 Pizzas – Total Revenue
# ==============================
bottom5_revenue = pizzas_by_name_revenue.sort_values(ascending=True).head(5)

ax = bottom5_revenue.plot(kind="bar", figsize=(8, 5), color="pink", edgecolor="black")
plt.title("Bottom 5 Pizzas by Revenue")
plt.xlabel("Pizza Name")
plt.ylabel("Total Revenue ($)")
plt.xticks(rotation=45)

for i, val in enumerate(bottom5_revenue):
    plt.text(i, val + 2, f"{val:,.0f}", ha="center", va="bottom", fontsize=9, fontweight="bold")

plt.tight_layout()
plt.show()
