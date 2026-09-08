# Cafe Sales Data Cleaning
- Content: An end-to-end data cleaning project on the Cafe Sales (Kaggle) dataset.
- Goal: Practicing strategies for handling missing data in a realistically dirty dataset.

## Dataset
- Source: [Cafe Sales - Dirty Data for Cleaning Training](https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training)
- Total: 10,000 rows, 8 columns

## Issues Found and Solutions

### 1. Rows with the wrong data type
- Quantity, Price Per Unit, Total Spent columns should have been numeric but were found as strings (object). Converted to numeric using pd.to_numeric().

### 2. Rows with missing data
- First, "ERROR" / "UNKNOWN" values were replaced with np.nan. This revealed the true rate of missing data.
- Using the Total Spent = Quantity x Price Per Unit relationship, if any one of the three columns was missing, it was calculated and filled in using the other two.
- No statistical relationship could be found between Payment Method, Location, Transaction Date and the other columns, so missing values in these rows were filled with "Unknown".
- Missing Item names were predicted from the price for products with a unique Price Per Unit value. This method wasn't applied for products with overlapping prices, since it wouldn't be reliable.
- A small number of rows (57) where multiple related columns were missing at the same time, with no way to fill them by any method, were deleted.

## Result
- The number of missing data was brought down to 0 across all columns. The clean data was saved as a new csv file.
- Part of the Item column (474 rows) couldn't be determined with certainty since multiple products shared the same price, and was marked as "Unknown".
- No chronological relationship could be found between Transaction Date and Transaction ID, so no predictions were made for missing dates.

## Tools Used
Python, pandas, numpy, Jupyter Notebook
