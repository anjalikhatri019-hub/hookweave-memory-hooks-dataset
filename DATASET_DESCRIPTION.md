# HOOKWEAVE dataset description

HOOKWEAVE is a 5,000-case synthetic corpus for studying an AI agent's memory updates under conflicting notes. Each case has 360 chronological events about 20 numbered memory slots, four later queries, and eight case-local hook candidates. The 4,000 training cases include a supervised optimal ordered hook pair and the resulting values and source-event indices. The 1,000 test cases omit those labels. Train and test hold out joint reliability/drift regime pairings.

The participant archive contains `train.jsonl`, `test.jsonl`, `manifest.json`, and `LICENSE_DATA.md`. Each JSONL row contains `case_id`, `events`, `query`, and `candidates`; training rows additionally contain `oracle_program`, `oracle_answers`, and `oracle_evidence`. An event is `[slot, reported_value, source_role, review_epoch, note]`. Hidden verification status and all counterfactual replay outcomes remain organizer-only.

The final participant archive is 292 MB, inside the requested 200 MB–2 GB range. This is a creator-owned generated benchmark, not a collection of real conversations. Dataset license: CC BY 4.0. No public source URL or downloadable hosting link exists yet; the local participant archive is `release_final/dataset_public.zip`.
