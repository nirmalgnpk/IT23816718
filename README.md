# IT23816718 Test Automation

This repository contains a Python Playwright script that reads Singlish test cases from an Excel workbook, types each input into the chat translator UI, captures the Sinhala output, and writes the results back into the workbook.

## What it does

- Loads test rows from Excel.
- Auto-detects the header row and matching input / expected / status columns.
- Supports merged cells in the test sheet.
- Runs against the default chat translator page or a custom URL.
- Writes `Actual output` and `Status` columns back to the workbook.

## Requirements

- Python 3.10 or newer
- `playwright`
- `openpyxl`
- Chromium installed through Playwright

Install the Python dependencies and browser runtime with:

```powershell
py -m pip install playwright openpyxl
py -m playwright install chromium
```

## Main Script

- `test_automation.py` — automation runner

## Excel Workbook Expectations

The script tries to find a column that matches one of these input names:

- Singlish
- Input
- Singlish Input
- Test Input
- Source
- Sentence
- Text

It also looks for an expected Sinhala output column such as:

- Sinhala
- Expected Output
- Expected
- Expected Sinhala

If `Actual output` or `Status` columns do not already exist, the script creates them automatically.

The header row can be auto-detected. If your sheet is unusual, you can set it manually with `--header-row`.

## Run

Example command with explicit workbook columns:

```powershell
py test_automation.py --excel "D:\3Y 1S\ITPM\IT23816718\IT23816718.xlsx" --url "https://www.pixelssuite.com/chat-translator" --wait-ms 5000 --type-delay-ms 80 --slow-mo-ms 200 --save-every 1 --keep-open --input-col "Input" --actual-col "Actual output" --status-col "Status"
```

If you want to save the results to a different file, pass `--output`.

## Useful Options

- `--excel`: input workbook path. Can be absolute or relative.
- `--output`: save results to a different workbook.
- `--sheet`: worksheet name. Default is the script's built-in ` Test cases` sheet.
- `--header-row`: use a specific header row when auto-detection is not enough.
- `--input-col`, `--expected-col`, `--actual-col`, `--status-col`: override column names.
- `--url`: frontend URL to test.
- `--headless`: run without opening the browser window.
- `--wait-ms`: wait after typing and submitting each row.
- `--retries`: retry reading the output if the page updates slowly.
- `--retry-wait-ms`: wait between output retries.
- `--type-delay-ms`: add a typing delay for each character.
- `--timeout-ms`: default timeout for page actions.
- `--slow-mo-ms`: slow down browser actions for easier debugging.
- `--save-every`: save the workbook after every N processed rows.
- `--keep-open`: keep the browser open after the run finishes.

## Status Values

| Status | Meaning |
|---|---|
| `PASS` | Output matches the expected Sinhala text |
| `FAIL` | Output did not match the expected text |
| `COLLECTED` | Output was captured but no expected value was present |
| `UI Error` | Automation failed for that row |

## Example Workflow

1. Prepare an Excel file with Singlish inputs and expected Sinhala output.
2. Run the script with the workbook path.
3. Review the updated `Actual output` and `Status` columns.
4. Re-run with a higher wait time if the site responds slowly.

## Troubleshooting

- If the script says it cannot resolve the input column, check the header names in the workbook.
- If the frontend does not load, verify the URL and internet connection.
- If outputs are empty or unchanged, increase `--wait-ms`, `--retries`, or `--retry-wait-ms`.
- If you want to watch the browser, do not use `--headless`.
