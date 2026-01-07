# Airbnb Top 10 Listings by Country

This repository contains a data analysis project that ranks Airbnb-style listings globally and extracts the **top 10** listings for every country based on a combined score of rating and number of reviews.

---

## Project Overview

The goal of this project is to identify high-quality and popular stays across multiple countries from a single aggregated dataset. The analysis is implemented in Python using a Jupyter/Colab notebook (`Top10_Airbnbs.ipynb`).

Key outcomes:

- Cleaned and standardized listing-level data (ratings, reviews, country names, etc.). 
- Engineered a composite score to reflect both listing quality and popularity.  
- Generated per-country CSV files containing the top 10 listings in each country.

---

## Dataset

- **Source file**: `airbnb_v2.csv`.
- **Rows / columns**: 12,805 listings with 23 original columns.
- **Example columns**:
  - Identification: `id`, `name`  
  - Quality / engagement: `rating`, `reviews`  
  - Location: `address`, `country`  
  - Property details: `bathrooms`, `bedrooms`, `beds`, `guests`, `studios`, `toiles`  
  - Experience fields: `features`, `amenities`, `safety_rules`, `hourse_rules`  
  - Operational: `host_name`, `host_id`, `checkin`, `checkout`  
  - Financial: `price`  

---

## Methods

### 1. Exploratory Data Analysis

Initial exploration steps:[file:3]

- Loaded `airbnb_v2.csv` into a pandas DataFrame.  
- Inspected structure and data types via `df.head()`, `df.info()`.  
- Checked missing values for each column using `df.isnull().sum()`. 

Findings included:

- 23 columns with mixed types (`int`, `float`, `object`). 
- Null values primarily in `host_name`, `checkin`, and `checkout`. 
- No duplicate listing IDs (`id` column) after checking uniqueness.

### 2. Data Cleaning

To prepare the dataset for ranking, several cleaning steps were applied:

- **Column removal** (not needed for ranking logic):  
  - Dropped: `Unnamed: 0`, `host_name`, `host_id`, `img_links`, `checkin`, `checkout`.
- **Type casting and standardization**:  
  - `rating`:  
    - Replaced `"New"` with `0`.  
    - Converted to numeric (`float`).
  - `reviews`:  
    - Cleaned to ensure numeric integer values (e.g., removing formatting characters if present).
  - `country`:  
    - Stripped whitespace from country names to avoid duplicate categories due to spacing.

These steps ensure that rating and review columns can be used in numeric calculations and that grouping by country is reliable.

### 3. Feature Engineering

A composite metric was created to combine **rating** and **review count** into a single ranking measure:

- New column: `ratingandreviewsfilter`  
- Definition:  
  \[
  \text{ratingandreviewsfilter} = \text{rating} \times \text{reviews}
  \]
- Rationale:  
  - Higher values indicate listings that are both highly rated and frequently reviewed, balancing quality and popularity.

### 4. Ranking Logic

The main analysis loop derives the top 10 listings for each country:

1. Extracted the list of unique countries from the cleaned `country` column.  
2. For each country:  
   - Filtered the DataFrame to that country’s listings.  
   - Sorted the subset in **descending** order of `ratingandreviewsfilter`.  
   - Selected the top 10 listings using `.head(10)`.  

This process was implemented with a simple `for` loop over the country list.

### 5. Output Generation

To make the results easily reusable and shareable:

- Created a directory `csvs/` from within the notebook.
 
- Compressed the entire `csvs/` directory into a single archive:  csvs.zip
  This archive contains one CSV per country, each with exactly 10 highest-ranked listings (or fewer if the country has less than 10 entries).

---

## Technologies Used

- **Language**: Python 3 (Google Colab / Jupyter environment).
- **Libraries**:  
- `pandas` for data loading, cleaning, and transformations.  
- `tqdm` for progress bars while iterating through countries.  
- `shutil` for creating the ZIP archive of result CSVs.

---

## How to Run

1. **Clone / download** this repository.  
2. Ensure `airbnb_v2.csv` is placed in the same directory as `Top10_Airbnbs.ipynb` (or update the path in the notebook).
3. Open `Top10_Airbnbs.ipynb` in Jupyter or Google Colab.  
4. Run all cells in order:  
 - Data loading and EDA.  
 - Cleaning and feature engineering.  
 - Per-country ranking and CSV export.  
5. After execution, check the `csvs/` folder and `csvs.zip` for per-country top 10 files.
