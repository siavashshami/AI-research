# Machine Learning Models for InSAR Displacement Prediction and Evaluation

A collection of Python scripts for training, evaluating, and visualizing machine-learning regression models — including **LASSO**, **KNN**, **GBR**, and **RFR** — for predicting surface displacement rates from geospatial and thematic features. The scripts cover model benchmarking, factor importance analysis, error distribution analysis, and publication-quality visualization.

---

## 1. Models (Feature Importance Issue)

### Overview

This Python script trains and evaluates four regression models — **LASSO**, **K-Nearest Neighbors (KNN)**, **Gradient Boosting Regressor (GBR)**, and **Random Forest Regressor (RFR)** — for predicting surface velocity (`vel`) from geospatial features.

The script performs 9 independent train/test splits and accumulates the evaluation metrics (RMSE, MAE, R²) for each model, providing a stability assessment of model performance across repeated random splits.

### Input Data

* A CSV file (referenced as `File pass`) containing:

  * Features (all columns except identifiers and target).
  * Target column: `vel` (displacement velocity).
  * Non-feature columns dropped: `FID`, `FID_Points`, `FID_Mian_F`, `FID_1`, `vel`, `coh`, `std`, `LatUTM`, `LongUTM`.

### Method

Four regression models are used:

* **LASSO** with `alpha=1.0`.
* **K-Nearest Neighbors** with `n_neighbors=8`.
* **Gradient Boosting Regressor** with `n_estimators=300, max_depth=7`.
* **Random Forest Regressor** with `n_estimators=300, max_depth=8`.

For each of 9 iterations:

* Data is split into train/test (70/30) with shuffling.
* Each model is fitted and evaluated on the test set.
* RMSE, MAE, and R² are computed and appended to per-model lists.

Evaluation metrics:

* RMSE (Root Mean Squared Error)
* MAE (Mean Absolute Error)
* R² (coefficient of determination, in percent)

### Requirements

* Python 3
* NumPy
* Pandas
* scikit-learn

### Output

Per-iteration console output for each model (`LASSO`, `Grad`, `RFR`, `KNN`) and the current stage number. The accumulated metric lists can be used for statistical comparison and box-plot visualization of model stability.

---

## 2. R2_RMSE_MAE

### Overview

This Python script generates a **three-panel bar chart** comparing the performance of four regression models — **LASSO**, **KNN**, **GBR**, and **RFR** — using three evaluation metrics: **R²**, **RMSE**, and **MAE**.

Each panel corresponds to one metric, and the bar values are hard-coded based on prior model evaluation.

### Input Data

The metric values are hard-coded in the script:

* **R²**: LASSO=0, KNN=0.84, GBR=0.85, RFR=0.77
* **RMSE**: LASSO=1.74, KNN=0.67, GBR=0.67, RFR=0.82
* **MAE**: LASSO=1.03, KNN=0.47, GBR=0.48, RFR=0.61

### Method

The script uses a three-subplot layout (`1×3`):

* **Panel 1**: R² (%) — `aquamarine` bars.
* **Panel 2**: RMSE (mm) — `lightseagreen` bars.
* **Panel 3**: MAE (mm) — `deepskyblue` bars.

Additional styling:

* Times New Roman font, 500 DPI output.
* Bold, large tick labels (size 25) and titles (size 35).
* Value annotations above each bar with a white background box.
* Dashed horizontal grid lines.

### Requirements

* Python 3
* Matplotlib
* Seaborn
* Pandas
* NumPy

### Output

A three-panel publication-quality bar chart comparing R², RMSE, and MAE across the four models.

---

## 3. Factor Importance

### Overview

This Python script generates a **grouped bar chart** of feature importance values for three regression models — **KNN**, **GBR**, and **RFR**.

Each feature is represented by three bars (one per model), with numeric values displayed above each bar, allowing direct comparison of how different models rank the same predictors.

### Input Data

Feature importance values are hard-coded:

| Feature | KNN   | GBR   | RFR   |
|---------|-------|-------|-------|
| DEM     | 0.057 | 0.047 | 0.028 |
| Slope   | 0.021 | 0.004 | 0.001 |
| Fault   | 0.410 | 0.490 | 0.530 |
| River   | 0.088 | 0.019 | 0.005 |
| Road    | 0.008 | 0.013 | 0.011 |
| PRC     | 0.210 | 0.410 | 0.410 |
| LU      | 0.190 | 0.009 | 0.006 |

### Method

The script uses a grouped bar-plot layout:

* X-axis: features (`DEM`, `Slope`, `Fault`, `River`, `Road`, `PRC`, `LU`).
* Y-axis: factor importance values.
* Three bars per feature (KNN in `aquamarine`, GBR in `lightseagreen`, RFR in `deepskyblue`).

Additional styling:

* Times New Roman font, 500 DPI output.
* Value annotations above each bar with white boxes and black edges.
* Y-limit fixed at 0.6.
* Dashed horizontal grid lines.
* Legend in the upper-left corner with shadow.

### Requirements

* Python 3
* Pandas
* Matplotlib

### Output

A single grouped bar chart showing feature importance for KNN, GBR, and RFR, ready for publication-quality figures.

---

## 4. Histogram

### Overview

This Python script generates a **histogram of displacement rates** from a CSV file, with reference lines for the mean and standard deviation.

The histogram visualizes the statistical distribution of the `vel` column (displacement velocity in mm/year), helping assess the central tendency and spread of the data.

### Input Data

* A CSV file (referenced as `File pass`) containing a column named `vel` (displacement rate in mm/year).

### Method

The script:

* Plots a histogram with 100 bins (`lightseagreen` bars with white edges).
* Adds a vertical dashed red line for the mean.
* Adds a vertical dashed gold line for the standard deviation.
* Applies a Times New Roman font and 500 DPI output.
* Prints the mean and standard deviation to the console.

### Requirements

* Python 3
* Pandas
* NumPy
* Matplotlib

### Output

A histogram plot with:

* Frequency on the y-axis.
* Displacement rate (mm/year) on the x-axis.
* Mean and standard deviation reference lines.
* Legend in the upper-left corner with shadow.
* Dashed grid lines for readability.

The script also prints the numerical mean and standard deviation of the `vel` column.