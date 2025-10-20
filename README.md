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
- **Sub-theme:** 3557 blanks were detected. Selected blanks using `Ctrl+G → Special → Blanks`.  
→ Replaced blanks with the label `"Unspecified`.	Classified as likely MNAR (Missing Not At Random).
  Subtheme had 3,557 missing values (~19%). A temporal pattern was observed between 1997–2002, primarily in the Normal category, indicating that these sets were released without a defined subtheme. To reflect
  this business reality, blanks were imputed with the label ‘Unspecified’ rather than ‘Unknown’
  
 - **Theme Group:**
 → Theme Group had 2 blank values out of 18,458 rows. Since subtheme was already labeled ‘Unspecified’ for these records and ‘Unspecified’ subthemes occur across multiple theme groups, the blanks were imputed
   with ‘Unspecified’ to maintain dashboard consistency.”

-**Category:**
 → The Category column contained three major values: Normal, Extended, and Random. An exploratory check against Theme and Subtheme showed no meaningful relationship for ‘Random’. Given its substantial count,        ‘Random’ was retained as a separate valid category rather than merged or dropped


- **Pieces:** 3,924 missing values.  
  → Decided **not to impute with `0`** since sets are supposed to have pieces.  
  → Handling Zero & Blank Values in pieces Column, The pieces column contained both blank values and zeros, mainly for themes like Books and Gear where piece counts are not applicable.
  → To make this consistent, I created a new column **Pieces_Status** using the formula: =IF(OR(ISBLANK(A2),A2=0),"Not Applicable","Has Pieces")
    Has Pieces → when the set has a non-zero piece count, Not Applicable → when the value is blank or zero. This helped standardize non-buildable sets and simplified filtering, grouping, and visualization in
    PivotTables and dashboards.

- **Minifigs:** ~10058 are blanks.  
  → Missing values make sense (many sets have no minifigs).  
  → Left blanks as is. I created additional status column (minifigs_status) for minifigs These flags help distinguish between records with valid data and those with blanks,
    making downstream analysis more robust.

- **Age Range:** 11,671 blanks.  
  → Left missing, will analyze later to see if imputation is needed.
  → To improve data quality and make missing values explicit, I created additional status column agerange_min_status. These flags helps in distinguishing between records with valid data and those with blanks,        making downstream analysis more robust.

- **US_retailPrice:** 11,476 blanks, Added a helper column (`US_retailprice_status`) with the formula:
  =IF(TRIM([@[US_retailPrice]])="", "Missing", "Available")
  to classify values as either Missing or Available. Additionally,I converted US_retailPrice to a numeric datatype to ensure accurate aggregation and visualization in PivotTables and dashboards.
  → Fixed formula issues caused by spaces in column name by using structured references correctly.
  
- **bricksetURL, thumbnailURL, and imageURL columns:**
  →The dataset contains URLs for brickset, thumbnail, and image; thumbnailURL and imageURL have missing values for the same sets, while bricksetURL is complete
  → During data cleaning, a helper column thumbnail_image_status was added to indicate whether both thumbnailURL and imageURL were missing for a set. All missing entries occur for the same sets.
 
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
 
• ** Missing Value Strategy **
•	sub_theme:
o	Initially tried blank selection in Excel (Ctrl + G → Special → Blanks).
o	Considered imputation but logically decided not to use 0 (since LEGO sets must have pieces).

•	minifigs:
o	Majority of values are missing → but this is expected (many LEGO sets don’t include minifigs).
o	Decision: leave blanks as "0" or "No Minifigs" depending on downstream use.
•	agerange_min:
o	Noted many missing values but verified valid year range of LEGO sets.
o	No imputation applied yet.
•	US_retailPrice:
o	Found hidden blanks (some cells look empty but aren’t truly blank).
o	Switched to:
o	=IF(LEN(TRIM([@US_retailPrice]))=0,"Missing","Available") to correctly flag missing values.

• Data Type Adjustments
  Converted columns to proper datatypes:
o	bricksetURL, thumbnailURL, imageURL → set as General (URL links) for uniform handling.
  Checked numeric columns like pieces, minifigs, and US_retailPrice to ensure proper numeric formatting.

### 5. Quality Check / Notes for Next Steps
•	Be mindful: adding many derived columns may increase table size but is fine during cleaning — drop or archive derived columns later if needed.
•	Potential next steps:
o	Handle missing values for modeling (decide on 0, "Missing", median, or leave as null depending on the column).
o	Consider feature engineering (e.g., price per piece, minifigs per set).
o	Validate outliers (e.g., unusually high prices or piece counts).
o	Standardize categorical columns (e.g., theme, sub_theme).

**Outcome after above steps**
•	All key columns reviewed.
•	Missing data assessed with logical reasoning per column.
•	Data types standardized for URL and numeric fields.
•	Missing value flagging formulas refined.

