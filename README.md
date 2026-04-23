# Garmin Raw Sensor Stream

High-frequency raw sensor data capture from Garmin watches with live 
streaming to Android for storage and analysis.

## Overview

Garmin watches contain powerful sensors (accelerometer, gyroscope, 
optical heart rate, GPS, barometer) but official apps only expose 
aggregated data. This project streams raw, high-frequency signals 
directly to an Android phone for research and advanced analysis.

## Architecture

- **Watch app** (Connect IQ / Monkey C): reads sensors at max rate, 
  packages samples, sends over BLE.
- **Android app** (Kotlin): receives, timestamps, writes to local 
  storage (CSV / Parquet / SQLite), optional live monitoring.
- **Protocol**: BLE GATT with custom service for sensor packets.

## Features

- Raw accelerometer / gyroscope / HR streaming
- Configurable sampling rate
- Reconnection handling and on-watch buffering
- Local storage on phone with export

## Status

🚧 Work in progress

## Roadmap

- [ ] Connect IQ prototype reading accelerometer at 25 Hz
- [ ] BLE transport layer
- [ ] Android receiver app (Kotlin)
- [ ] Multi-sensor support
- [ ] Data export (CSV, Parquet)

## License

MIT
