# HOOKWEAVE — Ordered Memory Hook Replay

## Objective

An assistant has a long stream of conflicting notes about a user's evolving workspace. Before answering four later requests, it may execute exactly two distinct memory hooks. **Predict** the two hooks in execution order, the four values the resulting memory will expose, and the event index supplying each exposed value. The evaluator replays the **selected** hook sequence against its held-out verification and truth states; choosing a different sequence changes both answer and evidence targets. This is a supervised prediction benchmark with latent test labels, not a puzzle whose exact answer is always recoverable from a test row.

Each case contains 360 chronological events over 20 numbered slots. An event is `[slot, reported_value, source_role, review_epoch, note]`. Values are integers 0–9. The note is part of the event and paraphrases its claim. Source roles are 0 user, 1 assistant, 2 tool, and 3 delegate. Reports may conflict. Each note includes a noisy verification cue, which is evidence about the latent verification flag but does not reveal it. The flag and final true slot values are withheld for test cases. Cases are independent. The four queried slots appear in `query` order.

Eight candidate hooks are supplied per case as `[kind, slot]` in a separately shuffled list. The candidate list index has no global meaning. For each slot, **initial memory** is its last *reported* value and that event's index, regardless of verification; if no event reports that slot, both are `-1`. The following hooks run in submitted order:

- `refresh` replaces its slot with the most recent *verified* event's value and index, if one exists; otherwise it leaves that slot unchanged. Which event is verified is latent in test cases, so this hook's exact effect may be uncertain.
- `merge` examines the last nine *reported* events for its slot, counting all reports regardless of verification. It loads the most frequent reported value and the latest event index reporting that value. A count tie goes to the value whose latest report is most recent. Its effect is computable from public events.
- `capture` saves its slot's current value and source-event index. A later capture replaces the saved pair.
- `restore` reinstates the last captured value and source-event index at the *captured* slot, if a capture exists; otherwise it does nothing. Its listed slot does not redirect the restore.
- `expire` sets its slot's value and evidence index to `-1`.

These rules do not assert that initial memory is verified. For example, a slot's last report can be unverified while an earlier report is verified: initialization uses the last report, whereas `refresh` uses the earlier verified report. A program without `refresh` can be replayed from public events alone; a program with `refresh` may require predicting latent verification. Event indices are zero-based within each case.

The public archive contains `train.jsonl`, `test.jsonl`, `manifest.json`, and `LICENSE_DATA.md`. Each JSONL line is one case. Training cases include `oracle_program`, `oracle_answers`, and `oracle_evidence`; test cases omit them. Train and test hold out joint reliability/drift regime pairings, while individual regime levels appear in both. The regimes themselves are not input fields. Training labels allow models to learn from visible roles, reports, and noisy cues. Exact hidden verification flags, final true values, and counterfactual hook outcomes are not participant data. Consequently, no deterministic method using only a test row is guaranteed to recover every exact test target; valid but imperfect predictions receive partial credit.

## Submission

Write `./working/submission.csv` with exactly `case_id,program_json,answers_json,evidence_json`. The JSON fields are respectively a list of two **distinct** integers 0–7, a list of four integers -1–9, and a list of four integers -1–359. Include exactly one row for every test ID. Extra non-evaluated IDs are ignored. Row order does not matter. An invalid evaluated row scores zero. A missing or duplicate evaluated ID, missing/extra column, or unparsable table scores zero for the submission.

## Evaluation

For the submitted program, the private replay uses the case's hidden verification flags to produce four realized values and four event indices. `R` is exact-position accuracy of the four submitted values; `E` is exact-position accuracy of the four submitted evidence indices. The program's hidden utility is the fraction of its four replayed values matching the final private true values, minus `0.01` for each selected `refresh` or `merge`. Let `lo` and `hi` be the minimum and maximum utility among all 56 legal ordered pairs. `A=(utility-lo)/(hi-lo)`; if `hi=lo`, `A=1`. The case score is `(A*R*E)^(1/3)`, and the final score is the mean over evaluated cases. The range is [0,1]. An organizer-only exact reference scores 1.0; this is a grader check, not a promise that public inputs determine the private labels. An inaccurate or irrelevant hook can lower `A`; even a good program cannot score highly without predicting its actual memory and evidence.

## Compute and rules

The intended tier is one A10G GPU, 64 GiB RAM, and 60 minutes. CPU solutions are allowed. Read only the public archive and write the required submission. Network access and private-answer access are prohibited. Training labels may be used for fitting; test IDs, archive ordering, hashes, or cross-case test statistics may not be used as answer keys. This is a creator-owned synthetic benchmark of memory behavior, not measured production assistant activity.
