# HOOKWEAVE — Ordered Memory Hook Replay

## Objective

An assistant has a long stream of conflicting notes about a user's evolving workspace. Before answering four later requests, it may execute exactly two distinct memory hooks. Submit the two hooks in execution order, the four values that the resulting memory will expose, and the event index supplying each exposed value. The evaluator replays the **selected** hook sequence; choosing a different sequence changes both answer and evidence targets.

Each case contains 360 chronological events over 20 numbered slots. An event is `[slot, reported_value, source_role, review_epoch, note]`. Values are integers 0–9. The note is part of the event and paraphrases its claim. Source roles are 0 user, 1 assistant, 2 tool, and 3 delegate. Reports may conflict. Each note includes a noisy verification cue; hidden verification status must be inferred from training examples and is not supplied in test. A verifier may also record a true value change. Cases are independent. The four queried slots appear in `query` order.

Eight candidate hooks are supplied per case as `[kind, slot]` in a separately shuffled list. The candidate list index has no global meaning. `refresh` loads the most recent privately verified event for its slot, if any; `merge` loads the plurality value among its last nine reports, breaking ties by the most recent report; `capture` saves its slot's current value and source event; `restore` reinstates the most recent captured pair, if present; `expire` clears its slot to unknown. `restore` uses the captured slot even if its own listed slot differs. The memory starts with the last reported event for each slot, or unknown if absent. A cleared value and its evidence index are both `-1`. Each event index is zero-based within its case.

The public archive contains `train.jsonl`, `test.jsonl`, `manifest.json`, and `LICENSE_DATA.md`. Each JSONL line is one case. Training cases include `oracle_program`, `oracle_answers`, and `oracle_evidence`; test cases omit them. Train and test hold out joint reliability/drift regime pairings, while individual regime levels appear in both. The regimes themselves are not input fields. The private verification states and outcomes are not participant data.

## Submission

Write `./working/submission.csv` with exactly `case_id,program_json,answers_json,evidence_json`. The JSON fields are respectively a list of two **distinct** integers 0–7, a list of four integers -1–9, and a list of four integers -1–359. Include exactly one row for every test ID. Extra non-evaluated IDs are ignored. Row order does not matter. An invalid evaluated row scores zero. A missing or duplicate evaluated ID, missing/extra column, or unparsable table scores zero for the submission.

## Evaluation

For the submitted program, the private replay produces four values and four event indices. `R` is exact-position accuracy of the four submitted values; `E` is exact-position accuracy of the four submitted evidence indices. The program's hidden utility is the fraction of its four replayed values matching the current verified values, minus `0.01` for each selected `refresh` or `merge`. Let `lo` and `hi` be the minimum and maximum utility among all 56 legal ordered pairs. `A=(utility-lo)/(hi-lo)`; if `hi=lo`, `A=1`. The case score is `(A*R*E)^(1/3)`, and the final score is the mean over evaluated cases. The range is [0,1], and the exact reference scores 1.0. An inaccurate or irrelevant hook can lower `A`; even a good program cannot score highly without predicting its actual memory and evidence.

## Compute and rules

The intended tier is one A10G GPU, 64 GiB RAM, and 60 minutes. CPU solutions are allowed. Read only the public archive and write the required submission. Network access and private-answer access are prohibited. Training labels may be used for fitting; test IDs, archive ordering, hashes, or cross-case test statistics may not be used as answer keys. This is a creator-owned synthetic benchmark of memory behavior, not measured production assistant activity.
