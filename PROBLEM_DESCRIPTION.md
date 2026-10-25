# HOOKWEAVE — Ordered Memory Hook Replay

## Objective

An assistant has a long stream of conflicting notes about a user's evolving workspace. Before answering four later requests, it may execute exactly two distinct memory hooks. Submit the two hooks in execution order, then give the four resulting memory values and their source-event indices. Every hook transition, value, evidence index, and pair utility is determined by the public case. Choose the legal ordered pair whose replayed values best match the case's public `goal_values`.

Each case contains 360 chronological events over 20 numbered slots. An event is `[slot, reported_value, source_role, review_epoch, note]`. Values are integers 0–9. Source roles are 0 user, 1 assistant, 2 tool, and 3 delegate. The note paraphrases the claim and ends with exactly one public marker: `Public review marker: cross-checked.` or `Public review marker: provisional.` Reports may conflict. The marker is visible in both train and test and controls `refresh`; it is not a hidden verification flag. Cases are independent. The four queried slots appear in `query` order.

Eight candidate hooks are supplied per case as `[kind, slot]` in a separately shuffled list. The candidate list index has no global meaning. For each slot, **initial memory** is its last reported value and that event's index, regardless of its public marker; if no event reports that slot, both are `-1`. The following hooks run in submitted order:

- `refresh` replaces its slot with the value and index of its latest publicly marked `cross-checked` report, if one exists; otherwise it leaves that slot unchanged. It reads the literal marker in `note` and no private state.
- `merge` examines the last nine reports for its slot, counting both public marker types. It loads the most frequent reported value and the latest event index reporting that value. A count tie goes to the value whose latest report is most recent. If fewer than nine reports exist, it uses all of them; if none exist, it leaves the slot unchanged.
- `capture` saves its slot's current value and source-event index. A later capture replaces the saved pair.
- `restore` reinstates the last captured value and source-event index at the *captured* slot, if a capture exists; otherwise it does nothing. Its listed slot does not redirect the restore.
- `expire` sets its slot's value and evidence index to `-1`.

For example, if a slot's last report is marked `provisional` but an earlier one is marked `cross-checked`, initialization uses the last report and `refresh` loads the earlier marked report. `merge` uses both reports when counting. This distinction is fully visible. A participant can replay every legal ordered pair from public events alone. Event indices are zero-based within each case.

The public `goal_values` list gives one target value per queried slot. Its rule is independent of the selected hooks: use the latest `cross-checked` report from role 0 (user) for that slot; if none exists, use the latest `cross-checked` report from any role; if none exists, use the slot's latest report; if the slot has no report, use `-1`. All priorities and recency comparisons use only the public events. `goal_values` is supplied in both train and test rows so the best pair can be determined without private labels.

The public archive contains `train.jsonl`, `test.jsonl`, `manifest.json`, and `LICENSE_DATA.md`. Each JSONL line is one case and includes `events`, `query`, `goal_values`, and `candidates`. Training cases additionally include one optimal `oracle_program`, its `oracle_answers`, and its `oracle_evidence`; test cases omit these oracle fields. Train and test hold out joint reliability/drift regime pairings, while individual regime levels appear in both. The regimes themselves are not input fields. The organizer's private answer file contains the same public-computable outcomes and utilities for grading, with no hidden information needed to select a pair.

## Submission

Write `./working/submission.csv` with exactly `case_id,program_json,answers_json,evidence_json`. The JSON fields are respectively a list of two **distinct** integers 0–7, a list of four integers -1–9, and a list of four integers -1–359. Include exactly one row for every test ID. Extra non-evaluated IDs are ignored. Row order does not matter. An invalid evaluated row scores zero. A missing or duplicate evaluated ID, missing/extra column, or unparsable table scores zero for the submission.

## Evaluation

For the submitted program, the evaluator deterministically replays its two hooks from the public events and obtains four values and four event indices. `R` is exact-position accuracy of the submitted values against that replay; `E` is exact-position accuracy of submitted evidence indices. The program's utility is the fraction of its four replayed values matching the four public `goal_values`. Hook kinds have no separate cost or penalty. Let `lo` and `hi` be the minimum and maximum utility among all 56 legal ordered pairs. `A=(utility-lo)/(hi-lo)`; if `hi=lo`, `A=1`. The case score is `(A*R*E)^(1/3)`, and the final score is the mean over evaluated cases. The range is [0,1]. Any pair tied for highest utility can reach `A=1` if its resulting values and evidence indices are submitted correctly. Participants can calculate `lo`, `hi`, and every pair's score from each public test row.

## Compute and rules

The platform tier is one A10G GPU, 64 GiB RAM, and 60 minutes; an exact CPU solution is allowed. Read only the public archive and write the required submission. Network access and private-answer access are prohibited. Test IDs, archive ordering, hashes, or cross-case test statistics may not be used as answer keys. This is a creator-owned synthetic benchmark of memory behavior, not measured production assistant activity.
