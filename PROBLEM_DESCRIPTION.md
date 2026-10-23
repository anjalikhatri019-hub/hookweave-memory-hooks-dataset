# HOOKWEAVE — Ordered Memory Hook Replay

## Objective

An assistant has a long stream of conflicting notes about a user's evolving workspace. Before answering four later requests, it may execute exactly two distinct memory hooks. Submit the two hooks in execution order, then give the four resulting memory values and their source-event indices. **Every hook transition, value, and evidence index is determined by the public events and selected hooks.** The prediction problem is choosing a pair whose resulting values best match private final true values. Choosing a different pair can change the values and evidence that must be submitted.

Each case contains 360 chronological events over 20 numbered slots. An event is `[slot, reported_value, source_role, review_epoch, note]`. Values are integers 0–9. Source roles are 0 user, 1 assistant, 2 tool, and 3 delegate. The note paraphrases the claim and ends with exactly one public marker: `Public review marker: cross-checked.` or `Public review marker: provisional.` Reports may conflict. The marker is visible in both train and test and controls `refresh`; it is not a hidden verification flag. Cases are independent. The four queried slots appear in `query` order.

Eight candidate hooks are supplied per case as `[kind, slot]` in a separately shuffled list. The candidate list index has no global meaning. For each slot, **initial memory** is its last reported value and that event's index, regardless of its public marker; if no event reports that slot, both are `-1`. The following hooks run in submitted order:

- `refresh` replaces its slot with the value and index of its latest publicly marked `cross-checked` report, if one exists; otherwise it leaves that slot unchanged. It reads the literal marker in `note` and no private state.
- `merge` examines the last nine reports for its slot, counting both public marker types. It loads the most frequent reported value and the latest event index reporting that value. A count tie goes to the value whose latest report is most recent. If fewer than nine reports exist, it uses all of them; if none exist, it leaves the slot unchanged.
- `capture` saves its slot's current value and source-event index. A later capture replaces the saved pair.
- `restore` reinstates the last captured value and source-event index at the *captured* slot, if a capture exists; otherwise it does nothing. Its listed slot does not redirect the restore.
- `expire` sets its slot's value and evidence index to `-1`.

For example, if a slot's last report is marked `provisional` but an earlier one is marked `cross-checked`, initialization uses the last report and `refresh` loads the earlier marked report. `merge` uses both reports when counting. This distinction is fully visible. A participant can replay every legal ordered pair from public events alone. Event indices are zero-based within each case.

The public archive contains `train.jsonl`, `test.jsonl`, `manifest.json`, and `LICENSE_DATA.md`. Each JSONL line is one case. Training cases include `oracle_program`, `oracle_answers`, and `oracle_evidence`; test cases omit them. Train and test hold out joint reliability/drift regime pairings, while individual regime levels appear in both. The regimes themselves are not input fields. Training labels can be used to learn which publicly replayable pair is useful. Final true slot values, which determine a pair's utility, remain private test labels; they do not affect replayed memory or evidence.

## Submission

Write `./working/submission.csv` with exactly `case_id,program_json,answers_json,evidence_json`. The JSON fields are respectively a list of two **distinct** integers 0–7, a list of four integers -1–9, and a list of four integers -1–359. Include exactly one row for every test ID. Extra non-evaluated IDs are ignored. Row order does not matter. An invalid evaluated row scores zero. A missing or duplicate evaluated ID, missing/extra column, or unparsable table scores zero for the submission.

## Evaluation

For the submitted program, the evaluator deterministically replays its two hooks from the public events and obtains four values and four event indices. `R` is exact-position accuracy of the submitted values against that replay; `E` is exact-position accuracy of submitted evidence indices. The program's utility is the fraction of its four replayed values matching the final private true values. Hook kinds have no separate cost or penalty. Let `lo` and `hi` be the minimum and maximum utility among all 56 legal ordered pairs. `A=(utility-lo)/(hi-lo)`; if `hi=lo`, `A=1`. The case score is `(A*R*E)^(1/3)`, and the final score is the mean over evaluated cases. The range is [0,1]. The organizer-only exact reference scores 1.0. A pair with fewer true-value matches lowers `A`; a good pair still needs its publicly computable values and evidence submitted correctly. If several pairs tie for highest utility, any of them can reach `A=1`.

## Compute and rules

The intended tier is one A10G GPU, 64 GiB RAM, and 60 minutes. CPU solutions are allowed. Read only the public archive and write the required submission. Network access and private-answer access are prohibited. Training labels may be used for fitting; test IDs, archive ordering, hashes, or cross-case test statistics may not be used as answer keys. This is a creator-owned synthetic benchmark of memory behavior, not measured production assistant activity.
