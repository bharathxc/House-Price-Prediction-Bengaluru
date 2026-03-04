# House Price Prediction — Bengaluru

Small exploratory notebook and modeling pipeline for Bengaluru house prices. The repository contains the main analysis notebook and two CSV datasets. The notebook performs cleaning, spatial feature engineering, clustering, model training (Random Forest / XGBoost / linear models), hyperparameter search, and saves trained artifacts with joblib.

Contents
- `price comparison.ipynb` — primary notebook (end-to-end): data load → cleaning → spatial features → clustering → models → SHAP → spatial regression.
- `Bengaluru_House_Data_with_coords.csv` — preferred dataset (includes latitude/longitude).
- `Bengaluru_House_Data.csv` — alternate dataset.
- `.github/copilot-instructions.md` — guidance for AI agents (created/updated).

Quick start (Windows PowerShell)
1. Create and activate a virtual environment (optional but recommended):

   python -m venv .venv
   .\.venv\Scripts\Activate.ps1

2. Install common dependencies (adjust if you use a requirements file):

   pip install pandas numpy matplotlib seaborn geopandas shapely geopy scikit-learn xgboost joblib libpysal spreg jupyterlab

3. Open the notebook:

   jupyter lab

4. IMPORTANT: edit the dataset path in the notebook (first cells). Replace the absolute path with the repo relative path, for example:

   file_path = "./Bengaluru_House_Data_with_coords.csv"

   The notebook currently contains an absolute path that will fail on other machines.

Key conventions & assumptions
- Canonical column names used throughout: `latitude`, `longitude`, `price`, `total_sqft`, `size`, `bath`, `balcony`, `location`.
- Notebook creates `Bedroom` from the `size` column and `price_per_sqft` as `price*100000/total_sqft`.
- Spatial features: distances to landmarks are computed with `geopy.distance.geodesic` and stored with names like `distance_to_electronic_city`.
- Clustering: `KMeans(n_clusters=5, random_state=42, n_init=10)` on `latitude` and `longitude` produces a numeric `cluster` column that is optionally added to model features.
- Scaling: `StandardScaler()` is used for model inputs and persisted with `joblib.dump("scaler.pkl")`.
- Deterministic seeds: most models and clustering use `random_state=42` — preserve this for reproducible runs.

Saved artifacts (not in repo)
- The notebook persists model components with `joblib.dump(...)` (examples: `xgb_model.pkl`, `scaler.pkl`, `kmeans.pkl`). Prefer writing these into a `models/` directory (create it before running) to keep the repo tidy.

Notes about heavy packages and environment
- `geopandas`, `libpysal`, and spatial packages often require system libraries (GEOS, PROJ). If installation fails, consult your OS package manager or use conda: `conda install -c conda-forge geopandas libpysal`.
- `spreg` is part of the PySAL family and may need `libpysal` and other PySAL subpackages.

Recommended quick smoke test (in notebook)
1. Edit `file_path` to the repo CSV.
2. Run the initial import and data-load cells to confirm pandas reads the CSV.
3. Run the preprocessing cells up to creation of `df_model` and a single `RandomForestRegressor(n_estimators=10, random_state=42)` fit on a small sample to confirm training runs.

If you want
- I can add a `requirements.txt` inferred from the notebook imports, create a small `scripts/` inference helper, or update the notebook to use a relative `file_path` automatically. Tell me which and I will add it.

— End
