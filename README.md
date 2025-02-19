### README: Overdose Data Processing Workflow

This repository contains a Python script that processes overdose data using `geopandas` and `arcpy`. The script performs several key tasks, including spatial joins, data aggregation, and field updates. Below is a detailed description of what the script does:

---

#### **1. Data Loading and CRS Conversion**
- **Input Files**:
  - `fatal.shp`: Shapefile containing fatal overdose data.
  - `nonfatal.shp`: Shapefile containing non-fatal overdose data.
  - `hexbin.shp`: Shapefile containing hexbin grid data.
- **Steps**:
  - Loads the shapefiles into GeoDataFrames using `geopandas`.
  - Checks the coordinate reference system (CRS) of each dataset.
  - Converts the CRS of all datasets to `EPSG:4269` (NAD83) if they are not already in this CRS.
  - Prints the CRS of each dataset after conversion for verification.

---

#### **2. Spatial Join of Overdose Data with Hexbins**
- **Objective**:
  - Spatially join the fatal and non-fatal overdose points with the hexbin grid to associate each overdose incident with a specific hexbin.
- **Steps**:
  - Uses `geopandas.sjoin` to perform an inner spatial join between the overdose points and the hexbin grid.
  - Renames the `index_right` column (created during the join) to avoid conflicts.
  - Truncates column names to 10 characters to comply with Shapefile limitations.
  - Saves the joined datasets as new shapefiles:
    - `fatal_with_hexbins.shp`
    - `nonfatal_with_hexbins.shp`

---

#### **3. Aggregating Overdose Counts by Hexbin**
- **Objective**:
  - Count the number of fatal and non-fatal overdose incidents within each hexbin.
- **Steps**:
  - Loads the spatially joined datasets (`fatal_with_hexbins.shp` and `nonfatal_with_hexbins.shp`).
  - Counts the occurrences of each `grid_id` (hexbin identifier) in both datasets.
  - Merges the counts into the original hexbin dataset.
  - Fills missing values with `0` for hexbins with no overdose incidents.
  - Saves the updated hexbin dataset as `hexbin_with_counts.shp`.

---

#### **4. Updating the Fatal Overdose Dataset with Substance Types**
- **Objective**:
  - Add a new field `type` to the fatal overdose dataset to indicate the substances involved in each overdose incident.
- **Steps**:
  - Loads the `fatal.shp` dataset.
  - Adds a new field `type` if it doesn't already exist.
  - Iterates through each row and checks the presence of specific substances (e.g., opioid, fentanyl, cocaine, alcohol).
  - Populates the `type` field with a comma-separated string of substances involved in the overdose.
  - Saves the updated dataset as `fatal_overdose_updated.shp`.

---

#### **Output Files**
1. **Spatially Joined Datasets**:
   - `fatal_with_hexbins.shp`: Fatal overdose data joined with hexbin grid.
   - `nonfatal_with_hexbins.shp`: Non-fatal overdose data joined with hexbin grid.
2. **Aggregated Hexbin Dataset**:
   - `hexbin_with_counts.shp`: Hexbin grid with counts of fatal and non-fatal overdose incidents.
3. **Updated Fatal Overdose Dataset**:
   - `fatal_overdose_updated.shp`: Fatal overdose data with a new `type` field indicating substances involved.

---

#### **Dependencies**
- Python libraries:
  - `geopandas`
  - `arcpy` (optional, for ArcGIS-specific functionality)
  - `os` (for file path operations)

---

#### **Usage**
1. Place the input shapefiles (`fatal.shp`, `nonfatal.shp`, `hexbin.shp`) in the specified folder (`C:\Users\l.dumarevskaya.ctr\ArcGIS projects\overdose`).
2. Run the script to process the data.
3. Check the output folder for the generated shapefiles.

---

#### **Notes**
- Ensure all input shapefiles have consistent attribute fields (e.g., `grid_id`, `opioid`, `fentanyl`, etc.).
- The script assumes the input data is in a projected coordinate system compatible with `EPSG:4269`.
- If using ArcGIS, ensure the `arcpy` environment is properly configured.

---

This workflow is designed to support spatial analysis of overdose incidents by associating them with a hexbin grid and aggregating counts for further analysis.
