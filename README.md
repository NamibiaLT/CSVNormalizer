## The CSV Normalizer App:

This app is a tool that reads a CSV formatted file on `stdin` and
emits a normalized CSV formatted file on `stdout`. Normalized, in this
case, means:

* The entire CSV is in the UTF-8 character set.
* The Timestamp column should be formatted in ISO-8601 format.
* The Timestamp column should be assumed to be in US/Pacific time;
  if it isn't, it is converted to US/Eastern.
* All ZIP codes are formatted as 5 digits. If there are less
  than 5 digits, 0 becomes the prefix for the output CSV.
* All name columns are converted to uppercase.
* The Address column is passed through as is, except for
  Unicode validation. There are commas in the Address
  field; so the CSV parsing takes that into account. Commas
  will only be present inside a quoted string.
* The columns `FooDuration` and `BarDuration` are in HH:MM:SS.MS
  format (where MS is milliseconds); In this normalizer, they are converted to floating point seconds format.
* The column "TotalDuration" is filled with garbage data. For each
  row, the value of TotalDuration is replaced with the sum of
  FooDuration and BarDuration.
* The column "Notes" is free form text input by the end-users and no    transformations are performed on this column. If there are invalid
  UTF-8 characters, they are replaced with the Unicode Replacement
  Character.

### Assumptions:
The input document is in UTF-8 and any times
that are missing timezone information are in US/Pacific. If a
character is invalid, it is replaced with the Unicode Replacement
Character. If that replacement makes data invalid (for example,
because it turns a date field into something unparseable), the normalizer will print a warning to `stderr` and drop the row from your output.

Another assumption is that the sample data contains all date
and time format variants needed.

## Python Version:
Python 3.7

## Requirements:
Install correct package versions by typing this in a terminal:
`pip install -r requirements.txt`

## Testing
The Pytest framework was used for testing. To install pytest and run the test file. First run:
`pip install pytest`
Then type the following command in your terminal:
`pytest test_csv_normalizer.py -v`

## Run app in terminal:
#### To run the app in a terminal, type the following command:
`cat [name_of_your_csv_file.csv] | python3 csv_normalizer.py `
The command will look something like this in your terminal:
`cat sample-with-broken-utf8.csv | python3 csv_normalizer.py`

### Note:
* This code is meant to run on macOS 10.13
