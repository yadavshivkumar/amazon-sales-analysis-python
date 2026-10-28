# Amazon Sales Analysis Using Python

## Project Overview

Amazon Sales Analysis is a data analysis project developed using Python. This project explores Amazon sales data to understand customer purchasing patterns, product demand, order fulfilment, and geographical sales distribution.

The analysis uses the Amazon Sale Report dataset and applies data cleaning, exploratory data analysis (EDA), and data visualization techniques to identify useful patterns and insights.

The project is implemented in Jupyter Notebook using Python libraries such as Pandas, NumPy, Matplotlib, and Seaborn.

## Project Objectives

- Analyze Amazon sales data to understand customer purchasing behaviour.
- Clean the dataset by handling missing values and removing unnecessary columns.
- Examine product categories and size preferences.
- Analyze order statuses and courier delivery information.
- Understand the distribution of B2B and retail customers.
- Investigate order fulfilment methods.
- Identify states with the highest number of orders.
- Visualize sales patterns using charts and graphs.

## Dataset Information

**Dataset Name:** Amazon Sale Report.csv

The dataset contains information about Amazon orders, products, fulfilment, and shipping details.

### Important Features

| Column | Description |
|---|---|
| Order ID | Unique identifier for an order |
| Date | Date of the order |
| Status | Current or recorded order status |
| Fulfilment | Order fulfilment method |
| Sales Channel | Sales channel associated with the order |
| ship-service-level | Shipping service level |
| Category | Product category |
| Size | Product size |
| Courier Status | Courier or shipment status |
| Qty | Quantity of products ordered |
| Amount | Order amount |
| ship-city | Customer's shipping city |
| ship-state | Customer's shipping state |
| ship-postal-code | Shipping postal code |
| ship-country | Shipping country |
| B2B | Indicates whether an order is a business-to-business order |
| fulfilled-by | Entity responsible for fulfilment, where recorded |

*Note: The dataset also contains additional fields, including currency and auxiliary columns.*

## Technologies Used

- **Python** — Programming language
- **Pandas** — Data manipulation and analysis
- **NumPy** — Numerical operations
- **Matplotlib** — Data visualization
- **Seaborn** — Statistical visualization
- **Jupyter Notebook** — Interactive analysis environment

## Project Workflow

### 1. Data Loading

The dataset is imported into a Pandas DataFrame for analysis.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv(
    "Amazon Sale Report.csv",
    encoding="unicode_escape"
)

df.head()
```

### 2. Data Exploration

The dataset is explored using methods such as:

- `head()` — View the first rows.
- `tail()` — View the last rows.
- `shape` — Check the number of rows and columns.
- `info()` — Examine column names and data types.
- `describe()` — Generate descriptive statistics.
- `isnull().sum()` — Check missing values.

### 3. Data Cleaning

The data cleaning process includes:

- Removing unnecessary columns such as `New` and `PendingS`.
- Checking missing values.
- Removing rows containing missing values.
- Converting the shipping postal code to an integer.
- Converting the Date column to a datetime data type.

Example:

```python
# Remove unnecessary columns
df.drop(
    columns=["New", "PendingS"],
    inplace=True,
    errors="ignore"
)

# Check missing values
print(df.isnull().sum())

# Remove rows with missing values
df.dropna(inplace=True)

# Convert data types
df["ship-postal-code"] = df["ship-postal-code"].astype(int)

df["Date"] = pd.to_datetime(df["Date"], dayfirst=True)

# Display dataset information
df.info()
```

**Note:** Removing all rows with missing values can significantly reduce the dataset. In a production analysis, missing values should be evaluated column by column before deciding whether to remove them.

### 4. Descriptive Statistics

Descriptive statistics are used to understand the numerical characteristics of the dataset.

```python
# Numerical summary
df.describe()

# Summary of categorical columns
df.describe(include="object")

# Summary of quantity and amount
df[["Qty", "Amount"]].describe()
```

### 5. Exploratory Data Analysis (EDA)

The project explores several aspects of Amazon sales data.

#### Product Size Analysis

A count plot is used to examine the distribution of product sizes.

```python
plt.figure(figsize=(10, 5))

ax = sns.countplot(data=df, x="Size")

for bars in ax.containers:
    ax.bar_label(bars)

plt.title("Distribution of Product Sizes")
plt.xlabel("Size")
plt.ylabel("Number of Orders")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

The notebook identifies M-size as a frequently purchased size.

#### Quantity Sold by Size

The total quantity is grouped by product size and sorted in descending order.

```python
S_Qty = (
    df.groupby("Size", as_index=False)["Qty"]
    .sum()
    .sort_values(by="Qty", ascending=False)
)

plt.figure(figsize=(10, 5))

sns.barplot(data=S_Qty, x="Size", y="Qty")

plt.title("Total Quantity Sold by Size")
plt.xlabel("Product Size")
plt.ylabel("Total Quantity")
plt.tight_layout()
plt.show()
```

This analysis helps compare the total quantities associated with different sizes.

#### Courier and Order Status Analysis

A count plot compares courier status with order status.

```python
plt.figure(figsize=(12, 6))

sns.countplot(
    data=df,
    x="Courier Status",
    hue="Status"
)

plt.title("Courier Status by Order Status")
plt.xlabel("Courier Status")
plt.ylabel("Number of Orders")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

This visualization helps examine the relationship between shipment progress and recorded order status.

#### Product Category Analysis

A histogram-style plot is used to explore the product categories in the dataset.

```python
plt.figure(figsize=(12, 6))

plt.hist(
    df["Category"].astype(str),
    bins=30,
    edgecolor="black"
)

plt.title("Distribution of Product Categories")
plt.xlabel("Product Category")
plt.ylabel("Frequency")
plt.xticks(rotation=90)
plt.tight_layout()
plt.show()
```

The notebook identifies T-shirts as a prominent product category.

#### B2B Customer Analysis

A pie chart is used to examine the distribution of B2B and non-B2B orders.

```python
B2B_Check = df["B2B"].value_counts()

plt.figure(figsize=(7, 7))

plt.pie(
    B2B_Check.values,
    labels=B2B_Check.index,
    autopct="%1.1f%%",
    startangle=90
)

plt.title("Distribution of B2B and Non-B2B Orders")
plt.tight_layout()
plt.show()
```

The notebook reports that approximately 99.3% of buyers are retailers and 0.7% are B2B buyers.

These percentages should be interpreted in the context of the cleaned dataset and the `B2B` field.

#### Order Fulfilment Analysis

A pie chart is used to compare order fulfilment methods.

```python
fulfilment_counts = df["Fulfilment"].value_counts()

plt.figure(figsize=(7, 7))

plt.pie(
    fulfilment_counts.values,
    labels=fulfilment_counts.index,
    autopct="%1.1f%%",
    startangle=90,
    wedgeprops={"width": 0.6}
)

plt.title("Distribution of Order Fulfilment Methods")
plt.axis("equal")
plt.show()
```

The notebook identifies Amazon as the predominant fulfilment method.

#### Geographical Sales Analysis

A count plot is used to explore the distribution of orders across Indian states.

```python
plt.figure(figsize=(14, 6))

sns.countplot(
    data=df,
    x="ship-state",
    order=df["ship-state"].value_counts().index
)

plt.title("Distribution of Orders by State")
plt.xlabel("State")
plt.ylabel("Number of Orders")
plt.xticks(rotation=90)
plt.tight_layout()
plt.show()
```

The analysis identifies Maharashtra as the state with the highest number of orders in the notebook.

#### Top 10 States by Order Count

The ten states with the highest order counts are selected for a more focused comparison.

```python
top_10_state = df["ship-state"].value_counts().head(10)

plt.figure(figsize=(12, 6))

sns.countplot(
    data=df[df["ship-state"].isin(top_10_state.index)],
    x="ship-state",
    order=top_10_state.index
)

plt.title("Top 10 States by Number of Orders")
plt.xlabel("State")
plt.ylabel("Number of Orders")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()
```

This visualization makes it easier to compare order distribution across the leading states.

## Key Findings

Based on the observations documented in the Jupyter Notebook:

1. **Product Size:** M-size is identified as a frequently purchased size.
2. **Product Category:** T-shirts are identified as a prominent product category.
3. **Customer Type:** The notebook reports that approximately 99.3% of buyers are retailers and 0.7% are B2B buyers.
4. **Order Fulfilment:** Amazon is identified as the predominant fulfilment method.
5. **Geographical Distribution:** Maharashtra has the highest number of orders in the analysis.
6. **Courier Status:** Courier status and order status are examined together to understand order and shipment patterns.

These findings describe the analysis performed on the dataset; they are not necessarily representative of all Amazon sales.

## Project Structure

```text
Amazon-Sales-Analysis/
│
├── Amazon Sale Report.csv
├── Amazon Sale Report.ipynb
└── README.md
```

*The structure above represents the suggested organization of the project repository.*

## How to Run the Project

### Step 1: Clone the Repository

```bash
git clone <your-github-repository-url>
```

### Step 2: Navigate to the Project Directory

```bash
cd Amazon-Sales-Analysis
```

### Step 3: Install the Required Libraries

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

### Step 4: Launch Jupyter Notebook

```bash
jupyter notebook
```

### Step 5: Run the Analysis

Open `Amazon Sale Report.ipynb` and execute the notebook cells in sequence.

Make sure the CSV file is available at the path used in the notebook. If both files are in the same directory, use the relative path:

```python
df = pd.read_csv(
    "Amazon Sale Report.csv",
    encoding="unicode_escape"
)
```

## Conclusion

The Amazon Sales Analysis project demonstrates how Python can be used to clean, explore, and visualize sales data.

The analysis examines product sizes, product categories, order and courier statuses, B2B transactions, fulfilment methods, and geographical order distribution.

The findings documented in the notebook indicate that M-size is a frequently purchased size, T-shirts are a prominent product category, Amazon is the predominant fulfilment method, and Maharashtra leads the analyzed state-wise order distribution.

Overall, this project provides practical experience in data preprocessing, exploratory data analysis, grouping, descriptive statistics, and visualization using Python.

## Skills Demonstrated

- Data Cleaning and Preprocessing
- Exploratory Data Analysis (EDA)
- Data Manipulation with Pandas
- Numerical Analysis with NumPy
- Data Visualization with Matplotlib
- Statistical Visualization with Seaborn
- GroupBy Operations and Aggregation
- Descriptive Statistics
- Jupyter Notebook



*This project was created for learning and demonstrating practical Python data analysis skills.*