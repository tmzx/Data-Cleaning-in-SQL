# Data Cleaning Project: Layoffs Dataset

This repository contains SQL scripts and documentation for cleaning and preparing the world_layoffs dataset for analysis. The project addresses common data quality issues to create a structured and reliable dataset suitable for visualization and reporting.

---

## Overview

The project focuses on transforming raw data by:

- Removing duplicates.
- Standardizing inconsistent fields.
- Handling null or blank values.
- Converting and formatting date fields.

The cleaned dataset ensures accuracy, consistency, and usability in analytics workflows.

---

## Features

### 1. Duplicate Removal

Duplicates in the dataset were identified and removed using SQL’s `ROW_NUMBER()` function with partitioning.

**Snippet:**


WITH duplicate_cte AS (
    SELECT *,
           ROW_NUMBER() OVER (
               PARTITION BY company, location, industry, total_laid_off, percentage_laid_off, `date`, stage, country, funds_raised_millions
           ) AS row_num
    FROM layoffs_staging
)
DELETE FROM duplicate_cte
WHERE row_num > 1;

### 2. Data Standardization

Inconsistent text fields (e.g., industry and country names) were standardized for uniformity using trimming and string updates.

**Snippet:**


UPDATE layoffs_staging2
SET industry = 'Crypto'
WHERE industry LIKE 'Crypto%';

UPDATE layoffs_staging2
SET country = TRIM(TRAILING '.' FROM country)
WHERE country LIKE 'United States%';


### 3. Null Value Handling

Null or blank values were replaced with meaningful defaults or inferred values using JOINs and updates.

**Snippet:**


UPDATE layoffs_staging2 t1
JOIN layoffs_staging2 t2
    ON t1.company = t2.company
SET t1.industry = t2.industry
WHERE t1.industry IS NULL;


### 4. Date Conversion and Formatting

Dates stored as text were converted into proper `DATE` types using `STR_TO_DATE()` and formatted uniformly.

**Snippet:**


UPDATE layoffs_staging2
SET `date` = STR_TO_DATE(`date`, '%m/%d/%Y');

ALTER TABLE layoffs_staging2
MODIFY COLUMN `date` DATE;


---

## Tools and Techniques

- **SQL**: Core language for querying, transforming, and cleaning data.
- **Staging Tables**: Used to preserve original data and track changes.
- **SQL Functions**: Leveraged `ROW_NUMBER()`, `TRIM()`, `STR_TO_DATE()`, and others for efficient processing.

---

## Results

- Duplicate-free, standardized dataset ready for visualization and reporting.
- Enhanced data consistency and usability for downstream analytics.

---

## Key Learnings

- SQL automation simplifies repetitive cleaning tasks.
- Preserving data integrity through staging tables is critical.
- Upfront data standardization minimizes downstream errors.

---

## Next Steps

1. Use the cleaned dataset for visualization and insight generation.
2. Enhance scripts for automated data cleaning workflows.
3. Explore advanced techniques for data validation and transformation.

---

### How to Use

1. Clone this repository.
2. Execute the SQL scripts in your preferred database environment.
3. Review the cleaned dataset for analysis or export.

---

### Contact

For questions or feedback, feel free to reach out!
