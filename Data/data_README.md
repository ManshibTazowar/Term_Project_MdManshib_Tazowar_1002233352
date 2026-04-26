# Data Documentation

## Overview
This directory contains the datasets used for predicting bridge foundation type (classification) and pier total depth (regression). The data was collected from field observations and structural records for 71 bridge instances.

Because the dataset is small ($n=71$), aggressive feature selection was employed using a combination of Correlation Matrices, Random Forest Feature Importance, PLS-VIP scoring, and structural engineering judgment.

## Directory Structure
* `raw_data/`: Contains the original, unedited CSV exports.
* `processed_data/`: Contains the cleaned datasets with engineered features, reduced dimensionality, and standardized missing value artifacts, ready for model ingestion.

---

## 1. Raw Data (`raw_data/`)
**Files:**
* `raw_classification_dataset.csv`
* `raw_prediction_dataset.csv`

**Description:**
The raw datasets consist of 71 bridge records. Each record contains 4 identification columns, 25 structural/geometric predictor columns, and 1 target variable.
* Contains missing values and "Unknown" text artifacts.
* `No of Lanes` and `Design Load` inconsistencies have been structurally aligned across both datasets.

---

## 2. Processed Data (`processed_data/`)
**Files:**
* `processed_classification_dataset.csv`
* `processed_prediction_dataset.csv`

**Preprocessing Steps Applied:**
1. **Identifier Removal:** Removed `SI`, `Asset Name`, `NBI 007`, and `NBI 006`.
2. **Artifact Handling:** Text artifacts denoting missing data (e.g., "U", "Unknown", " ") were standardized to `NaN`.
3. **Engineering Judgment:** `Rail Type` and `Rail Height (in)` were dropped due to high missingness and lack of physical correlation to foundation loads.
4. **Feature Engineering:** `Years Build` was transformed into `Bridge Age` (calculated as 2025 - Year Built) to better quantify deterioration exposure over time.
5. **Dimensionality Reduction:** To comply with the 10x rule of thumb for small datasets, features were restricted to the top 7 most mathematically significant structural parameters.

**CRITICAL PIPELINE NOTE - DATA LEAKAGE PREVENTION:**
To prevent data leakage during model training, **Missing Value Imputation (Median/Mode) and Z-Score Scaling have deliberately been omitted from the processed datasets.**
Missing values (e.g., in `Girder Type`) remain as `NaN` in these files. Imputation and scaling are handled dynamically inside the cross-validation pipelines in `src/model.py`.

### Final Features - Classification Task
* **Target:** `Foundation Type` (Deep vs. Shallow)
* **Predictors (7):**
  1. `Bridge Age`
  2. `Pier Shape`
  3. `Max Span Length (ft)`
  4. `Girder Type` (Contains `NaN` - Imputed in modeling pipeline)
  5. `Roadway Width (ft)`
  6. `Total Length (ft)`
  7. `Pier size/dia (in)`

### Final Features - Prediction Task
* **Target:** `Pier total Depth (ft)`
* **Predictors (7):**
  1. `Pier unbraced length (ft)`
  2. `Max Span Length (ft)`
  3. `Total Length (ft)`
  4. `Pier size/dia (in)`
  5. `Avg Daily Trafic`
  6. `Regulatory Speed (mph)`
  7. `Bridge Age`
