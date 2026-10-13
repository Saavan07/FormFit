# Source provenance

The October 2026 assembly preserves the existing personal repository history and the original team commit histories.

| Component | Source |
| --- | --- |
| Original collector and April personal snapshot | Saavan07/FormFitWatch-DataCollector, through `e08b4a0` |
| Fuller watch and phone app, graphs, inference | muvaro2/FormFit, `codex-formfit-ui-watch-sync` at `f1d5ed9` |
| Portable training/export snapshot | Existing personal `ml/` directory at `e08b4a0`, derived from the team's PyTorch work |
| Standalone segmentation and tests | muvaro2/FormFit `main` at `1bd8ad0`, copied into `signal-processing/` |
| Other experiments | Original team branches retained under `team-history/` |

The merge records source ancestry while deliberately assembling the component directories above. It does not imply that all historical branch trees are interchangeable or that the September Python module is integrated into the app. Generated Xcode user settings, nested worktree entries, and operating-system metadata are excluded from the assembled default tree.

Saavan confirmed permission to publish the team's work on October 13, 2026. Individual and shared contribution wording remains open to refinement. Author records are preserved rather than reconstructed from file ownership.

Uncommitted model artifacts in Saavan's original personal checkout were left intact. Their training provenance and difference from the committed models have not yet been verified, so they are not silently substituted into the portfolio snapshot.
