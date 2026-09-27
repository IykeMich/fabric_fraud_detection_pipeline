# Fabric Lord Project Assets

All data is synthetic and safe to use for learning.

## Main datasets
- customers.csv — 10,000 customer records
- merchants.csv — 1,000 merchant records
- terminals.csv — 2,000 terminal records
- transactions.csv — 100,740 transaction records
- dirty_transactions.csv — 40 intentionally duplicated/dirty records for quarantine testing
- dim_date_seed.csv — 214 date dimension seed
- api_reference_fixture.csv — small reference dataset representing an API-shaped source
- control_state_seed.csv — empty control table seed
- quarantine_reasons.csv — error classification lookup

## Important
`api_reference_fixture.csv` is a fixture, not a live API. The capstone should still include a REST/API ingestion exercise. You can later expose equivalent JSON/CSV content through a GitHub raw file or use a public reference API.

## Load order
1. Create the Fabric workspace and Lakehouse.
2. Create the external ADLS Gen2 landing zone.
3. Upload the CSV files under `raw/fabric-lord/`.
4. Upload `ingestion_config.json` under `raw/config/`.
5. Build the metadata-driven pipeline.
6. Land Bronze data before Silver/Gold transformations.
7. Use `dirty_transactions.csv` only for the failure/quarantine lab.
8. Use `payment_events_sample.jsonl` for event-shape testing before sending events through Event Hubs.

## Synthetic fraud seeds
The transaction data deliberately contains clusters representing rapid activity, impossible-location changes, and high-value new-device transactions. These are seeds for the fraud-rule exercises; do not treat the labels as ground-truth production fraud.
