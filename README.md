# USGS Earthquake ETL Project

This repository provides a simple data engineering project that extracts earthquake data from the
USGS FDSN event web service, performs basic transformations, and writes the result to disk.
The final dataset can then be consumed by visualization tools such as Tableau or Power BI.

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

The produced `data.csv` can be opened directly in Tableau/Power BI. Set up an extract or live connection
using the CSV or load it into a data warehouse if desired.

## Notebook

`api-data.ipynb` shows an example of using the ETL functions interactively and plotting.

## Next Steps / Extensions

- Schedule the script to run daily using Airflow/Prefect/cron.
- Load data into a relational database or cloud storage (S3, BigQuery, etc.).
- Add more elaborate transformations (e.g. geospatial bucketing, magnitude categorization).
- Automate visualization by writing scripts that generate Tableau datasources or Power BI reports.

---

*March 2026*