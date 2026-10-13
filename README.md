# FormFit

An Apple Watch and iPhone exercise-feedback prototype combining motion collection, session transfer, repetition analysis, and an experimental Core ML scoring model.

This repository began as Saavan Kiran's watch data collector. It now brings together the team's later app, the Python training/export pipeline, and the standalone repetition-segmentation module. It is a collaborative prototype, not a validated medical or rehabilitation product.

## What is implemented

- **Watch collection:** Core Motion acceleration, gyroscope, and orientation channels; collection controls and CSV saving.
- **Watch-to-phone transfer:** WatchConnectivity session commands and file transfer, with received files imported into phone-owned storage.
- **Phone app:** SwiftUI home, workout, progress, profile, tutorial, and leaderboard views; SwiftData persistence for imported sessions and repetitions.
- **Analysis:** CSV parsing, repetition splitting, range-of-motion and timing calculations, and workout graphs.
- **Experimental scoring:** the phone app's `FormFitInference` loads the bundled Core ML model, evaluates repetition windows, and stores model outputs. It falls back to a range-of-motion-derived score when inference is unavailable.
- **Python work:** a PyTorch CNN training/export pipeline in `ml/` and a separate tested segmentation module in `signal-processing/`.

The segmentation module is not yet wired into the Swift app. Leaderboard and profile screens do not establish a deployed multiplayer service. Some views can use seeded demonstration workouts; those scores are not evidence of model accuracy.

## Architecture

```text
Apple Watch / Core Motion
  -> CSV session files + WatchConnectivity
  -> iPhone import / repetition analysis
  -> SwiftData session and sample storage
  -> experimental Core ML inference + SwiftUI review

Python motion data / labels -> PyTorch -> Core ML package
Separate Python segmentation module -> synthetic regression tests
```

## Build and try the app

Open `watchos/FormFitWatch/FormFitWatch.xcodeproj` in Xcode and select the `FormFitPhone` scheme. The project targets iOS 18.6 and watchOS 11.6 or later.

The app and embedded watch target built successfully for the iOS Simulator with Xcode 26.1.1 on October 13, 2026:

```bash
xcodebuild -project watchos/FormFitWatch/FormFitWatch.xcodeproj \
  -scheme FormFitPhone -destination 'generic/platform=iOS Simulator' \
  -derivedDataPath /tmp/formfit-build CODE_SIGNING_ALLOWED=NO build
```

For physical-device collection, configure your own signing team in Xcode and use a paired iPhone and Apple Watch. Start a session, record movement, save it, transfer the CSV, and inspect the imported workout. The October verification did not include a fresh physical-device recording or sensor-transfer test.

## Python components

Run the existing segmentation tests:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install pytest
python -m pytest -q signal-processing
```

The `ml/` directory contains training code, data, labels, a checkpoint, and an exported model. Its README describes the workflow. Training writes model artifacts, so run it in a separate experiment checkout if you want to preserve the included snapshot. Training and accuracy evaluation were not rerun for this update.

## Contributions

FormFit was developed with the [muvaro2/FormFit team](https://github.com/muvaro2/FormFit). Saavan's recorded work includes the watch collector, collection timing and controls, watch/phone connectivity scaffolding, phone UI integration, data visualization integration, and Core ML pipeline integration. Teammate work includes model training, repetition processing, phone UI, and inference integration. These areas were collaborative; this list does not assert exclusive authorship.

Examples from the preserved team history:

- Saavan: `3abc4e8` (watch/phone connectivity), `9b216b6` (collector controls), `cfc47db` (phone UI and watch controls), `9ace180` (data visualization), `4bfe97a` (Core ML pipeline).
- `yuktha`: CNN model/training/export work, including `b5a3e14`, `0a56d94`, and `fa51ec0`.
- Roshan Gunuganti: later UI and inference/model work on `codex-formfit-ui-watch-sync`, ending at `f1d5ed9`.
- `Ithomas13`: repetition-processing work, including the September module and tests at `1bd8ad0`.
- Additional contributors are retained in the original branch history. Exact shared and individual responsibilities will be refined with the team.

## Repository history

The original personal collector commits are preserved. The October 2026 consolidation retains team histories as merge ancestry, restores the fuller app from `codex-formfit-ui-watch-sync`, retains the portable personal `ml/` snapshot, and copies the September segmentation module without source changes. Other development branches are preserved under `team-history/` for reference. This is a documented assembly of existing work, not a claim that all branches were previously integrated.

See [provenance](docs/provenance.md) and [verification](docs/verification.md).

## Limitations

The model and feedback are experimental. A successful build does not prove scoring accuracy, clinical validity, or dependable watch transfer. Motion preprocessing and the fallback score need evaluation on new recordings. The training loader uses overlapping windows and a random window split, which can mix windows from the same recording between train and validation sets. Do not interpret that validation alone as generalization to new people or sessions.

No new blanket license is assigned by this update. Retain contributor and third-party notices.
