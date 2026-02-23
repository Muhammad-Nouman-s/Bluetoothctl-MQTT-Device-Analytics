# Bluetoothctl MQTT Device Analytics
This repository contains a Jupyter Notebook and a Tkinter-based application for analyzing Bluetooth Low Energy (BLE) scan data collected via bluetoothctl and transmitted through MQTT. The workflow processes MQTT-exported CSV data, extracts device-level and controller-level information from raw scan logs, and provides structured summaries and visualizations for offline BLE data analysis.
 
## Functional Overview
The analysis performs the following operations:

1. Loads MQTT-exported CSV files and parses embedded JSON payload fields.
2. Standardizes MAC address formatting and converts timestamps to a UTC time base.
3. Cleans raw bluetoothctl scan output by removing ANSI escape sequences and control characters.
4. Normalizes and extracts device-level event tags (NEW, CHG, DEL) from log text.
5. Extracts BLE MAC addresses and reconstructs per-device event sequences.
6. Identifies iBeacon advertisement tuples and extracts:
   - UUID
   - Major
   - Minor
11. Computes session-based presence metrics per device, including:
    - First observed timestamp
    - Last observed timestamp
    - Number of sessions
    - Total present duration
    - Inter-session gap duration
13. Extracts RSSI values from log blocks and assigns proximity classes based on predefined signal thresholds.
14. Estimates device motion state (STATIC or MOVING) using Median Absolute Deviation (MAD)–based dispersion measures and rolling-window statistics.
15. Aggregates NEW, CHG, and DEL events into configurable time bins.
16. Generates visualizations of RSSI trends, proximity over time, movement indicators, empirical RSSI distributions (PDF and CDF), and binned event counts.
17. Provides export functionality for generated figures (PDF, PNG, JPG) and associated tabular data (CSV, XLSX).

## Dataset Note
**Beeta** refers to the MQTT topic/environment name used during data collection.
