# Shein E-Commerce Data Cleaning

- Content: An end-to-end data cleaning project on 21 scraped Shein product category files (Dirty E-Commerce Data, Kaggle).
- Goal: Combining inconsistent scraped files into one dataset, detecting missing values, duplicates, and messy text fields, and fixing them without unnecessary data loss.

## Dataset

- Source: [Dirty E-Commerce Data - Shein](https://www.kaggle.com/datasets/oleksiimartusiuk/e-commerce-data-shein)
- Total: 82,105 rows (after combining 21 files), column count varied per file (6-12 columns each) before combining.

## Issues Found and Solutions

### 1. Inconsistent files
21 separate CSV files (one per category) had different column counts. Combined them with glob.glob() and pd.concat() into a single DataFrame. Confirmed the mismatch wasn't a data error - it came from scraped pages not always containing the same HTML elements (rank badge, color count, Black Friday banner), so missing columns per product reflect real feature differences, not broken data.

### 2. Missing values
Checked isna().sum() and converted counts to percentages to judge severity properly. Most high-NaN columns turned out to represent optional product features (NaN = feature not present), not actual gaps. Only "price" (2 rows) and "goods-title-link" (678 rows) needed real attention.

### 3. Fully empty rows
Found 2 rows where every single column was NaN (including price). Dropped both.

### 4. Duplicate title columns
"goods-title-link" and "goods-title-link--jump" held the same product name from two different HTML sources, never filled at the same time. Merged them into one "title" column with fillna(). 12 rows still had no title after merging; filled with "Unknown" since the rest of their data was present.

### 5. Wrong data types
"price", "discount" and "color-count" were stored as text (with $ and % symbols). Cleaned the symbols and converted to float64 with pd.to_numeric(errors="coerce").

### 6. Duplicate rows
Duplicate definition changed the result significantly: "title" alone found 10,622 duplicates, adding "price" narrowed it to 8,630 (more reliable). Adding "href" narrowed it further, but href was ~99% empty and pandas treats NaN as equal to NaN, so it was excluded to avoid false matches. Final decision: drop_duplicates on "title" + "price" (8,630 rows removed).

### 7. Numbers hidden inside text
Extracted numeric values out of text fields using regex: "500+ sold recently" / "1.9k+ sold recently" → sold_over_count column, and "#9 Best Seller" → rank-number column. Verified the extraction kept NaN rows consistent with the originals.

### 8. Outlier checks
Checked for negative price, zero price, and discount over 100% - none found, no further fixing needed.

## Tools
Python, pandas, glob, regex (str.extract)