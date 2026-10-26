# HOOKWEAVE — Checkpointed Memory Hook Scheduling

This is the participant-safe source page for the HOOKWEAVE v5.0.1 documentation update. Its `dataset_public.zip` asset is byte-identical to v5.0.0 and contains 4,000 training episodes and 1,000 test episodes, each with 360 events. Solvers schedule a public budget of one to three hooks across three checkpoints and report the stored values and evidence after each checkpoint. Six queried slots and 12 shuffled candidates make execution time and partial trajectories matter. All checkpoint goals are public; the training oracle is one example of an optimal schedule. A legal schedule may select each slot only once; a repeated-slot evaluated row scores zero.

The dataset and dataset documentation are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Suggested attribution: "HOOKWEAVE — Checkpointed Memory Hook Scheduling, HOOKWEAVE dataset creators (2026), CC BY 4.0." The cases are generated, not measured conversations or production assistant activity.

The release asset contains no private test answer records or organizer code. See `DATASET_DESCRIPTION.md` for inventory and limitations and `PROBLEM_DESCRIPTION.md` for the task. Download `dataset_public.zip` from the release assets, rather than GitHub's automatically generated repository source ZIP.
