# HOOKWEAVE — Two-Slot Memory Hook Selection

## Objective

An assistant has conflicting reports about 20 memory slots. For each case, select exactly two hooks that update two different queried slots, then submit the four resulting memory values and their source-event indices. All hook outcomes, goals, and scores are determined by that case's public events. A case has 360 chronological events of the form `[slot, reported_value, source_role, review_epoch, note]`; values are integers 0–9, and source roles are 0 user, 1 assistant, 2 tool, and 3 delegate. Each note ends with exactly one visible marker: `Public review marker: cross-checked.` or `Public review marker: provisional.` Event indices are zero-based.

The four queried slots are listed in `query` order. Each slot's initial memory value and evidence index come from its last report, regardless of marker; a slot with no report starts at `[-1,-1]`. Each case supplies eight shuffled candidates `[kind, slot]`: exactly one `refresh` and one `merge` for each queried slot. Candidate indices are local to the case. The two hooks operate on different slots, so each changes only its own slot and their order has no effect. The candidate list is shuffled independently per case. Each queried slot is selected so its initial value, refreshed value, and merged value are three distinct integers; consequently, every candidate changes its slot's value from the initial state.

- `refresh` loads the value and index of the slot's latest `cross-checked` report. Every queried slot has one.
- `merge` examines the slot's last nine reports, counting both marker types; if fewer than nine exist, it uses all of them. It loads the most frequent value and the latest event index reporting that value. If counts tie, choose the value whose latest report is most recent.

The public `goal_values` array gives one target for each queried slot. For that slot, take the latest `cross-checked` user report; if none exists, the latest `cross-checked` report from any role; if none exists, the latest report; if no report exists, `-1`. The goals do not depend on the selected hooks. A participant can replay and compare all 24 legal pairs from public events alone. Training rows also contain `oracle_program`, `oracle_answers`, and `oracle_evidence` for one maximum-utility pair; test rows omit those three fields but include `goal_values`. The training oracle is one example of a maximizer, not a unique required program.

## Submission

Write `./working/submission.csv` with exactly `case_id,program_json,answers_json,evidence_json`. `program_json` is a JSON list `[a,b]` of two distinct candidate indices from 0–7, in increasing order (`a<b`), whose candidate slots differ. The sorted order is a canonical representation of a two-hook set. `answers_json` is a JSON list of four integers -1–9; `evidence_json` is a JSON list of four integers -1–359, both in `query` order. Include one row for every test ID. Extra non-evaluated IDs are ignored; row order does not matter. An invalid evaluated row scores zero. Missing or duplicate evaluated IDs, missing or extra columns, or an unparsable table score zero for the submission.

## Evaluation

For a legal pair `p`, replay its hooks from public events to obtain four values and four evidence indices. Its utility `u(p)` is the fraction of its four values equal to the four public `goal_values`. Let `h` be the maximum utility among the 24 legal pairs. The action component is `A = 1 - (h - u(p))`. Thus `A=1` exactly when `p` has maximum utility, including ties; the grader never compares `p` to the particular training oracle index. Let `R` be the fraction of submitted values equal to the replayed values and `E` the fraction of submitted evidence indices equal to the replayed indices. The case score is `(A*R*E)^(1/3)`, and the final score is the mean across test cases. All components and the final score lie in [0,1]. Every legal pair's score can be calculated from its public test row.

## Compute and rules

The platform tier is one A10G GPU, 64 GiB RAM, and 60 minutes; an exact CPU solution is allowed. Use only the public archive and write the required submission. Network access and organizer-only answer access are prohibited. Test IDs, archive ordering, hashes, and cross-case test statistics may not be used as answer keys. This is a creator-owned synthetic benchmark of memory behavior, not measured production assistant activity.
