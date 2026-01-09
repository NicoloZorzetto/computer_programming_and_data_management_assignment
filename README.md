# computer_programming_and_data_management_assignment
Code and data for the `Computer Programming and Data Management` course

# Structure
The `data/` directory contains all data used for the analysis with both raw and intermediate transformations in appropriately named subfolders.

The `src/` directory contains the Jupyter Notebooks used for the download (where possible), wrangling, merging and then analysis of the data.

# Code
## Structure
- `src/data_download_and_merge.ipynb` — data acquisition, wrangling and merging with integrity checks
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

# Output Files
- `data/tariffs/hs_annual_tariffs_cleaned.csv`
- `data/trade/merged_trade_data.csv`
- `data/trade_tariffs.csv` — final dataset used for analysis

# Cookbook
| Variable name | Description |
|---|---|
| year | Reference year of the observation. |
| hts8 | 8-digit Harmonized Tariff Schedule (HTS) product code used to match trade flows with tariff schedules. |
| country | Trade partner country associated with the trade flow. |
| ifc_dutiable_value | Dutiable value of imports for consumption, i.e. the portion of import value subject to tariffs. |
| ifc_customs_value | Total customs value of imports for consumption. |
| ifc_first_unit_of_quantity | First quantity measure for imports for consumption, expressed in the unit described in the original data. |
| ifc_second_unit_of_quantity | Second quantity measure for imports for consumption, available only for selected products. |
| ifc_landed_duty_paid_value | Landed, duty-paid value of imports for consumption, when reported. |
| ifc_calculated_duties | Calculated import duties associated with imports for consumption. |
| fex_first_unit_quantity | First quantity measure for foreign exports. |
| fex_second_unit_quantity | Second quantity measure for foreign exports, available only for selected products. |
| fex_fas_value | Value of foreign exports measured on a Free Alongside Ship (FAS) basis. |
| dex_first_unit_quantity | First quantity measure for domestic exports. |
| dex_second_unit_quantity | Second quantity measure for domestic exports, available only for selected products. |
| dex_fas_value | Value of domestic exports measured on a Free Alongside Ship (FAS) basis. |
| ifc_suppressed_any | Indicator equal to 1 if any imports-for-consumption data for the observation are suppressed for confidentiality reasons. |
| img_suppressed_any | Indicator equal to 1 if any general imports data for the observation are suppressed for confidentiality reasons. |
| fex_suppressed_any | Indicator equal to 1 if any foreign exports data for the observation are suppressed for confidentiality reasons. |
| dex_suppressed_any | Indicator equal to 1 if any domestic exports data for the observation are suppressed for confidentiality reasons. |
| brief_description | Short textual description of the product from the tariff schedule. |
| mfn_ave_clean | Average Most-Favored-Nation (MFN) tariff rate where available; only defined for a subset of products and years. |
| mfn_ad_val_rate | MFN ad valorem tariff rate (percentage), consistently reported across products and years. |
| tariff_specific_adval | Constructed indicator identifying tariff lines with ad valorem (as opposed to specific or textual) tariff rates. |

# Notes
While tariff and trade data are publicly available from the U.S. International Trade Commission, they are not distributed in a unified, analysis-ready format. This project contributes by constructing a harmonized, long-format panel dataset linking U.S. trade flows and tariff measures at the HTS8, year, and partner-country level.

# License
![GitHub license](https://img.shields.io/github/license/NicoloZorzetto/computer_programming_and_data_management_assignment)
