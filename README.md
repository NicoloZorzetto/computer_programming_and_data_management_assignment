# computer_programming_and_data_management_assignment
Code and data for the `Computer Programming and Data Management` course

# Structure
The `data/` directory contains all data used for the analysis with both raw and intermediate transformations in appropriately named subfolders.

The `src/` directory contains the Jupyter Notebooks used for the download (where possible), wrangling, merging and then analysis of the data.

# Code
## Download, wrangling and merge
The `src/data_download_and_merge.ipynb` notebook contains the automatic download, concatenation and cleanup of tariff data from the dataweb portal (https://dataweb.usitc.gov/tariff/annual) which is then saved to `data/tariffs/hs_annual_tariffs_cleaned.csv`.

It then parses the raw files obtained from the U.S. dataweb trade platform (https://dataweb.usitc.gov/) - which was not possible to download automatically - to create aggregated (complete) tradeflow-specific files which are then joined to create the `data/trade/merged_trade_data.csv`.

The 2 are joined by masking the trade data by country (through a manual mapping of country to country-rate column names) to filter the appropriate rows on which to apply the provided rates.

The "final" dataset used in further steps is saved to `data/trade_tariffs.csv`.

All preliminary steps are executed conditionally, if:
1. The intermediate files mentioned above are missing, or do not match the expected sha256 hash (indicating corruption or tampering)
2. The raw (source) files are present

It is possible to spot-download tariff data (by year) based on what is missing in the `data/tariffs/raw` directory, to re-create the tariff data needed for the final merge, but this is not done if the `data/tariffs/hs_annual_tariffs_raw_appended.csv` file is present and corresponds to the sha256 hash computed during the creation of the dataset.

It is not possible to automate the download of trade data from the portal, so a simple "presence" check is executed in case they were to be needed.
This actually contains a further preliminary check to see if the 'intermediate' files (e.g. `data/trade/imports_for_consumption/hts10_imports_for_consumption.csv`) are present and respect their sha256 checksums. If they are, and considered intact, the `data/trade/merged_trade_data.csv` dataset is rebuilt from them, avoiding unnecessary excel file parsing.

# Notes
While tariff and trade data are publicly available from the U.S. International Trade Commission, they are not distributed in a unified, analysis-ready format. This project contributes by constructing a harmonized, long-format panel dataset linking U.S. trade flows and tariff measures at the HTS8, year, and partner-country level.

# License
![GitHub license](https://img.shields.io/github/license/NicoloZorzetto/computer_programming_and_data_management_assignment)
