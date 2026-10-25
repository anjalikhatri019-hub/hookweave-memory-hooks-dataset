# v3.0.1 participant dataset and restore clarification

Asset: `dataset_public.zip`, 299,527,818 bytes, SHA-256 `c61dbb6afb1412c48068bfea6fc65ab7dab510850e716fde75f56155bd009a98`.

Archive members: `train.jsonl`, `test.jsonl`, `manifest.json`, `LICENSE_DATA.md`. The train split has 4,000 rows; the test split has 1,000 rows. This release intentionally excludes private answers and organizer generation/evaluation code.

Documentation correction: `restore` checks its listed slot against the most recent capture and writes only to that slot; without a matching capture it is a no-op. Each supplied case has one capture and one restore candidate with equal listed slots. The participant asset is byte-identical to v3.0.0, including the embedded CC BY 4.0 notice whose suggested attribution links to that equivalent earlier release. The challenge description and dataset description now state the keyed rule explicitly. Data, private answers, Prepare, Grade, metric, and submission contract are unchanged.
