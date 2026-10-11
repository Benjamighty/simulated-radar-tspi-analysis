# Simulated Radar TSPI Accuracy Analysis

## Overview

This project evaluates the positional accuracy of **simulated radar tracking observations** against a **simulated reference truth trajectory**. It demonstrates a practical Time-Space-Position Information (TSPI) analysis workflow using Python: inspect and clean time-series data, align observations by timestamp, calculate positional errors, and communicate results through summary statistics and visualizations.

> **Data notice:** Both datasets are generated for educational and portfolio use. They are not operational radar measurements or independently validated reference data.

## Objectives

- Compare radar-reported positions with a reference truth trajectory.
- Align observations taken at different times using time-based interpolation of truth positions.
- Calculate signed East, North, and Up errors, as well as horizontal and three-dimensional position errors.
- Quantify mean, median, standard deviation, maximum error, and root mean square error (RMSE).
- Visualize trajectories, error magnitudes over time, and directional errors over time.

## Tools

- Python
- pandas
- NumPy
- Matplotlib
- Jupyter Notebook

## Repository Structure

```text
simulated-radar-analysis/
├── data/
│   ├── truth.csv
│   └── radar_raw.csv
├── notebook/
│   └── simulated_analysis.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## Data and Methodology

The datasets describe one simulated track over approximately ten minutes. Positions use a shared **East-North-Up (ENU)** coordinate frame, in meters, and timestamps are in **UTC**.

1. **Inspect the datasets:** Check schema, missing values, timestamp order, and duplicate timestamps.
2. **Clean the data:** Remove three radar observations with duplicate timestamps and sort both datasets chronologically. The cleaned radar dataset contains **118 observations**.
3. **Align timestamps:** Of the 118 radar timestamps, **60** coincide with truth timestamps and **58** require interpolated truth positions. Interpolation is applied to the truth trajectory, not the radar observations.
4. **Calculate signed coordinate errors:** Radar position minus aligned truth position, separately for East, North, and Up.
5. **Calculate position error magnitudes:**

   - Horizontal error: `sqrt(E_error^2 + N_error^2)`
   - 3D error: `sqrt(E_error^2 + N_error^2 + U_error^2)`

6. **Summarize and visualize:** Calculate descriptive statistics and RMSE, then plot the trajectories, position error magnitudes, and directional errors over time.

Missing radar position components are retained in the cleaned dataset and excluded only from calculations requiring those components. Consequently, horizontal and 3D metrics have different valid sample counts.

## Results

| Metric | Horizontal error | 3D error |
|---|---:|---:|
| Valid observations | 115 | 114 |
| Mean | 34.46 m | 37.34 m |
| Median | 33.26 m | 35.72 m |
| Standard deviation | 15.00 m | 14.41 m |
| Maximum | 73.75 m | 73.98 m |
| RMSE | **37.55 m** | **40.00 m** |

### Mean Signed Coordinate Errors

| Coordinate | Mean signed error |
|---|---:|
| East | +23.86 m |
| North | −13.95 m |
| Up | +8.39 m |

The radar observations generally follow the simulated reference trajectory but show systematic average offsets toward the east, south, and upward directions.

## How to Run

1. Clone or download the repository and open the project folder in VS Code.
2. Create and activate a Python virtual environment:

   **Windows (PowerShell):**
   ```powershell
   py -m venv .venv
   .\.venv\Scripts\Activate.ps1
   ```

   **macOS / Linux:**
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```

3. Install dependencies:

   ```bash
   python -m pip install -r requirements.txt
   ```

4. Open `notebook/simulated_analysis.ipynb` in VS Code, select the virtual environment as the notebook kernel, and run all cells from top to bottom.

**File paths:** The notebook must read the CSVs from the `data/` folder. Depending on the notebook's working directory, paths may need to be `data/truth.csv` and `data/radar_raw.csv` (project root) or `../data/truth.csv` and `../data/radar_raw.csv` (notebook folder). Check the paths in the notebook if files are not found.

## Limitations

- All observations are simulated; results should not be interpreted as operational radar performance.
- The analysis covers a single simulated track over approximately ten minutes.
- Interpolating truth positions assumes approximately linear movement between neighboring truth samples.
- Some radar observations have missing position components, reducing valid counts for certain metrics.
- No system accuracy specification or acceptance threshold was provided; the analysis characterizes error but does not establish pass/fail performance.

## Project Scope

This is an educational TSPI accuracy analysis, not a calibrated radar performance assessment. Bias correction and multi-track analysis are outside the scope of this version.
