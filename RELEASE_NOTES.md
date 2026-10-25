# v3.0.0 participant dataset

Asset: `dataset_public.zip`, 299,527,818 bytes, SHA-256 `c61dbb6afb1412c48068bfea6fc65ab7dab510850e716fde75f56155bd009a98`.

Archive members: `train.jsonl`, `test.jsonl`, `manifest.json`, `LICENSE_DATA.md`. The train split has 4,000 rows; the test split has 1,000 rows. This release intentionally excludes private answers and organizer generation/evaluation code.

Specification rebuild: both train and test now include `goal_values`, a public target derived from visible source roles, review markers, and report recency. Pair utility is exactly the fraction of these public values matched. Every hook outcome, evidence index, and utility can be calculated from a public row; no hidden true-value target is required. Public events, queries, candidates, hook semantics, submission schema, license, and split are unchanged from v2.0.1, which remains preserved as the prior release.
