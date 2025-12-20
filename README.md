# Python_Customer_Segmentation_RFM
This project focuses on customer segmentation using RFM analysis based on e-commerce transactional data. The goal is to transform raw sales data into actionable customer insights that support marketing, retention, and revenue optimization decisions.

---

## Table of Contents
- Overview
- Exploratory Data Analysis (EDA)
- Data Wrangling
- RFM Score & Segment
- Visualizations & Key Insights
- Recommendations

---

## Overview
### Main Objectives
- Segment customers based on **Recency, Frequency, and Monetary value**
- Identify **high-value**, **loyal**, **at-risk**, and **inactive** customer groups
- Understand how customer behavior changes **over time**
- Support business decisions related to **customer retention, re-engagement, and revenue growth**
### Why RFM Analysis?
- RFM is used because it answers three fundamental business questions using **actual customer behavior**, not assumptions:
    - **Recency** – How recently did the customer purchase?
    - **Frequency** – How often do they purchase?
    - **Monetary** – How much do they spend?
- RFM is particularly suitable for this project because:
    - It is **simple, interpretable, and explainable** to non-technical stakeholders.
    - It works well with **transaction-level data**.
    - It directly links customer behavior to **business value**.
    - It allows easy mapping to **business-friendly segments** (e.g. Champions, Loyal, At Risk, Hibernating).
    - Unlike black-box models, RFM provides **transparent logic**, which is critical for business adoption.
### Business Questions
- Who are the **most valuable customers** contributing the highest revenue?
- Which customers are **active but low-spending**, and which are **high-spending but infrequent**?
- Which customers are **at risk of churn** due to declining recency or frequency?
- How does **customer value change over time**?
### Tools & Technologies
- **Python** (core language for the entire project)
- pandas, numpy – data manipulation & aggregation
- matplotlib / seaborn – data visualization
- Jupyter / Google Colab – analysis environment

---

## Exploratory Data Analysis (EDA)
In [1]:
```python
# Import libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
```
In [2]:
```python
# Import tables
from google.colab import drive
drive.mount('/content/drive')

path = '/content/drive/MyDrive/Python_Final Project/'

df_retail = pd.read_excel(path + 'ecommerce retail.xlsx', sheet_name="ecommerce retail")
df_segment = pd.read_excel(path + 'ecommerce retail.xlsx', sheet_name="Segmentation")

df_retail.head()
```
Out [2]:

<img width="1026" height="199" alt="image" src="https://github.com/user-attachments/assets/f5254f78-8b81-483d-816c-d01c6a276d55" />


In [3]:
```python
# Check the general info
df_retail.info()
```
Out [3]:
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 541909 entries, 0 to 541908
Data columns (total 8 columns):
 #   Column       Non-Null Count   Dtype         
---  ------       --------------   -----         
 0   InvoiceNo    541909 non-null  object        
 1   StockCode    541909 non-null  object        
 2   Description  540455 non-null  object        
 3   Quantity     541909 non-null  int64         
 4   InvoiceDate  541909 non-null  datetime64[ns]
 5   UnitPrice    541909 non-null  float64       
 6   CustomerID   406829 non-null  float64       
 7   Country      541909 non-null  object        
dtypes: datetime64[ns](1), float64(2), int64(1), object(4)
memory usage: 33.1+ MB
```
In [4]:
```python
# Check for missing values
df_retail.isna().sum()
```
Out [4]:

<img width="185" height="324" alt="image" src="https://github.com/user-attachments/assets/79106db9-fd04-428e-a384-5dc3903ef996" />

In [5]:
```python
# Check data summary
df_retail.describe()
```
Out [5]:

<img width="632" height="292" alt="image" src="https://github.com/user-attachments/assets/17a4ec6b-e4cc-4b69-8593-1ed9f0bfed23" />

In [6]:
```python
# Check duplicates
df_retail.duplicated().sum()
```
Out [6]:
```
np.int64(5268)
```

### Summary
- The dataframe (retail) contains 541,909 rows and 8 columns.
- There are 135,080 missing values for CustomerID which is important for RFM analysis.
- There are negative values in Quantity and Unit Price columns, which is not logically valid.
- There are 5268 duplicated values.

---

## Data Wrangling
