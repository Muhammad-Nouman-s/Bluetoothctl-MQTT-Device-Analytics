# BLE MQTT Beeta Device Analytics 
This repository contains a Jupyter Notebook for analyzing Bluetooth Low Energy (BLE) scan data collected via bluetoothctl and transported through MQTT.
 The notebook processes MQTT-exported CSV files, extracts device-level information, and produces clean, reproducible analytics on BLE device presence and activity. The workflow is suitable for offline analysis of BLE environments.
 
## What This Notebook Does
The notebook performs the following steps:

1. Loads MQTT CSV data and parses nested JSON payloads
2. Cleans raw bluetoothctl scan output, removing ANSI escape sequences and control characters
3. Analyzes controller-level state changes (power, discovery, pairing)
4. Analyzes device-level state changes (NEW, CHG, DEL)
5. Extracts BLE MAC addresses and aggregates device activity across messages
6. Builds per-device event histories from bluetoothctl logs
7. Identifies iBeacon advertisements, extracting:
   - UUID
   - Major
   - Minor
8. Computes presence intervals for each device (first seen, last seen, duration)
9. Calculates the frequency of events for each detected device

## Dataset Note
**Beeta** refers to the MQTT topic/environment name used during data collection and is preserved intentionally.
