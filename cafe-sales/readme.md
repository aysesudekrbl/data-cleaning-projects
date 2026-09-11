# Cafe Sales Data Cleaning

- Content: An end-to-end data cleaning project on the Cafe Sales (Kaggle) dataset.
- Goal: Practicing the strategies used when facing missing data in a realistic dirty dataset.

## Dataset

- Source: [Cafe Sales - Dirty Data for Cleaning Training](https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training)
- Total: 10,000 rows, 8 columns

## Issues Found and Solutions

### 1. Rows with wrong data type
Quantity, Price Per Unit and Total Spent were stored as strings (object) instead of numeric. Converted to numeric using pd.to_numeric().

### 2. Rows with missing data
- First, replaced "ERROR" / "UNKNOWN" values with np.nan, which revealed the real missing data rate.
- Used the relationship Total Spent = Quantity x Price Per Unit to fill in any one of the three columns when it was missing, using the other two.
- Payment Method, Location and Transaction Date had no statistical relationship with other columns, so missing values in these were filled with "Unknown".
- For products with a unique Price Per Unit, missing Item names were inferred from price. This wasn't applied to products with overlapping prices, since it wouldn't be reliable.
- A small number of rows (57) where multiple related columns were empty at the same time, with no way to fill them, were dropped.

## Result
- Missing values across all columns were reduced to 0. The cleaned data was saved as a new csv file.
- Part of the Item column (474 rows) couldn't be determined with certainty due to multiple products sharing the same price, and was marked as "Unknown".
- No relationship was found between Transaction Date and Transaction ID, so missing dates were not estimated.

## Tools
Python, pandas, numpy, Jupyter Notebook
