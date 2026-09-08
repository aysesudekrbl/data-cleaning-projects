# NYC Airbnb 2019 Data Cleaning
- Content: An end-to-end data cleaning project on the New York City Airbnb Open Data 2019 (Kaggle) dataset.
- Goal: Detecting missing values, logical errors, and inconsistencies that can be encountered in real-world data, and fixing them without data loss.

## Dataset
- Source: [New York City Airbnb Open Data](https://www.kaggle.com/datasets/dgomonov/new-york-city-airbnb-open-data)
- Total: 48,895 rows, 16 columns

## Issues Found and Solutions

### 1. Missing review data
- The "reviews_per_month" column had 10,052 missing values.
- All of these rows had a "number_of_reviews" value of 0. This showed us that there were no reviews at all, meaning the monthly review count couldn't be calculated.
- Missing values in the "reviews_per_month" column were filled with 0.
- Missing values in the "last_review" column were filled with "Doesn't exist" if "number_of_reviews", i.e. the number of reviews found, was 0, or "Couldn't find" otherwise (not encountered in this dataset). Existing dates were left untouched.

### 2. Missing "name" and "host_name"
- 16 and 21 rows respectively were missing this data. Since the number was very small and there was no information available to make a prediction, they were filled with "Unknown".
### 3. Logical Inconsistency (between "minimum_nights" and "availability_365")
- The "minimum_nights" column, i.e. the minimum number of nights required to stay when renting, had a max value of 1,250 nights. (Median 3, Mean 7) This showed us that there was a serious outlier.
- 14 rows were found where "minimum_nights" > "availability_365". A listing can't be rented for more days than it's available, so requesting a higher minimum number of nights isn't possible. This presents a logical impossibility.
- These 14 rows were **not deleted**, the original data was preserved and marked with two new columns instead:
       "is_unrealistic_min_nights": flags rows carrying this inconsistency as 'True'.
       "minimum_nights_adjusted": takes the minimum of the "minimum_nights" and "availability_365" values, which equals "availability_365" for rows containing the logical error, i.e. pulls it down to a realistic upper bound. Keeps the original value for the rest.
- In cases where "availability_365" is 0, the "minimum_nights_adjusted" value is also set to 0, which shows that the listing can't be rented at all.

### Columns checked and found clean
- No duplicate rows.
- "price" is the correct data type.
- "room_type" only contains 3 consistent categories.
- "latitude" and "longitude" match NYC's geographic location (northern western hemisphere).
- "availability_365" is always within the 0-365 range.

## Result
No missing values remained in the cleaned dataset. ("data.isnull().sum()" result is 0 across all columns). Flagging and logic-based imputation were preferred over deletion, minimizing original data loss. This way, suspicious rows can easily be filtered out in future analyses.

## Tools Used
Python, pandas, numpy, Jupyter Notebook
