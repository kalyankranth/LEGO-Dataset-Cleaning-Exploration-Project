**LEGO Dataset — Cleaning, Exploration & Excel Dashboard Project**
<img width="1536" height="324" alt="image" src="https://github.com/user-attachments/assets/0f3de237-b4ec-4dd4-bbe5-0412f3db7d7e" />



**LEGO sets dataset (1970–2022): End-to-end data cleaning, exploratory analysis, and dashboarding in Excel**

**📌 Project Overview:**

This project focuses on cleaning, organizing, analyzing, and visualizing a large LEGO dataset containing 18,458 sets released between 1970 and 2022.
The goal was to transform raw, inconsistent data into a high-quality analytical dataset and build an interactive Excel dashboard that reveals pricing trends, theme popularity, and product evolution over time.

**📁 Dataset Description:**

The dataset includes information such as:

- set_id — Unique identifier

- name — LEGO set name

- year — Release year

- theme / sub_theme — Product categorization

- pieces — Number of pieces

- minifigs — Number of minifigures

- age_range — Recommended age
  
- US_retailPrice — Retail price
  
- URLs — bricksetURL, thumbnailURL, imageURL

*The raw file has:*

- Missing values (categorical & numeric)
  
- Mixed data types
  
- Incorrect date parsing
  
- Empty text fields
  
- Zero/blank values that required interpretation
  
- Inconsistent formatting across columns

**Data Cleaning Process:**

The cleaning was completed entirely in Excel using formulas, structured references, helper columns, and consistent validation steps.
Below is the professional summary included in the README.

1. Initial Formatting
Converted set_id from misinterpreted Date to General.
Ensured URL fields were stored as General (not hyperlinks).
Validated core identifiers — no missing year or name.

2. Handling Missing Values (Column-wise Strategy)
   
🔹 Sub-theme (3,557 missing)
Missingness showed a temporal pattern between 1997–2002
Classified as MNAR (Missing Not At Random)
Imputed with "Unspecified" to reflect business reality (sets released without defined subthemes)

🔹 Theme Group (2 missing)
Imputed with “Unspecified” for consistency with the Sub-theme decision.

🔹 Category
Values: Normal, Extended, Random
After EDA, “Random” was retained as a valid category (not noise).

🔹 Pieces (3,924 missing/zero)
Zeros and blanks were meaningful for certain themes (Books, Gear).
Created a helper column:

Pieces_Status:

=IF(OR(ISBLANK(A2),A2=0),"Not Applicable","Has Pieces")

Used this for filtering and analysis instead of forcing imputations.

🔹 Minifigs (~10,058 blanks)
Expected for many sets → left as blank.
Added:
minifigs_status = "Has Minifigs" / "No Minifigs"

🔹 Age Range (11,671 blanks)
Left as-is but created:
agerange_min_status = "Available" / "Missing"

🔹 US_retailPrice (11,476 blanks)
Fixed hidden blanks using:
=IF(LEN(TRIM([@US_retailPrice]))=0,"Missing","Available")

Converted column to numeric to enable aggregation.

🔹 Image & Thumbnail URLs

Both missing together for specific sets → flagged using thumbnail_image_status.

3. Duplicates & Integrity Checks
   
Searched and removed duplicate set_id entries,Final row count preserved (18,458)

4. Standardization
   
- Replaced “?”, “Unnamed”, and similar placeholders with "Unknown"

- Cleaned inconsistent spacing with TRIM()
  
- Created status/flag columns for all key fields:
  
missing_pieces

missing_minifigs

missing_price

missing_agerange

These helper fields greatly improved dashboard filtering.


**📊 Exploratory Analysis (Pivot Tables)**

_**Key insights:**_

**1. Price Trends**
   
- Serious Play has the highest average price

- Star Wars continues to release the highest-priced sets

- Decade-wise average price steadily increases over time

**2. Theme Popularity**

- Gear has the highest number of sets overall
  
- Duplo is consistently the second most released theme
  
- Star Wars remains a dominant, high-value product line
  
**3. Decade-Level Observations**

- 1970s–1990s → Train series had high maximum prices
- 2000s → Star Wars becomes a key premium line
- 2010s → Serious Play emerges as a high-avg-price theme
- 2020s → Icons and Minifigs themes rise

**📈 Excel Dashboard**

The final dashboard includes:- 
Filters
Year
Theme
Sub-theme
Category
Theme Group
Decade
Visualizations
Clustered Column Chart
Average/Max/Min Price per Theme
Decade Trend Line
Average price evolution over time
Scatter Plot
Pieces vs. Price (value assessment)
Theme Popularity Bar Chart
Count of sets per theme
KPI Cards
Total Number of Sets
Highest Priced Set
Lowest Priced Set
Average Price
Average Minimum Age
Theme with Highest Count.

**Files Included:**

1.lego_dashboard.xlsx — Interactive Excel dashboard, with cleaned dataset, insights, dashboard and detailed cleaing steps

2.raw_dataset.csv — Original file

**📘 Learnings:**

- CSV files do not preserve formatting, formulas, or data types, so switched to .xlsx for reliable data handling

- Structured references dramatically reduce formula errors

- Missingness must be treated based on business meaning, not blindly imputed

- Helper/status columns make dashboards far more robust

- Excel is powerful enough for large-scale analytical workflows when structured properly

**🔚 Conclusion:**

This project demonstrates my end-to-end Excel-based data analytics like _**Data cleaning, Missing value strategy, Feature engineering, Pivot-table-based EDA, Interactive dashboards, Business insights, Clear documentation.**_

**Note:**
“Excel sheets are locked to preserve formatting and formulas. Data remains fully viewable.”


