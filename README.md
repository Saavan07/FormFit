# FormFit

FormFit is a watch-first rehabilitation and form-feedback platform built around Apple Watch motion capture, an iPhone companion app, and a lightweight machine learning pipeline for movement-quality scoring.

This repository has been updated from the original watch-only collector into a fuller Spring 2026 snapshot of the project: branded phone and watch UI, end-to-end watch-to-phone session transfer, app-side session storage, and the PyTorch-to-Core ML model pipeline used to prepare future in-app scoring.

## Highlights

- End-to-end Apple Watch motion collection using `CMDeviceMotion`
- iPhone companion app for receiving transferred sessions and storing them in-app
- Branded phone and watch UI with synchronized visual language and app icons
- Data pipeline for collecting, labeling, splitting, and training movement data
- 1D CNN training workflow with Core ML export included in the repo
- EOH 2026: FormFit earned **2nd Place in Most Industry Impact** after presenting to **30,000+ attendees**

## System Overview

FormFit currently spans three connected layers:

1. **Watch collection**
   The Apple Watch records user acceleration, gyroscope readings, and roll/pitch/yaw orientation data during rehab or strength movements.

2. **Phone experience**
   The iPhone app receives session files from the watch, preserves the raw CSV data, and imports sessions into app-owned storage so the data is ready for analysis and future scoring flows.

3. **ML pipeline**
   The `ml/` section contains the training code, labels, split rep files, trained weights, training curve, and exported Core ML package used to support movement-quality scoring.

## Repository Structure

```text
.
├── ml/
│   ├── CNN.py
│   ├── FormFitModel.mlpackage/
│   ├── best_model.pth
│   ├── formfit-data/
│   ├── formfit-labels.csv
│   ├── training_history.png
│   ├── README.md
│   └── requirements.txt
├── tools/
│   └── generate_formfit_icons.swift
└── watchos/
    ├── FormFitWatch/
    │   ├── FormFitPhone/
    │   ├── FormFitWatch Watch App/
    │   └── FormFitWatch.xcodeproj
    └── README.md
```

## What Is Included

### Apple Watch + iPhone app

- Watch motion capture flow with start/stop/save controls
- WatchConnectivity transfer from watch to phone
- iPhone-side import and persistence of workout sessions
- UI work for home, workout, progress, profile, tutorial, and leaderboard screens
- Themed watch and phone styling plus branded FormFit app icons
- Foundations for future coaching and scoring workflows

### Machine learning workflow

- Rep-window training data under `ml/formfit-data/`
- Label file under `ml/formfit-labels.csv`
- PyTorch CNN training and scoring script in `ml/CNN.py`
- Trained checkpoint in `ml/best_model.pth`
- Exported Core ML package in `ml/FormFitModel.mlpackage`
- Saved training curve in `ml/training_history.png`

## Running The App

Open the Xcode project at:

`watchos/FormFitWatch/FormFitWatch.xcodeproj`

Then:

1. Select the `FormFitPhone` scheme if you want the paired iPhone app flow.
2. Select the watch app scheme if you want to work directly on watch collection.
3. Run on a real iPhone + Apple Watch pair for sensor collection and transfer testing.

More detailed notes live in [watchos/README.md](watchos/README.md).

## Running The ML Pipeline

From the `ml/` directory:

```bash
python3 -m pip install -r requirements.txt
python3 CNN.py
```

That script:

- loads split rep CSV files
- trains the 1D CNN
- saves the best weights
- writes a training history plot
- evaluates validation accuracy
- exports the model as a Core ML package

More details live in [ml/README.md](ml/README.md).

## Current Status

This repo reflects a strong prototype milestone:

- collection is working
- watch-to-phone transfer is working
- app-side storage is working
- the UI is far beyond the original blank collector prototype
- the ML training/export pipeline is present

The next major layer is tightening the connection between imported sessions and live in-app scoring / coaching.

## Acknowledgment

FormFit was built through months of iteration across sensing, UI, data engineering, and model experimentation. This repository is meant to present that work as a cohesive system instead of only the earliest collector prototype.
