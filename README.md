# Point-dataset-analysis
Point dataset analysis (health-related data)
### Project: Overdose Case Analysis and Hexbin Aggregation

This project processes spatial data related to fatal and non-fatal overdose cases, aggregating the counts into a hexbin grid for visualization and analysis. The workflow uses `geopandas` for efficient spatial data manipulation and avoids dependencies on ArcGIS or `arcpy`.

#### Key Features:
- **Input Data**:
  - `fatal_overdose_updated.shp`: Shapefile containing fatal overdose cases.
  - `nonfatal.shp`: Shapefile containing non-fatal overdose cases.
  - `hexbin.shp`: Shapefile containing a hexbin grid for spatial aggregation.
- **Output Data**:
  - `hexbin_with_counts.shp`: Updated hexbin grid with counts of fatal and non-fatal cases for each grid cell.
- **Workflow**:
  1. Counts the occurrences of each `grid_id` in both fatal and non-fatal datasets.
  2. Merges the counts into the hexbin grid.
  3. Handles missing values by setting counts to `0` for grid cells with no cases.
  4. Saves the updated hexbin grid to a new shapefile.

#### Requirements:
- Python 3.x
- Libraries: `geopandas`, `os`

#### Usage:
1. Place the input shapefiles (`fatal_overdose_updated.shp`, `nonfatal.shp`, `hexbin.shp`) in the specified folder.
2. Update the `folder` variable in the script to point to the correct directory.
3. Run the script to generate the updated hexbin grid with counts.

#### Example Output:
The output shapefile (`hexbin_with_counts.shp`) will include:
- `grid_id`: Unique identifier for each hexbin cell.
- `fatal_counts`: Number of fatal overdose cases in the cell.
- `nonfatal_counts`: Number of non-fatal overdose cases in the cell.
- `geometry`: Spatial geometry of the hexbin cell.

This project is ideal for spatial analysis of overdose cases and can be extended for further visualization or statistical analysis.
