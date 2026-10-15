# Somna respiratory dashboard

Open `dist/index.html` in a browser to try the demo, or open the standalone `Somna-Dashboard.html` deliverable. No npm dependencies or build step is required.

## Included
- Simulated 20 Hz chest and abdomen streams, fixed baseline scales, estimated breathing rate and relative amplitude.
- Start/stop recording, browser-local session history, CSV export and JSON backup/import.
- Clickable session timeline and a 30-second two-band waveform inspector.
- Configurable Web Bluetooth service and notification UUIDs, independent pairing and disconnect gaps.

## Hardware contract
Each ESP32 sends one complete UTF-8 JSON notification: `{"t":12500,"seq":250,"value":1048576}`. `t` is monotonic milliseconds since device boot, `seq` increases once per sample, and `value` is the raw sensor value. Target 20 Hz. The firmware must match the UUIDs shown on Devices & setup. Use HTTPS or localhost in a Web Bluetooth browser. The adapter has not been tested against physical hardware. Notification JSON requires a sufficient negotiated BLE MTU; alternatively adapt the decoder to a compact binary packet once firmware is defined.

The first received sample establishes an approximate device-to-browser time offset. Accurate inter-band phase measurement requires clock synchronization in firmware. A reconnected device resets its decoder state. Calibrate after at least 5 seconds of good signal. Baselines depend on belt placement and tension; do not interpret percent movement as calibrated volume or muscle strength.

## Recording limits
This is a browser prototype, not a reliable unattended overnight collector. Keep the page open and device awake. Screen wake-lock is requested when supported but cannot guarantee background recording. An eventual phone companion app is needed for dependable screen-locked collection.

Sessions are stored in browser localStorage at stop, subject to browser quota. Long sessions can exceed this quota; a warning asks you to export the still-in-memory session. Closing the page during recording loses unsaved samples. Full-night production use needs incremental IndexedDB or native phone storage, and recovery from interruptions. No cloud storage or accounts are implemented. Demo and hardware data are explicitly distinguished.

## Validation
JavaScript syntax check and a Node DOM-mocked functional check passed for 15 bpm simulation, recording, two-channel sample capture, persistence, review, and disconnected hardware recording prevention. Visual browser QA was blocked by this environment; physical Bluetooth and overnight endurance are unverified.
