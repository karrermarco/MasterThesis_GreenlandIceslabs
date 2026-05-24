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
