# v2.0.0 participant dataset

Asset: `dataset_public.zip`, 299,407,835 bytes, SHA-256 `6b01c21f6ffd6dbbe4ae3bfbf9ef6a0063c6e1e6274ddb7fd003cae875c9cc99`.

Archive members: `train.jsonl`, `test.jsonl`, `manifest.json`, `LICENSE_DATA.md`. The train split has 4,000 rows; the test split has 1,000 rows. This release intentionally excludes private answers and organizer generation/evaluation code.

Structural update: `refresh` now reads only the literal public `cross-checked` review marker. All 56 ordered programs' resulting values and event indices can be computed from a public case. Private final true values affect action utility only. Training labels, private targets, and the participant archive were rebuilt; v1.0.2 remains preserved as the prior release.
