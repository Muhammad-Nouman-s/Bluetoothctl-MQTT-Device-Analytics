# Bluetoothctl MQTT Device Analytics

Notebooks for offline analysis of Bluetooth Low Energy (BLE) scan data. The scans are recorded
with `bluetoothctl` on a gateway, sent over MQTT and exported to CSV. The notebooks clean the raw
logs and turn them into per-device timelines, presence sessions, iBeacon identifiers and
RSSI-based proximity and movement estimates.

Beeta is the name of the MQTT topic/environment used during data collection.

## Notebooks

- `bluetooth_ble_mqtt_device_analytics.ipynb`: the basic pipeline. Loads the MQTT export, parses
  the JSON payloads, cleans the `bluetoothctl` output and extracts MAC addresses, NEW/CHG/DEL
  events, iBeacon tuples and event rates.
- `BLE_Device_Behavior_Tracking.ipynb`: the behaviour analysis. Adds session-based presence,
  RSSI proximity classes, motion state, plots and a Tkinter GUI (iBeacon Session Inspector) to
  browse devices and sessions.

## What the analysis does

- Parses the JSON `payload` field of each MQTT message and converts timestamps to UTC.
- Removes ANSI colour codes from the `bluetoothctl` output and normalises MAC addresses.
- Extracts NEW, CHG and DEL device events and rebuilds the event history of each device,
  keeping controller (gateway) events separate.
- Decodes iBeacon advertisements into UUID, Major and Minor.
- Splits each device's observations into sessions (default gap: 180 s) and computes first seen,
  last seen, number of sessions, total time present and gaps between sessions.
- Reads RSSI values and assigns a proximity class (NEAR, MID, FAR) using fixed thresholds.
- Estimates whether a device is STATIC or MOVING from the spread of its RSSI (median absolute
  deviation over a rolling window).
- Counts events in time bins and plots RSSI over time, proximity, movement, RSSI PDF/CDF and
  event counts.
- Exports figures (PDF, PNG, JPG) and tables (CSV, XLSX) from the GUI.

## Input data

One row per MQTT message, with these columns:

| Column | Content |
|---|---|
| `timestamp_client` | time recorded by the MQTT client |
| `topic` | MQTT topic |
| `payload` | JSON string with `timestamp` (gateway log time) and `revelations` (raw `bluetoothctl` output) |

The raw data contains real device MAC addresses, so it is not part of this repository. Put your
own export in `data/mqtt_input.csv` (the folder is ignored by git) or change `DEFAULT_CSV` in the
first cell. Set `GATEWAY_MAC` there as well.

## Setup

```bash
pip install -r requirements.txt
jupyter notebook
```

The GUI needs Tkinter and a desktop session, so it will not open on Google Colab or a headless
server. On Ubuntu, Tkinter can be installed with `sudo apt install python3-tk`.

## Author

Muhammad Nouman, PhD researcher at the Telematics Lab, Politecnico di Bari.
[Website](https://muhammad-nouman-s.github.io/)

Released under the [MIT License](LICENSE).
