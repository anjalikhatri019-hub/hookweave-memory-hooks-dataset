# v1.0.2 participant dataset

Asset: `dataset_public.zip`, 292,207,665 bytes, SHA-256 `2b092296cbd5e3b622767f518d7fc9f2a5e1221fa6d7c5f6fb1912a978afe460`.

Archive members: `train.jsonl`, `test.jsonl`, `manifest.json`, `LICENSE_DATA.md`. The train split has 4,000 rows; the test split has 1,000 rows. This release intentionally excludes private answers and organizer generation/evaluation code.

Documentation update: the challenge description now distinguishes visible last-report initialization and report-based `merge` from latent-verification `refresh`. It explicitly frames test outcomes and final truth as supervised prediction targets, including the possibility of irreducible uncertainty. The dataset description states the same limitation. Dataset bytes are unchanged from v1.0.1.
