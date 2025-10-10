# LEGO-Dataset-Cleaning-Exploration-Project
LEGO sets dataset (1970–2022): Data cleaning, exploratory analysis, and dashboarding in Excel.

## Project Overview
This project focuses on cleaning, organizing, and preparing a LEGO dataset for further analysis and visualization.  
The goal is to make the dataset structured, consistent, and ready for use in future data analytics or machine learning workflows.  
The data contains information about LEGO sets released from **1970 to 2022**, including details like name, set ID, sub-theme, price, number of pieces, minifigures, and more.

## Dataset Description
The dataset initially had **18,458 rows** and multiple columns with mixed data types and missing values.  
Some key columns include:
- `set_id` — Unique identifier of each LEGO set  
- `name` — Name of the LEGO set  
- `year` — Year of release (1970–2022)  
- `sub_theme` — Sub-theme of the set (many missing values)  
- `pieces` — Number of pieces in the set  
- `minifigs` — Number of minifigures (many sets don’t have any)  
- `age_range` — Recommended age range (many missing values)  
- `US_retailPrice` — Retail price in USD  
- `bricksetURL`, `thumbnailURL`, `imageURL` — Reference links for each set

## Data Cleaning Steps

### 1. **Initial Formatting**
- Converted `set_id` column from *Date* format to **General** to fix misinterpretation of IDs.
- Ensured columns with web links (`bricksetURL`, `thumbnailURL`, `imageURL`) were set to **General** type to avoid hyperlink formatting issues.
- Checked `year` and `name` columns for missing or incorrect values — none found.

### 2. **Handling Missing Values**
- **Sub-theme:** ~4,000 blanks were detected. Selected blanks using `Ctrl+G → Special → Blanks`.  
  → Replaced blanks with the label `"Unknown"`.
  
- **Pieces:** 3,924 missing values.  
  → Decided **not to impute with `0`** since sets are supposed to have pieces.  
  → Left as missing for now (may explore advanced imputation later).

- **Minifigs:** ~8,400 blanks.  
  → Missing values make sense (many sets have no minifigs).  
  → Left blanks as is.

- **Age Range:** 11,671 blanks.  
  → Left missing, will analyze later to see if imputation is needed.

- **US_retailPrice:** Added a helper column (`price_flag`) with the formula:
  ```excel
  =IF(ISBLANK([@[US_retailPrice]]), "Missing", "Available")
→ Fixed formula issues caused by spaces in column name by using structured references correctly.

### 3. **Duplicates & Consistency Checks**
•	Checked and removed duplicates in set_id column.
•	Confirmed counts before and after removal matched expected results.
•	Kept full dataset (18,458 rows) to preserve data integrity.

### 4. **Standardizing Columns**
•	Replaced ? or Unnamed values in name column with "Unknown".
•	Created helper columns to flag missingness for key fields:
o	missing_pieces
o	missing_minifigs
o	missing_agerange
o	missing_price

 Data Cleaning — LEGO Dataset
1. Initial Column Review & Missing Data Identification
•	Identified key columns with missing values:
o	sub_theme → 3,924 missing values
o	minifigs → ~8,400 missing values
o	agerange_min → 11,671 missing values
o	US_retailPrice → some blanks detected (further validated with LEN(TRIM()))
o	bricksetURL, thumbnailURL, imageURL → web link columns (mostly populated)
•	Confirmed presence of records from 1970–2022 (time span check).
________________________________________
2. Missing Value Strategy
•	sub_theme:
o	Initially tried blank selection in Excel (Ctrl + G → Special → Blanks).
o	Considered imputation but logically decided not to use 0 (since LEGO sets must have pieces).
o	Classified as likely MNAR (Missing Not At Random).
o	Final decision: leave blanks as is or tag as "Missing Pieces" label if needed for modeling.
•	minifigs:
o	Majority of values are missing → but this is expected (many LEGO sets don’t include minifigs).
o	Decision: leave blanks as "0" or "No Minifigs" depending on downstream use.
•	agerange_min:
o	Noted many missing values but verified valid year range of LEGO sets.
o	No imputation applied yet.
•	US_retailPrice:
o	Found hidden blanks (some cells look empty but aren’t truly blank).
o	Switched to:
o	=IF(LEN(TRIM([@US_retailPrice]))=0,"Missing","Available")
to correctly flag missing values.
________________________________________
3. Data Type Adjustments
•	Converted columns to proper datatypes:
o	bricksetURL, thumbnailURL, imageURL → set as General (URL links) for uniform handling.
•	Checked numeric columns like pieces, minifigs, and US_retailPrice to ensure proper numeric formatting.
________________________________________
4. Quality Check / Notes for Next Steps
•	⚠️ Be mindful: adding many derived columns may increase table size but is fine during cleaning — drop or archive derived columns later if needed.
•	Potential next steps:
o	Handle missing values for modeling (decide on 0, "Missing", median, or leave as null depending on the column).
o	Consider feature engineering (e.g., price per piece, minifigs per set).
o	Validate outliers (e.g., unusually high prices or piece counts).
o	Standardize categorical columns (e.g., theme, sub_theme).
________________________________________
✅ Outcome after Step 1:
•	All key columns reviewed.
•	Missing data assessed with logical reasoning per column.
•	Data types standardized for URL and numeric fields.
•	Missing value flagging formulas refined.

