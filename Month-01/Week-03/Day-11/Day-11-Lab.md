# 📊 Day 11 Lab: Titanic Dataset Exploration

## 📝 Task
Download the Titanic Dataset (`train.csv`) from [Kaggle](https://www.kaggle.com/c/titanic/data) (or search for "Titanic csv github raw").

## 🛠️ Script
Target File: `explore_titanic.py`

## 🚀 Action Items
1. **Load the CSV** into a pandas DataFrame.
2. **Print the first 5 rows** to inspect the data.
3. **Print the data types** of all columns.
4. **Print the shape** of the dataset (number of rows and columns).

## 🐍 Solution Code

```python
import pandas as pd

# 1. Load the CSV
try:
    df = pd.read_csv('train.csv')
    print("Successfully loaded 'train.csv'")
except FileNotFoundError:
    print("Error: 'train.csv' not found. Please ensure the file is in the same directory.")
    exit()

# 2. Print the first 5 rows
print("\n--- First 5 Rows ---")
print(df.head())

# 3. Print the data types of columns
print("\n--- Data Types ---")
print(df.dtypes)

# 4. Print the shape of the dataset
print("\n--- Dataset Shape ---")
rows, cols = df.shape
print(f"Rows: {rows}")
print(f"Columns: {cols}")
```
