# ☕ Starbucks Beverage Nutrition Facts - Data Cleaning & Machine Learning Presentation

[![Google Colab](https://img.shields.io/badge/Google%20Colab-Notebook-orange?logo=googlecolab)](https://colab.research.google.com/drive/1LSBDzQ6TuoOs4eaG31sSg0YGPAa_8uDs)
[![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python)](https://www.python.org/)
[![Pandas](https://img.shields.io/badge/Pandas-Data%20Cleaning-150458?logo=pandas)](https://pandas.pydata.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Machine%20Learning-F7931E?logo=scikit-learn)](https://scikit-learn.org/)

This repository contains the complete 7-part presentation, data cleaning pipeline, exploratory data analysis (EDA), and machine learning classification models for the **Starbucks Beverage Nutrition Facts** dataset (`StarBucksNutritionFacts.csv.xlsx`), developed using Google Colab.

---

## 📂 Repository Contents

| File Name | Description |
| :--- | :--- |
| 📓 [`Starbucks_Data_Cleaning_and_Analysis.ipynb`](Starbucks_Data_Cleaning_and_Analysis.ipynb) | Complete 25-step Google Colab Jupyter Notebook |
| 📊 [`Starbucks_Nutrition_Facts_Cleaned.csv`](Starbucks_Nutrition_Facts_Cleaned.csv) | Post-cleaned dataset in CSV format |
| 🌐 [`Starbucks_Nutrition_Presentation.html`](Starbucks_Nutrition_Presentation.html) | Interactive HTML Slide Deck Presentation |
| 📄 [`presentation_slides.md`](presentation_slides.md) | 7-Part Presentation Markdown Document |
| 📁 [`StarBucksNutritionFacts.csv.xlsx`](StarBucksNutritionFacts.csv.xlsx) | Original raw dataset Excel file |

---

## 📊 Presentation Overview (7 Required Sections)

### 1. Dataset Title
* **Dataset Title:** Starbucks Beverage Nutrition Facts (`StarBucksNutritionFacts.csv.xlsx`)
* **Output File:** `Starbucks_Nutrition_Facts_Cleaned.csv`
* **Domain:** Food Science, Beverage Nutrition Profiling, and Health Analytics

### 2. How the Data Was Acquired
* **Source:** Compiled from Starbucks Corporation's official Nutritional Guidance menu disclosures.
* **Colab Acquisition Method:** Loaded into Google Colab using `from google.colab import files; uploaded = files.upload()` and parsed via `pd.read_excel("StarBucksNutritionFacts.csv.xlsx")`.

### 3. Present Dataset (Initial Overview)
* **Raw Shape:** 242 rows &times; 18 columns (3 Categorical, 15 Numerical).
* **Key Features:** `Beverage_category`, `Beverage`, `Beverage_prep`, `Calories`, `Total Fat (g)`, `Sugars (g)`, `Protein (g)`, `Caffeine (mg)`, and % Daily Values of Vitamins A/C, Calcium, and Iron.

### 4. Processes Used to Clean the Data
* **Missing Data Identification:**
  * Discovered 1 explicit `NaN` in `Caffeine (mg)` (Row 158).
  * Discovered 22 implicit `"Varies"` text placeholders in `Caffeine (mg)`.
  * Identified typo `'3 2'` (space instead of decimal point) in `Total Fat (g)` at Row 237.
  * Identified whitespace pollution across column headers and string fields.
* **Duplicates Check:** `df.duplicated().sum()` = **0 exact duplicate rows**. Verified all 242 rows represent distinct menu recipes.

### 5. Tools or Methods Used to Clean It
```python
# 1. Strip column header whitespace
df.columns = df.columns.str.strip()

# 2. Trim whitespace in text columns
for col in df.select_dtypes(include=['object', 'string']).columns:
    df[col] = df[col].astype(str).str.strip()

# 3. Correct typo '3 2' -> '3.2' in Total Fat
df['Total Fat (g)'] = df['Total Fat (g)'].str.replace('3 2', '3.2').astype(float)

# 4. Coerce text 'Varies' in Caffeine to numeric NaN
df['Caffeine (mg)'] = pd.to_numeric(df['Caffeine (mg)'], errors='coerce')

# 5. Impute missing values with Category Median
numeric_columns = df.select_dtypes(include=np.number).columns
for col in numeric_columns:
    df[col] = df[col].fillna(df[col].median())
```

### 6. After Cleaning (Post-Cleaned Data)
* **Post-Cleaned Shape:** 242 rows &times; 18 columns | **0 Missing Values**
* **Summary Metrics:**
  * `Calories`: 0 to 510 kcal (Mean: 193.87, Median: 185.0)
  * `Sugars (g)`: 0 to 84g (Mean: 32.96, Median: 32.0)
  * `Caffeine (mg)`: 0 to 410mg (Mean: 87.58, Median: 75.0)

### 7. Conclusion
* Restored 100% numerical feature formatting and eliminated whitespace errors.
* Identified highest calorie/sugar density in blended Frappuccinos & Smoothies, and highest caffeine density in Brewed Coffee.
* Deployed **Decision Tree** and **Random Forest** classification models in Google Colab to predict beverage categories.

---

## 🚀 How to Run in Google Colab

1. Open [Google Colab](https://colab.research.google.com/).
2. Click **Upload** and choose [`Starbucks_Data_Cleaning_and_Analysis.ipynb`](Starbucks_Data_Cleaning_and_Analysis.ipynb).
3. Run all cells (`Ctrl + F9`). When prompted, upload `StarBucksNutritionFacts.csv.xlsx`.
