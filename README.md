# Garmin Raw Sensor Stream

High-frequency raw sensor data capture from Garmin watches with live  
streaming to Android for storage and analysis.

---

## Overview

Garmin watches contain powerful sensors (accelerometer, gyroscope,  
optical heart rate, GPS, barometer) but official apps only expose  
aggregated data. This project streams raw, high-frequency signals  
directly to an Android phone for research and advanced analysis.

The system is designed to be **modular and extensible**, allowing integration of  
additional external raw sensors (IMU, AHRS, etc.) for synchronized multi-source acquisition.

---

## Architecture

- **Watch app** (Connect IQ / Monkey C)  
  Reads sensors at maximum rate, packages samples, sends over BLE.

- **Android app** (Kotlin)  
  Receives streams, timestamps data, writes to storage (CSV / Parquet / SQLite), optional live monitoring.

- **Protocol**  
  BLE GATT with custom service for sensor packets.

- **External sensors (optional)**  
  Additional raw sensor streams via BLE, USB (OTG), or Serial (UART).

---

## Features

- Raw accelerometer / gyroscope / HR streaming  
- Configurable sampling rate  
- Reconnection handling and buffering  
- Local storage with export  
- Multi-sensor extensibility (Garmin + external IMUs)  

---

## Supported Sensors

### Garmin (native)

- Accelerometer  
- Gyroscope  
- Optical Heart Rate  
- GPS  
- Barometer  

---

### External Raw Sensors (extensible)

The architecture supports integration of additional raw sensors.  
The following models are identified for future support:

#### ATO Series
- ATO-IMU-100BL  
- ATO-IMU-TL725D  
- ATO-IMU-570  
- ATO-IMU-AH200C  

#### WT Series
- WT9011DCL  
- WTGAHRS3  

---

## External Sensor Integration Guide

To integrate a new sensor, implement the following components.

---

### 1. Communication Interface

Define transport layer:

- BLE (recommended)  
- USB (Android OTG)  
- UART / Serial  

---

### 2. Data Normalization

All sensors must map to a unified structure:

```
timestamp, source, sensor_id, type, values...
```

Examples:

- IMU  
```
accel_x, accel_y, accel_z, gyro_x, gyro_y, gyro_z
```

- AHRS  
```
roll, pitch, yaw
```

- Barometer  
```
pressure, altitude
```

---

### 3. Timestamp Strategy

- Prefer **sensor-native timestamps** when available  
- Otherwise use **Android reception timestamp**  
- Maintain consistency across all sources  

---

### 4. Sampling Rate Handling

- Detect native sampling frequency  
- Store raw frequency without forced downsampling  
- Optional post-processing:
  - resampling  
  - interpolation  

---

### 5. Sensor Identification

Each sensor must define:

```
sensor_id   -> unique identifier
sensor_type -> imu / ahrs / hr / gps / etc.
model       -> device model name
```

---

### 6. Calibration Layer

Support per-sensor calibration:

- Offset correction  
- Scale normalization  
- Axis alignment  

Calibration should be configurable and versioned.

---

## Unified Data Model

All streams (Garmin + external) converge into a common schema:

| Field       | Description                         |
|------------|-------------------------------------|
| timestamp  | Epoch (ms or ns)                    |
| source     | garmin / external                   |
| sensor_id  | Unique identifier                   |
| type       | accel / gyro / hr / ahrs / etc.     |
| values     | Sensor-specific payload             |

---

## Extending with New Sensors

To add a new sensor:

1. Implement communication driver (BLE / USB / Serial)  
2. Parse raw packets  
3. Map fields to unified schema  
4. Register sensor metadata (ID, model, type)  
5. Add optional calibration profile  

---

## Roadmap

- [ ] Connect IQ prototype (accelerometer @ 25 Hz)  
- [ ] BLE transport layer  
- [ ] Android receiver app (Kotlin)  
- [ ] Multi-sensor support  
- [ ] External IMU integration (ATO / WT series)  
- [ ] Data fusion pipeline  
- [ ] Export (CSV, Parquet, SQLite)  

---

## Status

🚧 Work in progress

---

## License

MIT
