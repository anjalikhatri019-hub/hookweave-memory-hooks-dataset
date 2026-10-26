# v5.0.1 — slot-legality documentation correction

This patch clarifies that each slot may be selected at most once in a three-checkpoint schedule. Selecting two candidates for the same slot, even at different checkpoints, is an invalid evaluated row and scores zero. The challenge description no longer uses an initial-value change as an explanation for that rule. Dataset records, private answers, Prepare, Grade, metric, and participant ZIP bytes are unchanged from v5.0.0.

The participant asset is 292,938,902 bytes, SHA-256 `1bf211ebf5183e922e8aefe3fd835cc5d5bdd49bfbfd47953ae9076322d0968b`, under CC BY 4.0. The original v5.0.0 release remains available.
