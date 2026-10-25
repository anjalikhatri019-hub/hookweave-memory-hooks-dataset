# v4.0.0 two-slot hook selection dataset

Asset: `dataset_public.zip`, 299,519,978 bytes, SHA-256 `852acf62b3a51ba870b0bd1932941419a947cd3673cf0d5901e06edb85e5fcd5`.

Archive members: `train.jsonl`, `test.jsonl`, `manifest.json`, `LICENSE_DATA.md`. The train split has 4,000 rows; the test split has 1,000 rows. This release intentionally excludes private answers and organizer generation/evaluation code.

Structural repair: capture, restore, and expire candidates have been removed. Each case has one refresh and one merge candidate for each of four queried slots; select two different slots, yielding 24 legal pairs. Queried slots are selected so every candidate changes its initial value. Action credit is `1-(best public utility - selected utility)`, so every maximum-utility pair receives full action credit, even when it differs from the training oracle. Public events, private grading records, Prepare, Grade, title, and descriptions were rebuilt together. Previous releases remain available but are incompatible with v4.0.0.
