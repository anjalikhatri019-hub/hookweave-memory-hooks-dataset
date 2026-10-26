# HOOKWEAVE checkpoint scheduling dataset

## Purpose and intended use

This creator-owned synthetic corpus supports research and teaching about resource-limited AI-agent memory updates. A solver schedules a small number of refresh/merge hooks across three checkpoints, then reports the stored memory and supporting event indices after each checkpoint. It evaluates an intervention trajectory: later reports do not update stored memory unless the schedule executes a hook. It is not a dataset of real conversations and does not establish that a hook policy is safe for deployment.

## Data, targets, and split

The public archive contains 5,000 independent cases: 4,000 training rows and 1,000 test rows. Each case has 360 chronological reports about 20 numbered slots, six queried slots, 12 shuffled hook candidates (one refresh and one merge per queried slot), a budget of 1, 2, or 3 actions, and three checkpoint goal arrays. A report is `[slot,value,source_role,review_epoch,note]`; value is 0–9; roles 0/1/2/3 mean user/assistant/tool/delegate. Each note ends with a public cross-checked or provisional marker. Cases use independent generation randomness; train and test do not share event sequences or case IDs. Both splits use the same synthetic mechanism, so the split does not prove transfer to another generator.

The first 90 reports initialize stored memory from each queried slot's latest report. The three later prefixes end after reports 180, 270, and 360. Reports arriving after initialization enter the log but do not themselves mutate stored memory. At each checkpoint, a selected refresh uses the latest cross-checked report in the prefix; a selected merge uses the plurality of the slot's last nine prefix reports, breaking a tie by the most recent supporting report. One hook or skip is allowed at each checkpoint; exactly the case's budget actions must run, and selected actions must address distinct slots. The generator ensures any selected candidate changes its slot's initial stored value at any checkpoint.

`goals_by_phase` is present in **both training and test rows**. Every queried slot has a report in the first 90 events and at least nine reports, a cross-checked report, and a cross-checked user report by the first checkpoint. For each queried slot and checkpoint prefix, its target is the value of the latest cross-checked user report. This target is public and unaffected by the selected schedule. Training additionally provides one maximizing `oracle_program`, `oracle_answers`, and `oracle_evidence`; test omits only these oracle fields. A tied maximizer is equally valid. The grader's private payload duplicates public-computable replay data and the maximum utility; it is an implementation aid, not a secret target.

The scheduled output is three rows of six stored values and three rows of six evidence indices, one row after each checkpoint. Utility is the fraction of 18 stored values matching public checkpoint goals. If `h` is the maximum utility for a legal schedule with that case's budget, action quality is `A=u/h` for h>0 and 1 for h=0. Replay value and evidence agreement each use 18 cells. The case score is `A*R*E` and the final score is the mean over test cases. All legal schedules and their scores can be computed from the public test row; the problem is trajectory planning and exact provenance replay, not hidden-label inference.

## Archive inventory and preparation

The participant archive `dataset_public.zip` is 292,938,902 bytes and contains exactly:

- `train.jsonl`: 4,000 cases with public fields and one oracle schedule/replay.
- `test.jsonl`: 1,000 cases with public fields and no oracle schedule/replay.
- `manifest.json`: version 5.0.0, train/test counts, 360 events, 20 slots, six queried slots, checkpoints `[180,270,360]`, 90-event initialization, 12 candidates, budgets `[1,2,3]`, synthetic provenance, and `CC-BY-4.0` license identifier. It contains no case labels or generation seed.
- `LICENSE_DATA.md`: the CC BY 4.0 notice, attribution guidance, source link request, change disclosure request, and a statement that code and organizer-only answer records are outside the data-license grant.

The platform organizer input `raw_dataset.zip` contains the participant archive and `private_answers.jsonl`. Prepare verifies pinned hashes and archive inventory, extracts the four participant files to the public directory, emits a public `sample_submission.csv`, and writes private `answers.csv`. The public source release contains only `dataset_public.zip`; the raw organizer archive and private answers must remain private.

## Known limitations

Every report and goal is synthetic. The notes use a small template family and deliberately structured conflict patterns; neither conversational language variety nor human intent uncertainty is represented. Goals are public, and an exact solver can enumerate 36, 360, or 960 legal schedules depending on budget. Dataset size and the number of events do not by themselves establish Hard difficulty. The episodes are planned offline with full knowledge of the event stream, rather than decisions made under live uncertainty. The benchmark does not test consent, privacy rights, deletion requests, adversarial instructions, multi-agent safety, or real system failures. Results should not be interpreted as measured assistant performance in production.

## Source and license

Dataset Source URL: https://github.com/anjalikhatri019-hub/hookweave-memory-hooks-dataset/releases/tag/v5.0.0

Participant dataset download: https://github.com/anjalikhatri019-hub/hookweave-memory-hooks-dataset/releases/download/v5.0.0/dataset_public.zip

The generated data and dataset documentation are offered under Creative Commons Attribution 4.0 International (CC BY 4.0): https://creativecommons.org/licenses/by/4.0/ . Suggested attribution: "HOOKWEAVE — Checkpointed Memory Hook Scheduling, HOOKWEAVE dataset creators (2026), CC BY 4.0." No third-party conversation text or personal data was copied.
