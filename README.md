# FormFit Watch - Core Motion Data Collector

A minimal watchOS app that records **fused motion** from `CMDeviceMotion` on Apple Watch and saves it to CSV for later analysis.

## Features

- Adjustable sampling tick, default **50 ms**, range **10 to 200 ms**
- Rolling buffer with configurable max size (default ~5k rows)
- Live readout for accel, gyro, and attitude
- One tap **Save CSV** to the app Documents directory

## Why

I needed a simple, reliable way to capture watch motion for prototyping form tracking and model training, without wiring the phone or server yet.

## How it works

- Uses `CMMotionManager` **deviceMotion** stream
- Captures the following per sample:
  - Linear acceleration `userAcceleration.{x,y,z}` in m/s²
  - Rotation rate `rotationRate.{x,y,z}` in rad/s
  - Orientation `attitude.{roll,pitch,yaw}` in radians
- Keeps:
  - `latest` for quick UI display
  - `buffer` for the time series, capped at `maxBuffer`

### CSV format

Header:
# FormFit Watch - Core Motion Data Collector

A minimal watchOS app that records **fused motion** from `CMDeviceMotion` on Apple Watch and saves it to CSV for later analysis.

## Features

- Adjustable sampling tick, default **50 ms**, range **10 to 200 ms**
- Rolling buffer with configurable max size (default ~5k rows)
- Live readout for accel, gyro, and attitude
- One tap **Save CSV** to the app Documents directory

## Why

I needed a simple, reliable way to capture watch motion for prototyping form tracking and model training, without wiring the phone or server yet.

## How it works

- Uses `CMMotionManager` **deviceMotion** stream
- Captures the following per sample:
  - Linear acceleration `userAcceleration.{x,y,z}` in m/s²
  - Rotation rate `rotationRate.{x,y,z}` in rad/s
  - Orientation `attitude.{roll,pitch,yaw}` in radians
- Keeps:
  - `latest` for quick UI display
  - `buffer` for the time series, capped at `maxBuffer`

### CSV format

Header:
idx,timestamp,ax,ay,az,gx,gy,gz,roll,pitch,yaw

Rows:
- `idx` is a zero based counter within the current buffer
- `timestamp` is Core Motion timestamp in seconds since the boot reference
- Other columns are doubles

File name example:
formfit-YYYYMMDD-HHMMSS-<tickMs>ms.csv

## Build and run

### Requirements

- Apple Watch paired to an iPhone
- Xcode version that matches your watchOS
  - watchOS 10.6.1 pairs well with **Xcode 15.4**
  - watchOS 11 pairs well with **Xcode 16+**

### Steps

1. Open `FormFitWatch.xcodeproj` in Xcode.
2. Select the **watch app** scheme.
3. Choose your **real Apple Watch** as the destination.
4. Run. Grant Motion permission on first launch.

> The watch simulator will not produce real motion values. Use a device for data.

## Configuration

In `MotionManager.swift`:
- `tickMs`: sampling interval in milliseconds
- `maxBuffer`: maximum number of samples to keep before trimming

You can also expose `tickMs` via the UI slider provided.

## UI

- **Start**, **Stop**, **Clear**, **Save CSV**
- **Tick** control with label that shows the current ms
- Optional live metrics block for quick sanity checks

## Roadmap

- Add WatchConnectivity to transfer CSV to iPhone Files
- Add session metadata and calibration markers
- Build a quick visualizer notebook for CSV files
- Move to a ring buffer for O(1) trimming on long sessions

## Notes

- Ensure `NSMotionUsageDescription` exists in the target Info so the permission prompt is clear.
- First install can take a moment while Xcode prepares the device.
