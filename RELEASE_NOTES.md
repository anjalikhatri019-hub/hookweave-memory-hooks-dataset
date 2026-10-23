# v2.0.1 participant dataset

Asset: `dataset_public.zip`, 299,407,834 bytes, SHA-256 `0ce50782318312d257316369e092442cb0660b2b341cf3cf48a57fa109d9867b`.

Archive members: `train.jsonl`, `test.jsonl`, `manifest.json`, `LICENSE_DATA.md`. The train split has 4,000 rows; the test split has 1,000 rows. This release intentionally excludes private answers and organizer generation/evaluation code.

Scoring repair: utility is now exactly the fraction of four private true values matched, with no hook-kind cost. All pairs with the same number of matches have the same utility. Training oracle labels and private targets were rebuilt. Public events, candidates, test rows, hook semantics, submission schema, license, and data split are unchanged from v2.0.0, which remains preserved as the prior release.
