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

### Step 1: Spatial Grid Association & Autocorrelation (Mean Approach)
* **File:** `Moran_Approach1_MT_Karrer.ipynb`
* **Methodology (Regional Aggregation):** Implements the **Mean Approach** for data merging. For each ice slab measurement point $i$, a 1 km radius buffer is delineated. The script calculates the mean probability of surface meltwater occurrence and the mean ice slab thickness for all data points within this area (requiring a minimum threshold of $\ge 50$ points per buffer).
* **Statistical Output:** Computes parametric and non-parametric correlations (Pearson/Spearman) and spatial dependencies on a regionally smoothed scale. Adds the aggregated columns `hydro_1km_mean` and `ice_1km_mean` to the dataset.

### Step 2: Localized Spatial Autocorrelation (Exact Approach)
* **File:** `Moran_Approach2_MT_Karrer.ipynb`
* **Methodology (Point-Based Context):** Implements the **Exact Approach** for data merging. It identifies the exact hydrology pixel for each individual ice slab measurement point $i$. To account for glaciological ice motion, the search window is expanded to a $7 \times 7$ pixel neighborhood (210 m x 210 m), extracting the maximum hydrology value from these 49 pixels into a new column called `hydrology_value_buffer`.
* **Statistical Output:** Models localized spatial dependencies using a **Local Bivariate Moran's I** framework (`Moran_Local_BV`). Spatial weight structures are constructed via a K-Nearest Neighbors ($k$-NN) matrix using an `sklearn.neighbors.BallTree` lookup. Significance is validated via **999 random spatial permutations** to classify grid cells into spatial clusters (High-High Hotspots, Low-Low Coldspots). Results are exported as vectorized GeoPackages (`.gpkg`).
### Step 3: Topographic and Ice Flow Direction Alignment Analysis
* **File:** `Flow_Alignment_Analysis_MT_Karrer-Copy1.ipynb`
* **Purpose:** Investigates whether the spatial hotspot clusters exhibit distinct ice flow physics.
  * Calculates the absolute angular difference (in degrees) between ice flow velocity vectors and the maximum topographic slope direction derived from the ArcticDEM.
  * Employs non-parametric distribution tests—specifically the **Mann-Whitney U Test** and the **Kolmogorov-Smirnov Test**—to verify if the alignment significantly shifts inside the High-High hotspots compared to the background ice sheet sectors.

### Step 4: Generalized Additive Modeling (GAM)
* **File:** `GAM_Analysis_MT_Karrer.ipynb`
* **Purpose:** Resolves the multi-variate, non-linear relationships driving ice slab thickness variations.
  * Configures and trains a `LinearGAM` using a **Gaussian error distribution family** and an **identity link function**.
  * Employs penalized B-spline smoothers to isolate the shape and magnitude of non-linear partial effects.
  * Restricts training to the localized High-High spatial clusters to measure how much variance is explained by surface meltwater versus additional glaciological covariates (Elevation, Ice Velocity, Strain Rates, Flow Alignment), generating partial dependence plots with 95% confidence intervals.
