Zigbee2MQTT Migration Guide
Sonoff Dongle-E v2 → Sonoff Dongle-M (EFR32MG24 / Ember)
No device re-pairing required
🧰 What You Will Need
Hardware
·	Raspberry Pi 4 running Home Assistant
·	Sonoff Zigbee Dongle‑E v2 (current coordinator)
·	Sonoff Zigbee Dongle‑M (new coordinator)
·	USB extension cable (strongly recommended to reduce interference)
Software
·	Home Assistant (latest)
·	Zigbee2MQTT (latest)
⚠️ Critical Rules (Read Before Starting)
·	❌ Do NOT delete Zigbee2MQTT
·	❌ Do NOT create a new Zigbee network
·	❌ Do NOT re-pair any devices
·	❌ Do NOT change PAN ID, Extended PAN ID, network key, or channel
·	✅ We are only replacing the coordinator hardware
If these rules are followed, all devices will automatically reconnect.
Step 1: Back Up Zigbee2MQTT (MANDATORY)
This backup preserves:
·	Network keys
·	PAN ID / Extended PAN ID
·	Device IEEE addresses
·	Routing information
Without this backup, re-pairing will be required.
✅ Method 1 (RECOMMENDED): Zigbee2MQTT UI Backup
This is the simplest and safest method.
1.	Open the Zigbee2MQTT UI
2.	Click Settings
3.	Click Tools
4.	Click Request Z2M backup
5.	Wait a few seconds
6.	Click Download Zigbee2MQTT backup
7.	Save the file to your local computer
✔ This single file contains:
·	database.db
·	coordinator_backup.json
·	configuration.yaml
·	All Zigbee network state
📌 This file alone is sufficient for a full restore.
(Optional) Alternative Backup Methods
File Editor
·	Backup /config/zigbee2mqtt/
SSH
cp -r /config/zigbee2mqtt /config/zigbee2mqtt_backup

Step 2: Stop Zigbee2MQTT
1.	Home Assistant → Settings → Add-ons → Zigbee2MQTT
2.	Click Stop
3.	Wait until the add-on status shows Stopped

Step 3: Shut Down Home Assistant
1.	Go to Settings → System → Power
2.	Click Shut Down
3.	Wait until the Raspberry Pi fully powers off
Step 4: Physically Swap the Dongles
1.	Remove Sonoff Dongle-E v2
2.	Insert Sonoff Dongle-M
3.	Use a USB extension cable
4.	Power the Raspberry Pi back on
5.	Wait for Home Assistant to fully boot
Step 5: Identify the New Serial Port (Correct & Safe Methods)
You must use a persistent /dev/serial/by-id/ path.
❌ Never use /dev/ttyUSB0
 ✅ Always use /dev/serial/by-id/...
Method 1: Zigbee2MQTT Log
1.	Home Assistant → Settings → Add-ons → Zigbee2MQTT
2.	Click Log
3.	Look for a line similar to:
/dev/serial/by-id/usb-SONOFF_SONOFF_Dongle_Max_MG24_da00da6b6bfaef11b2888d256d9880ab-if00-port0

Copy this entire path.
Method 2: SSH (Very Reliable)
1.	Open Terminal & SSH
2.	Run:
ls -l /dev/serial/by-id/

1.	Copy the left-hand path:
/dev/serial/by-id/usb-SONOFF_SONOFF_Dongle_Max_MG24_da00da6b6bfaef11b2888d256d9880ab-if00-port0

Method 3: Home Assistant Hardware Page (GUI-Only)
1.	Home Assistant → Settings → Hardware
2.	Click All Hardware
3.	Scroll to the Sonoff Zigbee dongle
4.	Click it to expand
5.	Copy the device ID / path shown
✔ Home Assistant automatically provides the stable by-id path
 ✔ /dev/ttyUSB0 is excluded by design

Step 6: Update Zigbee2MQTT Configuration
1.	Home Assistant → Settings → Add-ons → Zigbee2MQTT
2.	Click Configuration
3.	Click Serial twisty
4.	Under the serial → port, enter the new serial port you copied  in step 5: /dev/serial/by-id/usb-SONOFF_SONOFF_Dongle_Max_MG24_da00da6b6bfaef11b2888d256d9880ab-if00-port0
5.	Click Save
✅ Correct Configuration for Sonoff Dongle-M (EFR32MG24)
serial:
  port: /dev/serial/by-id/usb-SONOFF_SONOFF_Dongle_Max_MG24_da00da6b6bfaef11b2888d256d9880ab-if00-port0
  adapter: ember

⚠️ Do NOT change:
·	pan_id
·	ext_pan_id
·	network_key
·	channel
Step 7: Start Zigbee2MQTT
1.	Go to Settings → Add-ons → Zigbee2MQTT
2.	Click Start
3.	Monitor the logs
Expected Log Lines
Using Ember adapter
Coordinator started
Zigbee network started

·	Routers reconnect first
·	Battery devices reconnect when they wake
·	No pairing mode required
Step 8: Verify Operation
Zigbee2MQTT UI
·	Devices show online
·	“Last seen” timestamps update
Home Assistant
·	All entities unchanged
·	Automations continue working
Step 9: Optional Network Optimisation
After 12–24 hours:
1.	Power-cycle mains-powered routers (plugs, bulbs)
2.	Leave battery devices untouched
3.	Allow routing tables to stabilise naturally
🛑 Restore Procedure (If Needed)
1.	Stop Zigbee2MQTT
2.	Open Zigbee2MQTT UI → Settings → Tools
3.	Click Restore Zigbee2MQTT backup
4.	Upload the previously downloaded backup file
5.	Reinsert the original dongle if required
6.	Start Zigbee2MQTT
Your Zigbee network will be fully restored.
✅ Final Result
·	Same Zigbee network
·	Same devices & entities
·	Same automations
·	No re-pairing
·	Upgraded, future-proof coordinator (EFR32MG24)

