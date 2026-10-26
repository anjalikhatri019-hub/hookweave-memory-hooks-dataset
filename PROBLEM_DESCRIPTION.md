# HOOKWEAVE — Checkpointed Memory Hook Scheduling

## Objective

Schedule memory updates while an assistant receives conflicting reports. Each case has 360 chronological events about 20 numbered slots. Six slots are queried. A report is `[slot, value, role, review_epoch, note]`, where `value` is 0–9 and `role` is 0 user, 1 assistant, 2 tool, or 3 delegate. Every note ends with the literal public marker `Public review marker: cross-checked.` or `Public review marker: provisional.` Event indices are zero-based. The case supplies all events in advance: this is an offline schedule, not an online prediction task.

After events 0–89, the initial memory for each queried slot is its latest reported value and that report's index. Every queried slot has a report in this initial prefix. Subsequent reports enter the log but **do not change stored memory by themselves**. The three checkpoints occur after events 0–179, 0–269, and 0–359 respectively. At each checkpoint, execute either one chosen hook or no hook; then record the six stored values and six evidence indices. A case's public `budget` is 1, 2, or 3. Exactly `budget` checkpoints must execute a hook. Any other checkpoint uses `-1` to skip.

There are 12 shuffled, case-local candidates `[kind,slot]`: one `refresh` and one `merge` for each queried slot. A candidate can execute at any checkpoint, using only reports up to that checkpoint. Across the entire three-checkpoint schedule, executed hooks must address different slots; therefore no selected hook overwrites an earlier selected slot. Every executed hook changes its slot's stored value from that slot's initial memory value.

- `refresh` writes the value and event index of the slot's latest `cross-checked` report in the available prefix.
- `merge` examines the slot's last nine reports in that prefix. It writes the most frequent value and the latest event index reporting that value. If frequencies tie, choose the value whose latest report has the greatest index.

By the first checkpoint, every queried slot has at least nine reports, a `cross-checked` report, and a `cross-checked` user report. For every queried slot at every checkpoint, the public `goals_by_phase` target is the value of its latest `cross-checked` user report in that prefix. Goals are computed from the event log and do not depend on the schedule. Test rows include these goals. Training rows additionally show one optimal schedule and its replayed values and evidence; a tied optimal schedule is equally valid.

## Submission

Write `./working/submission.csv` with exactly `case_id,program_json,answers_json,evidence_json` and one row per test case. `program_json` is a JSON list of three integers, one per checkpoint: `-1` for skip or a candidate index 0–11. It must contain exactly `budget` nonnegative entries, and their candidates' slots must be distinct. Indices are executed in checkpoint order; they are **not sorted**.

`answers_json` and `evidence_json` are JSON lists of three lists of six integers. The outer order is checkpoint order; each inner list follows `query` order and describes stored memory **after** that checkpoint's chosen hook or skip. Values must be -1–9 and evidence indices -1–359. An invalid evaluated row scores zero. Missing or duplicate evaluated IDs, missing or extra columns, or an unreadable table score zero for the submission. Extra non-evaluated IDs are ignored; row order does not matter.

## Evaluation

For each legal schedule `p`, replay all three checkpoints from public events. Its utility `u(p)` is the fraction of its 18 stored values that equal the corresponding public `goals_by_phase` entries. Let `h` be the greatest utility among **all legal schedules for that case's budget**. The action component is `A=u(p)/h` when `h>0`, and `A=1` when `h=0`. Every schedule attaining `h`, including any tie that differs from the training oracle, gets `A=1`. The released cases all have `h>0`; the zero rule defines the metric for any valid empty-signal case.

Let `R` be the fraction of the 18 submitted values equal to the exact replay of the submitted schedule. Let `E` be the fraction of the 18 submitted evidence indices equal to that replay. The case score is `A*R*E`; the final score is the arithmetic mean over evaluated test cases. Every term lies in [0,1]. Thus the chosen schedule, its three intermediate states, and their provenance each affect the result. All targets and replay rules are public and deterministic; private grading records only duplicate these public-computable facts.

## Compute and use

The configured tier is one A10G GPU, 64 GiB RAM, and 60 minutes; CPU solutions are allowed. Use only the public archive. Network access and organizer-only answers are prohibited. IDs, file order, hashes, and cross-case test statistics may not be used as answer keys. The episodes are creator-owned synthetic data, not measured production assistant behavior.
