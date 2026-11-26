# Sea-Level Rise Forces Global Landward Expansion of Mangroves

**Author:** R. Walcker — Toulouse University, Center of Research in Biodiversity and Environment (CRBE)  
**Co-author:** F. Fromard — CNRS, Center of Research in Biodiversity and Environment (CRBE)  
**Code Repository:** `https://github.com/WalckR/Mangroves_and_sea-level_rise`  
**Intermediate Datasets (42 GB, Zenodo DOI):** https://doi.org/10.5281/zenodo.24526648  
**Peer-Review Private Access Link:** https://zenodo.org/records/24526649?token=eyJhbGciOiJIUzUxMiJ9.eyJpZCI6Ijc0YWFhYTVlLWU0ZGMtNGIzMy1hMTcxLTg2MGM5MjQ3ZTgyMCIsImRhdGEiOnt9LCJyYW5kb20iOiJlYjNhZTZiOGI2OGZmMjc1Yjg4ZTlhODY4NDliYzk5YyJ9.IS5hVJc6cLPCtg9QZSkDjQZ-JIph2LgA3mPkrKHnSNxafeSmuoMiLGHqhpzb4jHNUZ1pOWkNEqEHhyCb-9wSng

---

## 1. Overview

This repository contains the Jupyter notebooks and documentation required to reproduce the spatial preprocessing, statistical analyses, and figures presented in the manuscript **"Sea-level rise forces global landward expansion of mangroves"**.

Because the precomputed intermediate datasets total **~42 GB**, they are archived externally on Zenodo rather than stored in GitHub. Reviewers can either:
- **Option A (Recommended):** Download the 42 GB precomputed intermediate datasets from Zenodo into `<working_dir>/Created_Data/` and run the figure notebooks (`STEP06_Figure1.ipynb` to `STEP11_Figure4.ipynb`) directly.
- **Option B (Full Pipeline):** Format raw inputs (`STEP01_Format_GMWv4.ipynb`, `STEP02_Format_SSH_data.ipynb`), download/format JRC Global Surface Water yearly history from Google Earth Engine (`STEP03a_Download_and_Format_JRC_YeralyHistory.ipynb`), rebuild intermediate datasets (`STEP03b` through `STEP05`), and generate the manuscript figures (`STEP06` through `STEP11`).

---

## 2. Repository & Local Workspace Structure

Create a local working directory (e.g., `<working_dir>/`) containing this repository and a `Created_Data/` subfolder where the Zenodo archives are extracted:

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
```

> **Path Configuration:** Before running any notebook, update the root working directory path variable at the top of each notebook so that it points to your local `<working_dir>/Created_Data/` folder (and to your downloaded GMW v4.1.12 and ORAS5 inputs).

---

## 3. Preprocessing, Intermediate Datasets & Manuscript Figures

### Raw Input Formatting & GEE Download (`STEP01` – `STEP03a`)

1. **GMW v4.1.12 Formatting (`STEP01_Format_GMWv4.ipynb`):**
   - **Input:** Raw Global Mangrove Watch (GMW v4.1.12) raster stack downloaded from the source cited in the manuscript.
   - **Output:** Semicolon-delimited (`;`) CSV summary table with columns:
     `tile_id;year;center_lat;center_lon;pixel_count;area_km2;file_name`

2. **ORAS5 Sea Surface Height Formatting (`STEP02_Format_SSH_data.ipynb`):**
   - **Input:** Raw ORAS5 sea surface height (`SSH`, in meters) data downloaded from the Copernicus / ECMWF source cited in the manuscript.
   - **Output:** Semicolon-delimited (`;`) CSV table of annual mean `SSH` (`m`) aggregated onto the same tiles as the GMW v4.1.12 dataset, with columns:
     `tile_id;year;center_lat;center_lon;SSH_m`

3. **JRC Yearly Water History Download & Formatting (`STEP03a_Download_and_Format_JRC_YeralyHistory.ipynb`):**
   - **Source & Requirement:** Downloads and formats multi-year permanent water cover rasters from **Google Earth Engine (GEE)** based on the JRC Global Surface Water dataset (Pekel et al., 2016). **Requires a Google Earth Engine account** and can take a considerable amount of time to run.

### Tabular & Spatial Reference Files in Created_Data/

In addition to the six raster/CSV archives, ensure the following files are placed inside <working_dir>/Created_Data/ (including all sidecar files—.shp, .shx, .dbf, .prj, .cpg—for each shapefile):

| File | Location | Description & Encoding |
| :--- | :--- | :--- |
| **`ecmwf_msl_annual_1deg_tiles.csv/`** | `Created_Data/` | Semicolon-delimited (;) annual mean sea-level / SSH summary table per 1° x 1° tile. |
| **`gmwv4_surfaces_annual_1deg_tiles.csv`** | `Created_Data/` | Semicolon-delimited (;) annual mangrove surface area summary table per 1° x 1° tile. |
| **`GMW 1° x 1° Tiles Shapefile (mask_artif)`** | `Created_Data/` | Shapefile of the 1,696 global 1° x 1° tiles including the mask_artif attribute (0 = pristine, 1,261 tiles covering ~14.94 million km²; 1 = non-pristine, 435 tiles). |
| **`world-administrative-boundaries.shp`** | `Created_Data/` | Global country and territory administrative boundaries shapefile (from Opendatasoft / WFP & Natural Earth compilation, not a direct OpenStreetMap extract) used for regional grouping and mapping. |

### Intermediate Datasets on Zenodo (`Created_Data/`, ~42 GB)

All six archives on Zenodo are organized by `1° x 1°` `"pristine"` geographic tiles and must be extracted into `<working_dir>/Created_Data/`:

| Folder | Generated By | Format & CRS | Description & Encoding |
| :--- | :--- | :--- | :--- |
| **`GSW_1984_baseline/`** | `STEP03b_Create_1984_Baseline_Permanent_Water_Raster.ipynb` | GeoTIFF rasters (`EPSG:54034`) | One raster per `1° x 1°` `"pristine"` tile. `1` = permanent water in 1984; `0` = otherwise. |
| **`GSW_2021_baseline/`** | `STEP03c_Create_2021_Baseline_Permanent_Water_Raster.ipynb` | GeoTIFF rasters (`EPSG:54034`) | One raster per `1° x 1°` `"pristine"` tile. `1` = permanent water in 2021; `0` = otherwise. |
| **`Gain_Age_2025_Rasters/`** | `STEP04a_Create_gain_age_2025_rasters.ipynb` | GeoTIFF rasters (`EPSG:54034`) | Mangrove pixels in 2025. Pixel value = mangrove establishment/gain age (`2025` reference). |
| **`Loss_Age_2025_Rasters/`** | `STEP04b_Create_loss_age_2025_rasters.ipynb` | GeoTIFF rasters (`EPSG:54034`) | Mangrove pixels in 2015. Pixel value = **number of years since the most recent mangrove loss** (`2025` reference). |
| **`CSV_Gain_Loss_Age/`** | `STEP04c_Create_Gain_Loss_Age_CSVs.ipynb` | Semicolon-delimited (`;`) CSVs (`WGS84`) | Columns: `tile_id`, `latitude`, `longitude`, `age`, `year`, `position` (`"land"` or `"sea"`), `class` (`"gain"` or `"loss"`). |
| **`Distance_CSVs/`** | `STEP05_Create_Distance_CSVs.ipynb` | Semicolon-delimited (`;`) CSVs (`WGS84`) | Columns: `tile_id`, `year`, `longitude`, `latitude`, `dist_to_land` (m), `dist_to_sea` (m) relative to `GSW_1984_baseline/`. |

> **Important CSV Note:** All CSV files use a **semicolon (`;`)** column delimiter (e.g., `pd.read_csv(..., sep=";")` in Python or `read.csv2(...)` in R).

### Manuscript Figures (`STEP06_Figure1.ipynb` – `STEP11_Figure4.ipynb`)

Notebooks `STEP06_Figure1.ipynb` through `STEP11_Figure4.ipynb` generate the figures for the manuscript.

---

## 4. Software Environment

- **Language:** Python (`[ex. 3.11]`) — Jupyter Notebook / JupyterLab
- **External Account Required (Full Pipeline):** Google Earth Engine (GEE) account for `STEP03a_Download_and_Format_JRC_YeralyHistory.ipynb`.
- **Hardware Requirements:** At least **42 GB** of free disk space for `Created_Data/` and `[ex. 16 GB]` RAM.

Create and activate the environment:
```bash
# Using Conda
conda env create -f environment.yml
conda activate mangroves-slr

# Or using pip
# pip install -r requirements.txt
```

---

## 5. Reproduction Workflow

### Option A — Starting from the Precomputed Zenodo Datasets (Recommended)
1. Download the six `.zip` archives and `DATA_README.txt` from Zenodo (`https://doi.org/10.5281/zenodo.24526648` or via the peer-review private access link).
2. Extract all six `.zip` files into `<working_dir>/Created_Data/`.
3. Open and execute the figure notebooks in sequence from `STEP06_Figure1.ipynb` to `STEP11_Figure4.ipynb`.

### Option B — Rebuilding Intermediate Datasets from Raw Sources
1. Download the raw public datasets listed in Section 6 (GMW v4.1.12 raster stack and ORAS5 SSH data) and ensure your **Google Earth Engine (GEE)** account is configured.
2. Run the formatting, download, and preprocessing notebooks in numerical order (note that `STEP03a` can be slow):
   1. `STEP01_Format_GMWv4.ipynb`
   2. `STEP02_Format_SSH_data.ipynb`
   3. `STEP03a_Download_and_Format_JRC_YeralyHistory.ipynb` *(requires GEE account; long runtime)*
   4. `STEP03b_Create_1984_Baseline_Permanent_Water_Raster.ipynb`
   5. `STEP03c_Create_2021_Baseline_Permanent_Water_Raster.ipynb`
   6. `STEP04a_Create_gain_age_2025_rasters.ipynb`
   7. `STEP04b_Create_loss_age_2025_rasters.ipynb`
   8. `STEP04c_Create_Gain_Loss_Age_CSVs.ipynb`
   9. `STEP05_Create_Distance_CSVs.ipynb`
3. Run `STEP06_Figure1.ipynb` through `STEP11_Figure4.ipynb` to generate the figures for the manuscript.

---

## 6. Data Sources, Attribution & Licenses

### Raw Public Data Sources
The intermediate datasets archived on Zenodo were derived from three publicly available sources:

1. **Global Mangrove Watch (v4.1.12):** Bunting, P. et al. *Global Mangrove Watch: Timeseries of Mangrove Extent (Version 4.1.12)*. Zenodo, https://doi.org/10.5281/zenodo.21346457 (2026). Licensed under **CC-BY 4.0**.
2. **ORAS5 Global Ocean Reanalysis:** Copernicus Climate Change Service, Climate Data Store (2021). *ORAS5 global ocean reanalysis monthly data from 1958 to present*. https://doi.org/10.24381/cds.67e8eeb7 (Accessed on 2026-09-03). Contains modified Copernicus Climate Change Service information 2026.
3. **JRC Global Surface Water:** Pekel, J.-F., Cottam, A., Gorelick, N., & Belward, A. S. *High-resolution mapping of global surface water and its long-term changes*. Nature 540, 418–422 (2016). Data provided by the European Commission Joint Research Centre (JRC), licensed under **CC-BY 4.0**.

### Licenses
- **Code (this repository):** Licensed under the **MIT License** (see `LICENSE`).
- **Intermediate datasets (Zenodo):** Licensed under **Creative Commons Attribution 4.0 International (CC-BY 4.0)**.

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
```
**Path Configuration**: Before running any notebook, update the root working directory path variable at the top of each notebook so that it points to your local <working_dir>/Created_Data/ folder (and to your downloaded GMW v4.1.12 and ORAS5 inputs).

## 3. Preprocessing, Intermediate Datasets & Manuscript Figures

Raw Input Formatting & GEE Download (STEP01 – STEP03a)

    1- GMW v4.1.12 Formatting (STEP01_Format_GMWv4.ipynb):

        Input: Raw Global Mangrove Watch (GMW v4.1.12) raster stack and 1° x 1° tile grid (with mask_artif field) downloaded from the source cited in the manuscript.

        Output (Data_Created/): Semicolon-delimited (;) CSV summary table (gmwv4_surfaces_annual_1deg_tiles.csv), with columns:tile_id;year;center_lat;center_lon;pixel_count;area_km2;file_name

    ORAS5 Sea Surface Height Formatting (STEP02_Format_SSH_data.ipynb):

        Input: Raw ORAS5 sea surface height (SSH, in meters) data downloaded from the Copernicus / ECMWF source cited in the manuscript.

        Output (Data_Created/): Semicolon-delimited (;) CSV table (ecmwf_msl_annual_1deg_tiles.csv) of annual mean SSH (m) aggregated onto the 1° x 1° tiles, with columns:tile_id;year;center_lat;center_lon;SSH_m

    JRC Yearly Water History Download & Formatting (STEP03a_Download_and_Format_JRC_YeralyHistory.ipynb):

        Source & Requirement: Downloads and formats multi-year permanent water cover rasters from Google Earth Engine (GEE) based on the JRC Global Surface Water dataset (Pekel et al., 2016). Requires a Google Earth Engine account and can take a considerable amount of time to run.
