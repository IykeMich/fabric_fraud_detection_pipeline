# Fabric Lord — Fabric snippets

## A. Masterclass-derived metadata-driven parameter
Create a pipeline parameter:
- Name: `files`
- Type: `Array`

Example value:
```json
[
  {"file":"customers.csv","key":["customer_id"]},
  {"file":"merchants.csv","key":["merchant_id"]},
  {"file":"terminals.csv","key":["terminal_id"]},
  {"file":"transactions.csv","key":["transaction_id"]}
]
```

The source masterclass specifically demonstrates dictionary objects with `file` and `key`, and notes that Upsert key columns must be a list.

## B. For Each items
If using the pipeline parameter:
```text
@pipeline().parameters.files
```

Inside the loop, the current dictionary is available as:
```text
@item()
```

Individual properties:
```text
@item().file
@item().key
```

## C. Dynamic source filename / relative URL
Generic pattern:
```text
@concat('<base-relative-path>/', item().file)
```

If you are building a larger string:
```text
<fixed-text>@{item().file}
```

Do not hardcode the filename when the pipeline is supposed to be metadata-driven.

## D. Dynamic destination table
Example:
```text
@item().target_table
```
or, when the table name is the file stem, derive it from the configured metadata rather than hardcoding it.

## E. Lookup → For Each
If `config.json` has:
```json
{"configs":[ ... ]}
```
and Lookup is named `lookup config`, the masterclass uses:
```text
@activity('lookup config').output.value.configs
```

## F. Get Metadata → For Each
Set the For Each Items expression to:
```text
@activity('get metadata folder').output.childItems
```

## G. Filter only CSV files
If Condition:
```text
@endsWith(item().name, '.csv')
```

## H. File name inside the loop
```text
@item().name
```

## I. Count processed files
Append Variable:
```text
@activity('copy data').output.filesRead
```

Count the array after the loop:
```text
@length(variables('files_number'))
```

## J. Capstone metadata configuration
The project version adds fields for dataset name, target table, load method and active flag:
```json
{
  "dataset_name": "transactions",
  "file": "transactions.csv",
  "key": ["transaction_id"],
  "source_format": "CSV",
  "target_schema": "bronze",
  "target_table": "transactions",
  "load_method": "append",
  "active": true
}
```

## K. Incremental control-state table
Recommended columns:
```text
dataset_name
last_successful_timestamp
last_successful_run_id
status
updated_at
```

## L. Fraud alert minimum schema
```text
alert_id
transaction_id
customer_id
rule_id
rule_name
severity
alert_timestamp
amount
location
device_id
status
explanation
```

## M. Example fraud rule logic
Rapid activity:
```text
COUNT(successful transactions)
OVER customer_id within a 3-minute window >= 4
```

Impossible location:
```text
same customer
+ different locations
+ timestamp gap <= 10 minutes
```

High-value new device:
```text
amount >= 300000
AND device_id not previously associated with customer
```

## N. Event Hub message shape
Each event should be JSON matching `payment_event_schema.json`.

## O. Evidence to capture
For each major lab keep:
1. pipeline canvas screenshot
2. parameter/config screenshot
3. successful run screenshot
4. output row-count/sample screenshot
5. failure/retry/quarantine evidence where applicable
6. short explanation of the design decision
