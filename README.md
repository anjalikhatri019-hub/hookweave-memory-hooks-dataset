# HOOKWEAVE — Two-Slot Memory Hook Selection

This repository is the participant-safe source page for the HOOKWEAVE synthetic AI-agent memory-hook dataset. The v4.0.0 release asset `dataset_public.zip` contains 4,000 labeled training episodes and 1,000 test episodes with public `goal_values`, with 360 events per episode. Solvers select two hook candidates for different queried slots, then compute the resulting values and source-event indices from public events. Each case has 24 legal pairs. The grader awards full action credit to every pair with maximum public goal-match utility.

The dataset and documentation are licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). Suggested attribution: "HOOKWEAVE — Two-Slot Memory Hook Selection, HOOKWEAVE dataset creators (2026), CC BY 4.0." This is synthetic data and does not represent real conversations or measured production assistant behavior.

The public release contains no private test answers, generator seed, or organizer replay code. See `DATASET_DESCRIPTION.md` for schemas and `PROBLEM_DESCRIPTION.md` for the competition contract. The source archive is the release asset, not GitHub's automatically generated repository source ZIP.
