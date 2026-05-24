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

# Statistical Analysis of Ice Slabs, Surface Hydrology, and Ice Flow Alignment on the Greenland Ice Sheet

## Introduction & Scientific Context
Global climate warming is especially noticeable in Arctic regions. Since the 1990s, meltwater runoff from the Greenland Ice Sheet (GrIS) has drastically increased, contributing significantly to global sea-level rise. Concurrently, a rapid propagation of low-permeability **ice slabs** has developed in the shallow firn. This occurs when excess meltwater percolates into the firn and refreezes within the pore space, creating solid ice layers that restrict further vertical percolation. This causes a critical regime shift: instead of being retained in the firn, additional meltwater remains near the surface—forming supraglacial lakes, rivers, and slush fields—and runs off directly into the ocean.

While a link between surface meltwater occurrence and ice slab thickness has been proposed, it had not yet been statistically validated. This study conducts a comprehensive statistical analysis across five regional sectors of the Greenland Ice Sheet (**SW, CW, NW, NO, NE**) to evaluate these dynamics. 

### Core Findings of this Pipeline:
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
|   |-- Greenland/ArcticDEM/deg_difference/
|   |   `-- deg_difference100_python_Greenland.tif  [External Asset - Download via PGC required]
|   `-- Ice Slab (Jullien)/Ice_Thickness_Greenland_Firn/
|       `-- [Original Jullien CSV/Pickle Files]     [External Asset - Download via Zenodo required]
|
`-- Outputs/                                        [Empty directories - generated automatically]
    |-- Moran_Approach1/
    |-- Moran_Approach2/
    `-- Plots Deg Difference/
