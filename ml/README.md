# FormFit ML Pipeline

This folder contains the training and export pipeline for the current FormFit movement-quality model work.

## Contents

- `CNN.py`
  Trains a 1D convolutional network on rep-window motion data, evaluates it, prints sample feedback, and exports a Core ML package.

- `formfit-data/`
  Split repetition CSV files used for the current training workflow.

- `formfit-labels.csv`
  Labels aligned to the split rep sessions.

- `best_model.pth`
  Saved PyTorch checkpoint from training.

- `training_history.png`
  Loss curve generated from a training run.

- `FormFitModel.mlpackage/`
  Exported Core ML package for downstream Apple-platform integration.

## Install

```bash
python3 -m pip install -r requirements.txt
```

## Train + Export

```bash
python3 CNN.py
```

The script is configured to:

- load data relative to its own directory
- save the best PyTorch weights locally
- save the training curve as an image
- export the Core ML package locally

## Model Inputs

Each rep window is built from nine motion channels:

- `ax`, `ay`, `az`
- `gx`, `gy`, `gz`
- `roll`, `pitch`, `yaw`

## Current Scope

The current model focuses on three movement-quality signals:

- elbow hiking
- shoulder hiking
- torso twist

The pipeline also folds eccentric timing into the reported feedback score during evaluation.
