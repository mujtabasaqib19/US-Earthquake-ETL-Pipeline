# USGS Earthquake ETL Project

This repository hosts an end‑to‑end data engineering pipeline that pulls earthquake records from
the USGS FDSN event API and prepares them for analysis. All code is in Python and can be run
locally or containerized. The output feeds into interactive dashboards (Power BI / Tableau)
—see the **Dashboard** section below for a preview.

The notebook `api-data.ipynb` documents the entire workflow and includes sample queries and
transformations, making it easy to reproduce or extend the pipeline.

## Overview

- **Source API**: `https://earthquake.usgs.gov/fdsnws/event/1/query`
- **Language**: Python 3
- **Dependencies**: `pandas`, `requests`

## Pipeline Components

1. **Extract** – `src/etl_pipeline.py` contains `fetch_data` which calls the USGS API and
   normalizes the JSON response into a pandas DataFrame.
2. **Transform** – Basic cleaning in `transform_data`, e.g. removing entries without magnitude and
   adding a `date` column.
3. **Load** – `load_data` writes the DataFrame to a CSV file (can be extended to write parquet,
   insert into a database, etc.).

## Usage

```sh
python -m pip install -r requirements.txt

# run pipeline for the last 30 days with mag >= 4
python src/etl_pipeline.py --start 2026-01-01 --end 2026-02-28 --minmag 4 --out data.csv
```

The produced `data.csv` (or a database table if you configure Postgres) serves as the
backing store for the interactive dashboard.

The produced `data.csv` can be opened directly in Tableau/Power BI. Set up an extract or live connection
using the CSV or load it into a data warehouse if desired.

## Notebook

`api-data.ipynb` shows an example of using the ETL functions interactively and plotting.

## Dashboard

Once the pipeline has produced or loaded data, you can create an interactive dashboard.
Below is a placeholder for the Power BI dashboard; replace with your embedded report when available:

<iframe title="USGS Earthquake Dashboard" width="800" height="600" src="YOUR_DASHBOARD_URL_HERE" frameborder="0" allowFullScreen="true"></iframe>

The notebook also includes steps for exporting a `.pbix` file (see `us earthquake dashboard.pbix`).


## Next Steps / Extensions

- Schedule the script to run daily using Airflow/Prefect/cron.
- Load data into a relational database or cloud storage (S3, BigQuery, etc.).
- Add more elaborate transformations (e.g. geospatial bucketing, magnitude categorization).
- Automate visualization by writing scripts that generate Tableau datasources or Power BI reports.

---

*March 2026*