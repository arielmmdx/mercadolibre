# Exercise instructions.

[Exercise instructions](https://github.com/arielmmdx/mercadolibre/blob/main/Data%20Engineer%20-%20Exercise.pdf)

# Main Code

[Jupyter Notebook Resolved- mercadolibre.ipynb](https://github.com/arielmmdx/mercadolibre/blob/main/mercadolibre.ipynb)


This Python script reads three sources of data: 
- `prints.json`: History of value_prop shown to each user for 1 month, in JSON lines format.
- `taps.json`: History of value_prop clicked by a user for 1 month, in JSON lines format.
- `pays.csv`: History of payments made by users for 1 month, in CSV format.

The expected output after transformation is a file `expected_value.csv` that contains:

- Prints from the last week:
  - For each print:
    - A field indicating if a click was made or not.
    - The number of times the user viewed each value_prop in the 3 weeks prior to that print.
    - The number of times the user clicked each value_prop in the 3 weeks prior to that print.
    - The number of payments the user made for each value_prop in the 3 weeks prior to that print.
    - The accumulated amounts the user spent for each value_prop in the 3 weeks prior to that print.

## Requirements

- Python 3.x
- pandas
- datetime
- json
- logging

## Installation

To install the required packages, run:

```bash
pip install pandas
```

## Files

- `prints.json`: Contains print data in JSON lines format.
- `taps.json`: Contains tap data in JSON lines format.
- `pays.csv`: Contains payment data in CSV format.
- `expected_result.csv`: The path where the processed data will be saved.

## Data Types

The expected data types for the columns in the input files are defined as follows:

- `prints.json` and `taps.json`:
  - `day`: 'datetime64[ns]'
  - `user_id`: integer
  - `position`: integer
  - `value_prop`: string

- `pays.csv`:
  - `pay_date`: 'datetime64[ns]'
  - `total`: float
  - `user_id`: integer
  - `value_prop`: string

- `expected_result.csv`:
  - `user_id`: int
  - `day`: 'datetime64[ns]'
  - `value_prop`: str
  - `clicked`: bool
  - `views_3w`: int
  - `clicks_3w`: int
  - `payments_3w`: int
  - `total_spent_3w`: float

## Script Overview

The script performs the following steps:

1. **Setup Logging**: Configures logging to capture and log information during the script execution.

2. **Read Data**:
   - Reads `prints.json` and `taps.json` files into pandas DataFrames using the `read_json_lines_to_dataframe` function.
   - Reads `pays.csv` file into a pandas DataFrame using the `read_csv_to_dataframe` function.

3. **Process Data using `process_and_save_dataframe` function**:
   - Strips whitespace from 'value_prop' columns in all input DataFrames.
   - Converts date columns to datetime and normalizes them.
   - Calculates the last print date and the start date of the last week.
   - Filters prints for the last week based on the last week start date.
   - Merges taps data with prints to add a 'clicked' column.
   - Merges taps and pays data with prints.
   - Filters prints, taps, and pays data for the last three weeks.
   - Aggregates views, clicks, payments, and total spent in the last three weeks.
   - Merges aggregated data with prints.
   - Selects required columns and fills NA values with 0.
   - Converts columns to appropriate data types and rounds 'total_spent_3w' to 4 decimal places.
   - Save Processed Data: Saves the processed DataFrame to `expected_result.csv`.

4. **Final Expected Results**:
    -  [expected_result.csv](https://raw.githubusercontent.com/arielmmdx/mercadolibre/main/expected_result.csv)

# Testing Code
Manual testing to verify per user that `expected_result.csv` is correct by comparing it against the original files `prints.json`, `taps.json`, and `pays.csv`.
