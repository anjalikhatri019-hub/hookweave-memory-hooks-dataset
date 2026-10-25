# HOOKWEAVE — Ordered Memory Hook Replay

This repository is the participant-safe source page for the HOOKWEAVE synthetic AI-agent memory-hook dataset. The v3.0.1 release asset `dataset_public.zip` contains 4,000 labeled training episodes and 1,000 test episodes with public `goal_values`, with 360 events per episode. Solvers choose two ordered memory hooks and compute the resulting values and source-event indices from public events. Pair utility is the fraction of public goals matched; hook kinds have no separate scoring penalty. `restore` uses its listed slot as a key for the most recent capture.

The dataset and documentation are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Suggested attribution: "HOOKWEAVE — Ordered Memory Hook Replay, HOOKWEAVE dataset creators (2026), CC BY 4.0." This is synthetic data and does not represent real conversations or measured production assistant behavior.

The public release contains no private test answers, generator seed, or organizer replay code. See `DATASET_DESCRIPTION.md` for schemas and `PROBLEM_DESCRIPTION.md` for the competition contract. The source archive is the release asset, not GitHub's automatically generated repository source ZIP.
