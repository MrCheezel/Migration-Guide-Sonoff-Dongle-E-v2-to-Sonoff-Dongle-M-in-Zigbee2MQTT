Zigbee2MQTT Migration Guide
Sonoff Dongle-E v2 → Sonoff Dongle-M (EFR32MG24 / Ember)

No device re-pairing required

🧰 What You Will Need
Hardware
·Raspberry Pi 4 running Home Assistant
·Sonoff Zigbee Dongle‑E v2 (current coordinator)
·Sonoff Zigbee Dongle‑M (new coordinator)
·USB extension cable (strongly recommended to reduce interference)

Software
·Home Assistant (latest)
·Zigbee2MQTT (latest)

⚠️ Critical Rules (Read Before Starting)
·❌ Do NOT delete Zigbee2MQTT
·❌ Do NOT create a new Zigbee network
·❌ Do NOT re-pair any devices
·❌ Do NOT change PAN ID, Extended PAN ID, network key, or channel
·✅ We are only replacing the coordinator hardware
If these rules are followed, all devices will automatically reconnect.
