# Sea-Level Rise Forces Global Landward Expansion of Mangroves

**Author:** R. Walcker — Research scientist — Toulouse University, Center of Research in Biodiversity and Environment (CRBE)  
**Co-author:** F. Fromard — Researcher emeritus — CNRS, Center of Research in Biodiversity and Environment (CRBE)  
**Code Repository:** `https://github.com/WalckR/Mangroves_and_sea-level_rise`  
**Intermediate Datasets (42 GB, Zenodo DOI):** https://doi.org/10.5281/zenodo.24526648  
**Peer-Review Private Access Link:** https://zenodo.org/records/24526649?token=eyJhbGciOiJIUzUxMiJ9.eyJpZCI6Ijc0YWFhYTVlLWU0ZGMtNGIzMy1hMTcxLTg2MGM5MjQ3ZTgyMCIsImRhdGEiOnt9LCJyYW5kb20iOiJlYjNhZTZiOGI2OGZmMjc1Yjg4ZTlhODY4NDliYzk5YyJ9.IS5hVJc6cLPCtg9QZSkDjQZ-JIph2LgA3mPkrKHnSNxafeSmuoMiLGHqhpzb4jHNUZ1pOWkNEqEHhyCb-9wSng

---

## 1. Overview

This repository contains the Jupyter notebooks and documentation required to reproduce the spatial preprocessing, statistical analyses, and figures presented in the manuscript **"Sea-level rise forces global landward expansion of mangroves"**.

Because the precomputed intermediate datasets total **~42 GB**, they are archived externally on Zenodo rather than stored in GitHub. Reviewers can either:
- **Option A (Recommended):** Download the precomputed intermediate datasets (including the ~42 GB Zenodo archives, summary CSVs, and spatial boundary/tile shapefiles) into `<working_dir>/Created_Data/` (or your local data directory) and run the figure notebooks (`STEP06_Figure1.ipynb` to `STEP11_Figure4.ipynb`) directly.
- **Option B (Full Pipeline):** Format raw inputs (`STEP01_Format_GMWv4.ipynb`, `STEP02_Format_SSH_data.ipynb`), download/format JRC Global Surface Water yearly history from Google Earth Engine (`STEP03a_Download_and_Format_JRC_YeralyHistory.ipynb`), rebuild intermediate datasets (`STEP03b` through `STEP05`), and generate the manuscript figures (`STEP06` through `STEP11`).

---

## 2. Repository & Local Workspace Structure

Create a local working directory (e.g., `<working_dir>/`) containing this repository and a `Created_Data/` subfolder where the Zenodo archives and tabular/spatial intermediate files are stored:

```text
<working_dir>/
├── Mangroves_and_sea-level_rise/               # This GitHub repository
│   ├── README.md
│   ├── LICENSE                                 # MIT License
│   ├── environment.yml                         # Python dependencies (or requirements.txt)
│   ├── STEP01_Format_GMWv4.ipynb
│   ├── STEP02_Format_SSH_data.ipynb
│   ├── STEP03a_Download_and_Format_JRC_YeralyHistory.ipynb
│   ├── STEP03b_Create_1984_Baseline_Permanent_Water_Raster.ipynb
│   ├── STEP03c_Create_2021_Baseline_Permanent_Water_Raster.ipynb
│   ├── STEP04a_Create_gain_age_2025_rasters.ipynb
│   ├── STEP04b_Create_loss_age_2025_rasters.ipynb
│   ├── STEP04c_Create_Gain_Loss_Age_CSVs.ipynb
│   ├── STEP05_Create_Distance_CSVs.ipynb
│   ├── STEP06_Figure1.ipynb
│   ├── ...
│   └── STEP11_Figure4.ipynb
└── Created_Data/                               # Target folder for Zenodo datasets (~42 GB)
    ├── ecmwf_msl_annual_1deg_tiles.csv         # Annual mean sea level per 1° x 1° tile
    ├── gmwv4_surfaces_annual_1deg_tiles.csv    # Annual mangrove surface area per 1° x 1° tile
    ├── GMW_tiles_mask_artif/                   # GMW 1° x 1° tiles shapefile (.shp + sidecar files, mask_artif field)
    ├── World_Administrative_Boundaries/          # world-administrative-boundaries.shp (+ sidecar files)
    ├── GSW_1984_baseline/
    ├── GSW_2021_baseline/
    ├── Gain_Age_2025_Rasters/
    ├── Loss_Age_2025_Rasters/
    ├── CSV_Gain_Loss_Age/
    └── Distance_CSVs/
