# Machine Learning Scripts for InSAR Displacement Analysis

A collection of Python scripts for machine-learning-based surface displacement prediction, model evaluation, error analysis, and visualization of InSAR results using Random Forest Regressor (RFR) and Support Vector Regressor (SVR).

---

## 1. RFR and SVR Code Shahgheyb

### Overview

This Python script trains and evaluates two machine-learning regression models — **Random Forest Regressor (RFR)** and **Support Vector Regressor (SVR)** — for predicting surface velocity (`Vel`) from geospatial features.

The workflow reads two datasets: one for training (`2016.csv`) and one for testing (`2020.csv`). After dropping non-feature columns (e.g., `FID`, `pointid`, and identifier fields), the models are fitted on the 2016 data and evaluated on the 2020 data.

The script first runs a single RFR and a single SVR fit, computes standard regression metrics (RMSE, MAE, R², and Pearson correlation), and then runs the RFR model 100 times to accumulate correlation coefficients and other metrics for stability analysis.

### Input Data

* `2016.csv` — Training dataset (features + `Vel` target).
* `2020.csv` — Test dataset (features + `Vel` target).

### Method

Two regression models are used:

* **Random Forest Regressor** with `n_estimators=100`.
* **Support Vector Regressor** with an RBF kernel and `C=100`.

Evaluation metrics:

* RMSE (Root Mean Squared Error)
* MAE (Mean Absolute Error)
* R² (coefficient of determination, in percent)
* Pearson correlation coefficient (in percent)

The RFR model is then repeated 100 times to collect correlation coefficients and metric lists for statistical assessment.

### Requirements

* Python 3
* NumPy
* Pandas
* scikit-learn

### Output

The script prints:

* RMSE and correlation for the initial RFR run.
* RMSE and correlation for the SVR run.
* Progress output for each of the 100 RFR iterations.

The accumulated lists (`AR_RF`, `AR2_RF`, `ARMSE_RF`, `AMAE_RF`) can be used for further statistical analysis.

---

## 2. BarPlot Extraction Shahgheyb

### Overview

This Python script generates a **grouped bar chart** comparing the RMSE values of two regression models — **Random Forest Regressor (RFR)** and **Support Vector Regressor (SVR)** — across 29 different scenarios.

Each scenario is represented by a pair of bars (one for RFR and one for SVR), with numeric labels displayed above each bar. Two specific bars (scenario 15 for RFR and scenario 18 for SVR) are highlighted with a red edge to emphasize the best-performing cases.

### Input Data

The RMSE values are hard-coded in the script:

* `RMSE_RFR` — RMSE values for the RFR model across 29 scenarios.
* `RMSE_SVR` — RMSE values for the SVR model across 29 scenarios.

Scenario labels are defined in the `labels` list (`Senario 1` to `Senario 29`).

### Method

The script uses a grouped bar-plot layout:

* X-axis: scenarios (1 to 29).
* Y-axis: RMSE values.
* Two bars per scenario (RFR in `royalblue`, SVR in `greenyellow`), both with black edges.

Additional styling:

* Bold fonts, 500 DPI output.
* Rotated x-axis labels (90°) and large tick sizes.
* Grid lines on the y-axis.
* Value annotations above each bar.
* Red edge highlights on selected bars to indicate best scenarios.

### Requirements

* Python 3
* Matplotlib
* NumPy

### Output

A single high-resolution grouped bar chart showing RMSE for both models across all 29 scenarios, ready for publication-quality figures.

---

## 3. Calculation NaN and Error

### Overview

This Python script analyzes a column of residual values (`Diff SVR`) in a CSV file and classifies each value into one of three categories:

* **Within range** — values between −3 and 3.
* **Outside range** — values outside the [−3, 3] interval.
* **NaN values** — missing entries.

This is useful for evaluating the error distribution of an SVR-based prediction against a reference (e.g., InSAR displacement).

### Input Data

* `UD-BB'.csv` — CSV file containing a column named `Diff SVR`.

### Method

The script iterates row-by-row and:

* Checks for NaN using `np.isnan`.
* Classifies values as within range or outside range based on the [−3, 3] threshold.

Each category is stored in a separate list.

### Requirements

* Python 3
* Pandas
* NumPy

### Output

The script prints:

* `Values within range`
* `Values outside range`
* `NaN values`

These lists can be used for further statistical analysis or reporting.

---

## 4. Histogram Shahgheyb

### Overview

This Python script generates a **histogram with an overlaid Gaussian distribution curve** for a set of vertical displacement values stored in a CSV file.

The histogram visualizes the statistical distribution of the `UD2020` column, and the fitted normal curve helps assess how closely the data follows a Gaussian distribution.

### Input Data

* `Histo.csv` — CSV file containing a column named `UD2020` (vertical displacement values).

### Method

The script:

* Plots a density histogram with 100 bins.
* Computes the mean and standard deviation.
* Overlays a theoretical Gaussian curve based on the computed mean and standard deviation.
* Adds a vertical dashed line at zero for reference.
* Displays statistical annotations (Mean, Variance, Max, Min) inside a text box.

### Requirements

* Python 3
* Pandas
* NumPy
* Matplotlib
* statistics

### Output

A histogram plot with:

* Density-normalized bars (`mediumturquoise`).
* Gaussian fit line (`darkblue`).
* Statistical summary text box.
* Grid lines for readability.

The script also prints the mean, variance, minimum, and maximum of the `UD2020` column.

---

## 5. LinePlot With Coloring Background Shahgheyb

### Overview

This Python script generates a **two-panel line plot** comparing residuals of two regression models — **RFR** and **SVR** — against InSAR observations along a profile.

Each panel corresponds to one model (top: RFR, bottom: SVR). The background of each panel is colored based on the residual value at each point:

* **Green** for residuals within [−3, 3].
* **Red** for residuals outside this range.

Vertical dashed lines and labeled markers (`i` and `i'`) highlight reference positions along the profile.

### Input Data

* `EW-AA'.csv` — CSV file containing:

  * `Diff RFR` — residuals of the RFR model.
  * `Diff SVR` — residuals of the SVR model.
  * `Dist km` — distance along the profile (for x-axis labels).

### Method

The script:

* Creates a two-row shared-x figure using `GridSpec`.
* Iterates over residual values and applies `axvspan` coloring for each point.
* Plots the residual series in black.
* Adds horizontal zero-lines, vertical reference lines, and text labels for `i` and `i'`.
* Uses bold fonts and 500 DPI output for publication-quality figures.

### Requirements

* Python 3
* Pandas
* NumPy
* Matplotlib

### Output

A two-panel line plot showing residuals of RFR and SVR along the profile, with background coloring to indicate values within or outside the ±3 mm range.

---

## 6. Map Generation Shahgheyb

### Overview

This Python script generates a **spatial prediction map** of surface velocity (`Vel`) using a trained **Random Forest Regressor (RFR)** and a stack of topographic rasters (DEM, slope, aspect, curvature, TWI).

The trained model predicts velocity values for every pixel in the raster stack, and the resulting prediction grid is saved as a GeoTIFF.

### Input Data

* `EW2016Domecsv.csv` — Training dataset (features + `Vel` target).
* Raster files:

  * `DEM.tif`
  * `slope1.tif`
  * `aspect1.tif`
  * `curveture1.tif`
  * `TWI.tif`

The CSV features exclude identifier columns and additional thematic layers (`NDWI`, `LST`, `Fault`, `Aquifer`, `River`, `PRC`, `Earthquake`, `Hole`, `Lithology`).

### Method

The script:

* Trains an RFR model (`n_estimators=100`) on the CSV features and `Vel` target.
* Reads the five raster layers using GDAL.
* Stacks the rasters into a 3D array.
* Flattens the stack and predicts velocity for all pixels.
* Reshapes the predictions into a 2D grid (163 × 181).
* Writes the result as a GeoTIFF using the DEM's transform and CRS.

### Requirements

* Python 3
* NumPy
* Pandas
* GDAL (`osgeo.gdal`)
* Rasterio
* Matplotlib
* scikit-learn

### Output

* `RFR_EW_Senario15_Topo_Map.tif` — GeoTIFF containing the predicted velocity map.
* A quick visualization of the DEM via `plt.imshow`.

This workflow is useful for producing scenario-based spatial predictions of surface deformation from topographic and thematic predictors.