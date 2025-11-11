# Victron MQTT Entities for Home Assistant

This is a template file for adding a comprehensive set of Victron entities to Home Assistant via MQTT.

It creates a main "Cerbo GX" device and links child devices for "Batteries," "Solar," "Alternator," and "Tanks" to keep your entities clean and organized.

## How to Install

### 1. Find Your VRM Portal ID

1.  Open **MQTT Explorer** and connect to your MQTT broker.
2.  Look for the main `victron` topic.
3.  Under `victron`, you will see a topic `N` (for Notifications).
4.  Under `N`, you will see a unique 12-character string. This is your **VRM Portal ID** (e.g., `c0619ab9478b`).
5.  Copy this ID.

### 2. Configure the Template

1.  Download the `victron_mqtt.yaml` file from this Gist.
2.  Open it in a text editor.
3.  Use "Find and Replace" to replace all instances of **`YOUR_VRM_PORTAL_ID`** with your 12-character ID from step 1.

### 3. Check Your Device Instance IDs (Important!)

This template assumes standard instance IDs (e.g., `vebus/276`, `solarcharger/279`). Your system may be different.

1.  In **MQTT Explorer**, check the topics for your devices.
2.  For example, if your Vebus inverter is under `victron/N/YOUR_ID/vebus/277`, you must change all references of `vebus/276` in the YAML file to `vebus/277`.
3.  Do this for all devices: `vebus`, `solarcharger`, `battery`, `alternator`, and `tank`.

### 4. Install in Home Assistant

1.  Place your edited `victron_mqtt.yaml` file in your Home Assistant `/config` directory (or a sub-directory, like `/config/mqtt`).
2.  Open your main `configuration.yaml` file.
3.  Add this line to include your new file, under your existing `mqtt:` configuration:

```yaml
mqtt:
  # ... other mqtt settings ...
  sensor: !include_dir_merge_list mqtt/sensors
  binary_sensor: !include_dir_merge_list mqtt/binary_sensors
  switch: !include_dir_merge_list mqtt/switches
  
  # Add this line:
  !include victron_mqtt.yaml
