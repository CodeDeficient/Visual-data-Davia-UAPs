# Data Schema and Preprocessing Document

## 1. Introduction

**Project Title:** Interactive UFO/UAP Sightings Explorer & AI Analyst

This document details the data schema for the UFO/UAP sightings data used in the application and the preprocessing steps applied to the original dataset. Unlike traditional applications with a relational database, this project utilizes a pre-processed flat file (Apache Parquet format) bundled with the backend application. This approach is chosen for simplicity, cost-effectiveness (no database hosting fees), and efficient read performance with Pandas in a serverless environment.

## 2. Data Source

*   **Name:** National UFO Reporting Center (NUFORC) UFO Sightings Dataset.
*   **Original Source:** Typically available on platforms like Kaggle.
    *   Example Kaggle Link (conceptual): `https://www.kaggle.com/datasets/NUFORC/ufo-sightings` (The actual link used will be the one identified during the data acquisition phase).
*   **Original Format:** CSV (Comma Separated Values).

## 3. Data Preprocessing Steps (Offline Task)

The following steps will be performed once, offline, to prepare the `nuforc_data.parquet` file that will be used by the application:

1.  **Load Original CSV:** Read the raw CSV dataset into a Pandas DataFrame.
2.  **Initial Cleaning & Inspection:**
    *   Review column names and data types.
    *   Handle inconsistencies in column naming (e.g., whitespace, special characters).
    *   Identify and assess missing values (NaNs) for each column.
3.  **Column Selection/Renaming:**
    *   Select relevant columns for the application. Based on the `initial_concept.md` and typical UFO data, these are likely to include:
        *   `datetime` (or `date posted`, `date_time`) -> Rename to `date_time`
        *   `city` -> Rename to `city`
        *   `state` -> Rename to `state`
        *   `country` -> Rename to `country`
        *   `shape` (or `ufo_shape`) -> Rename to `shape`
        *   `duration (seconds)` -> Rename to `duration_seconds`
        *   `duration (hours/min)` -> (Parse and convert to `duration_seconds` if primary seconds field is unreliable or missing)
        *   `comments` -> Rename to `comments`
        *   `latitude` -> Rename to `latitude`
        *   `longitude` (or `long`) -> Rename to `longitude`
    *   Drop columns not required for the application to reduce file size and complexity.
4.  **Data Type Conversion & Standardization:**
    *   **`date_time`**: Convert to Pandas datetime objects. Handle potential parsing errors. Standardize to UTC if timezone information is inconsistent or missing, or document the assumed timezone.
    *   **`city`, `state`, `country`, `shape`, `comments`**: Ensure these are string types. Clean extraneous whitespace. Convert text to a consistent case (e.g., lowercase or title case) for `city`, `state`, `country`, `shape` to aid filtering and grouping, while preserving original case for `comments`.
    *   **`duration_seconds`**: Convert to numeric (float or integer). Handle non-numeric entries (e.g., text descriptions of duration) by attempting to parse them or setting them to NaN if unparseable. Ensure consistency if multiple duration columns exist.
    *   **`latitude`, `longitude`**: Convert to numeric (float). Validate ranges (Latitude: -90 to 90, Longitude: -180 to 180).
5.  **Handling Missing Values (NaNs):**
    *   **`latitude`, `longitude`**: Sightings with missing latitude/longitude may be dropped if map visualization is critical and imputation is not feasible or desired. Alternatively, they can be kept but excluded from map views.
    *   **`date_time`, `shape`**: Sightings with missing values in these core filterable fields might be dropped or assigned a placeholder like "unknown" if appropriate for the analysis.
    *   **`state`, `country`**: Similar to `shape`. If `country` is missing but `state` implies a country (e.g., US states), `country` could be imputed.
    *   **`comments`**: Missing comments are acceptable.
    *   **`duration_seconds`**: Missing durations are acceptable; they can be treated as 'unknown' or excluded from duration-specific analyses.
6.  **Outlier Handling (Optional, based on initial exploration):**
    *   For `duration_seconds`, extreme outliers might be capped or investigated.
7.  **Filtering Rows (Optional):**
    *   Remove rows with insufficient data for core functionality (e.g., if `date_time` AND `latitude`/`longitude` are both missing).
8.  **Final Review:** Inspect the cleaned DataFrame for consistency and correctness.
9.  **Save to Parquet:** Save the processed Pandas DataFrame to `nuforc_data.parquet` using `df.to_parquet()`. Use a suitable compression Codec (e.g., 'snappy' or 'gzip') if needed to balance file size and read speed.

## 4. Schema of `nuforc_data.parquet`

The Parquet file will contain a single table-like structure. Each row represents a UFO/UAP sighting.

| Column Name        | Data Type (Pandas/Parquet) | Description                                                                 | Example Value             | Notes                                                                 |
|--------------------|----------------------------|-----------------------------------------------------------------------------|---------------------------|-----------------------------------------------------------------------|
| `date_time`        | `datetime64[ns]`           | Date and time of the sighting (UTC or documented timezone)                  | `2023-10-26T14:30:00`     | Crucial for time-based filtering and analysis.                        |
| `city`             | `string` / `object`        | City where the sighting occurred.                                           | `Roswell`                 | Standardized case (e.g., Title Case).                                 |
| `state`            | `string` / `object`        | State or province where the sighting occurred.                              | `NM`                      | Standardized case (e.g., Uppercase for US states).                    |
| `country`          | `string` / `object`        | Country where the sighting occurred.                                        | `US`                      | Standardized case (e.g., Uppercase 2-letter ISO code if possible).    |
| `shape`            | `string` / `object`        | Reported shape of the UFO/UAP.                                              | `Triangle`                | Standardized case (e.g., Title Case). Values like "Unknown" for NaNs. |
| `duration_seconds` | `float64` / `int64`        | Duration of the sighting in seconds.                                        | `300.0`                   | May contain NaNs if duration was not parseable or reported.           |
| `comments`         | `string` / `object`        | Text description or comments about the sighting.                            | `Bright light moving fast.` | Preserves original text as much as possible. NaNs if no comments.     |
| `latitude`         | `float64`                  | Latitude of the sighting location.                                          | `33.9808`                 | WGS84 decimal degrees. NaNs if not available/valid.                   |
| `longitude`        | `float64`                  | Longitude of the sighting location.                                         | `-104.5260`               | WGS84 decimal degrees. NaNs if not available/valid.                   |

*(Additional columns might be included if deemed valuable after initial data exploration, but the above form the core schema for the planned application features.)*

## 5. Data Volume and Size Considerations

*   The original NUFORC dataset can contain over 80,000 rows.
*   Preprocessing aims to reduce the dataset to essential columns and clean data, which should result in a Parquet file of manageable size (ideally a few MB to tens of MB) to ensure quick loading times by the Google Cloud Run backend instance.
*   The choice of Parquet format with compression will significantly help in keeping the file size small compared to CSV.

## 6. Data Refresh Strategy

*   For this project (a job application demo), the dataset is considered static. There is no requirement for ongoing data refresh. The `nuforc_data.parquet` file will be generated once and bundled with the application.

This data schema and preprocessing plan provide a clear path for preparing the dataset that will power the Interactive UFO/UAP Sightings Explorer.
