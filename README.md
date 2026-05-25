# Investigating the Link between Greenland Firn Structure and the Hydrological Surface Network

The analysis explores the complex interactions between ice slabs and surface meltwater accumulation, as well as the driving forces behind ice slab evolution, utilizing localized spatial corelations and non-linear regression frameworks (GAMs) across five regional regions of the Greenland Ice Sheet (SW, CW, NW, NO, NE).

This repository contains the complete computational workflow, data preprocessing pipelines, and spatial/non-spatial statistical models developed for the Master's Thesis: 
"Investigating the Link between Greenland Firn Structure and the Hydrological Surface Network".
 

### Core Findings of Master Thesis:
* **Spatial Clustering (Bivariate Moran's I):** The findings indicate a high degree of spatial correlation between the probability of surface meltwater occurrence and ice slab thickness in three out of five regions, and a moderate correlation in the remaining two, reflecting a systematic physical coupling.
* **Multivariate Drivers (GAM Analysis):** Including a Generalized Additive Model (GAM) framework revealed that surface meltwater explains a maximum of 33% of the variance in ice slab thickness. Additional glaciological drivers—such as elevation, ice flow velocity, and strain rates—exert a substantial influence, highlighting that the underlying spatial relationships are highly multivariate and vary regionally across the Ice Sheet.

---

## 1. Project Structure & Directory Mapping

The scripts rely on a standardized directory hierarchy to process the spatial datasets. 

* **Important Note on Replicability:** Path adaptations are mandatory before running any code. You will need to adjust the absolute base paths (e.g., `C:/Masterarbeit/Daten Masterarbeit/...`) in the setup cells of each Jupyter Notebook to match your local file system.

The tree below illustrates the required logical architecture.

```text
[Your_Project_Base_Directory]/
|-- Flow_Alignment_Analysis_MT_Karrer-Copy1.ipynb   [Included]
|-- GAM_Analysis_MT_Karrer.ipynb                    [Included]
|-- Moran_Approach1_MT_Karrer.ipynb                 [Included]
|-- Moran_Approach2_MT_Karrer.ipynb                 [Included]
|
|-- Datasets/                                       [Directory structure only - data excluded]
    |-- Ice Slab (Jullien)/IceSlabs_20022018/IceSlabs_20022018/20102018/Ice_Thickness_Greenland_Firn/
|       |-- Clipped CW/
|       |-- Clipped NE/
|       |-- Clipped NO/
|       |-- Clipped NW/
|       |-- Clipped SW/
|           |-- Firnice_Thickness_SW_15km.shp       [External Asset - Jullien et al. (2023)]
    |-- Greenland/
|       |-- melting map/tedstone/master_hydrology_map_tedstone2022/
|           |-- master_map_GrIS_mean.vrt                [External Asset - Tedstone et al. (2022)]
|
|-- Outputs/                                        [Empty directories - generated automatically]
    |-- Moran_Approach1/
    |-- Moran_Approach2/

```

## 2. Computational Workflow & Execution Order

To fully reproduce the findings, statistical metrics, and regional figures of the thesis, the Jupyter Notebooks must be executed in the following chronological sequence. The pipeline moves from exploratory data synchronization to spatial autocorrelation modeling, geographic alignment validation, and final multivariate non-linear regression.

### Step 1: Localized Spatial Autocorrelation (Exact Approach)

* **File:** `Moran_Approach1_MT_Karrer.ipynb`
* **Methodology (Point-Based Context):** Implements the **Exact Approach** for data merging. It identifies the exact hydrology pixel for each individual ice slab measurement point $i$. To account for glaciological ice motion, the search window is expanded to a $7 \times 7$ pixel neighborhood (210 m x 210 m), extracting the maximum hydrology value from these 49 pixels into a new column called `hydrology_value_buffer`.
* **Statistical Output:** Models localized spatial dependencies using a Global and Local Bivariate Moran's I framework (`Moran_Global_BV` / `Moran_Local_BV`). Spatial weight structures are constructed using the `libpysal` library to build spatial weights based on localized coordinate configurations. Significance is validated via 999 random spatial permutations to classify grid cells into spatial clusters (High-High, High-Low, Low-High and Low-Low). Results are exported as vectorized GeoPackages (`.gpkg`).

### Step 2: Spatial Grid Association & Autocorrelation (Mean Approach)
* **File:** `Moran_Approach2_MT_Karrer.ipynb`
* **Methodology (Regional Aggregation):** Implements the **Mean Approach** for data merging. For each ice slab measurement point $i$, a 1 km radius buffer is delineated. The script calculates the mean probability of surface meltwater occurrence and the mean ice slab thickness for all data points within this area (requiring a minimum threshold of $\ge 50$ points per buffer).
* **Statistical Output:** Models localized spatial dependencies using a Global and Local Bivariate Moran's I framework (`Moran_Global_BV` / `Moran_Local_BV`). Spatial weight structures are constructed using the `libpysal` library to build spatial weights based on localized coordinate configurations. Significance is validated via 999 random spatial permutations to classify grid cells into spatial clusters (High-High, High-Low, Low-High and Low-Low). Results are exported as vectorized GeoPackages (`.gpkg`).

### Step 3: Topographic and Ice Flow Direction Alignment Analysis
* **File:** `Flow_Alignment_Analysis_MT_Karrer-Copy1.ipynb`
* **Purpose:** Investigates whether the spatial clusters exhibit distinct ice flow physics.
  * Calculates the absolute angular difference (in degrees) between ice flow direction and the topographic slope direction derived from the ArcticDEM.
* **Statistical Output:** Executes statistics to verify how the angular alignment varies across all four significant spatial categories: `High-High`, `Low-Low`, `High-Low`, and `Low-High`. It implements the `Mann-Whitney U Test` (to isolate shifts in medians) and the `Kolmogorov-Smirnov Test` (to evaluate variations in the cumulative distribution shapes) between the cluster types. Outputs are exported as comprehensive multi-cluster boxplots.

### Step 4: Generalized Additive Modeling (GAM)
* **File:** `GAM_Analysis_MT_Karrer.ipynb`
* **Purpose:** Implements and trains a `LinearGAM` framework using a Gaussian error distribution family.  Systematically tests individual relationships against ice slab thickness for five core variables: Surface Water Probability, Elevation, Flow Alignment, Ice Velocity, and Strain Rate.
 * **Statistical Output:** Extracts the pseudo-$R^2$ metric to determine the proportion of variance explained in ice slab thickness by the covariates. Generates 2x2 grids of Partial Dependence Plots mapping the non-linear regression curves with 95% shaded confidence intervals.


---

## 3. Installation & Dependency Requirements

The spatial processing and statistical operations within this pipeline were compiled and evaluated inside a Python 3.11 environment utilizing the Jupyter Lab interface. 

To initialize the compute environment and install all necessary geospatial, machine learning, and regression libraries, execute the following commands in your terminal:

```bash
pip install numpy pandas scipy matplotlib seaborn rasterio geopandas libpysal esda pygam
```
---

## 4. Primary Data Sources & References

To replicate the analysis, the source datasets must be acquired from the following official repositories:

1. **Ice Slab Thickness:** The ice slab thickness measurements aquired across the five regional sectors (SW, CW, NW, NO, NE) can be downloaded on the data repository of Jullien et al. (2023) (https://doi.org/10.5281/zenodo.7505426).
  
2. **Surface Hydrology Dataset (Tedstone 2022):** The surface hydrology map utilized for the thesis is built upon the multi-year mean surface meltwater occurrence. The data was made available by Tedstone and Machguth (2022) upon request (https://doi.org/10.1038/s41558-022-01371-z).

3. **ArcticDEM:** The ArcticDEM was derived to calculate the slope, which is the basis for the angular difference between ice flow and water flow direction. The ArcticDEM dataset is provided by the Polar Geospatial Center (PGC)
(https://www.pgc.umn.edu/data/arcticdem/).
 
4. **Ice Flow Velocity and Strain Rate:** The surface velocity magnitude was derived from the ITS_LIVE projct by NASA. The strian rate was calculated on based on the ice velocity data (https://its-live.jpl.nasa.gov/#datasets).


### Core Literature References:
* **Jullien, N., Tedstone, A. J., Machguth, H., Karlsson, N. B., and Helm, V. (2023).** Greenland
ice sheet ice slab expansion and thickening. Geophysical Research Letters, 50(10).
* **Tedstone, A. J. and Machguth, H. (2022).** Increasing surface runoff from greenland’s firn
areas. Nature Climate Change, 12(7):672–676.

---

## 5. License & Academic Citation

This repository hosts code developed exclusively for academic research purposes as part of a Master's Thesis. 

---

## 5. License & Contact

This repository hosts the computational workflow and data processing scripts developed as part of a Master's Thesis. All code is available for academic and research purposes.

* **Author:** M. Karrer (2026)
* **Master Thesis:** Investigating the Link between Greenland Firn Structure and the Hydrological Surface Network.
