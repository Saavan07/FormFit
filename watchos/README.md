# FormFit watchOS + iPhone App

This folder contains the Apple Watch collection app and the paired iPhone companion app used for FormFit’s motion-capture workflow.

## What This Module Does

- collects fused motion data on Apple Watch
- lets users control collection sessions from the watch
- transfers saved sessions from the watch to the iPhone
- stores imported sessions inside the phone app for downstream review and scoring
- provides the themed phone UI foundation for future AI coaching and in-app scoring

## Key Components

### Watch app

Located in:

- `watchos/FormFitWatch/FormFitWatch Watch App/`

Responsibilities:

- motion capture via `CMMotionManager`
- collection controls
- watch-side CSV save flow
- watch-to-phone transfer

### Phone app

Located in:

- `watchos/FormFitWatch/FormFitPhone/`

Responsibilities:

- receive transferred files from the watch
- preserve imported workout data
- store sessions in app-owned data structures
- present the modernized FormFit UI

## Build

Open:

`watchos/FormFitWatch/FormFitWatch.xcodeproj`

Recommended flow:

1. Select the `FormFitPhone` scheme.
2. Use a paired iPhone + Apple Watch destination.
3. Build and run from Xcode.

## Expected Data Flow

1. Start a workout collection on the watch.
2. Record motion samples during the exercise.
3. Save the session on the watch.
4. Transfer the session to the paired phone.
5. Persist the raw file and import the session into the app for future analysis.

## Notes

- Real sensor collection requires a physical Apple Watch.
- The app is structured to support future in-app scoring, but the ML training/export pipeline itself lives in the top-level `ml/` folder.
