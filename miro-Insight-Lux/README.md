# **Miromico Miro Insight - Environmental and Hall Sensor Blueprint Guide**

The **Miromico Miro Insight** is a compact battery-powered LoRaWAN device
equipped with a **temperature sensor**, **humidity sensor**, and **hall
sensor**. It’s designed for real-time environmental monitoring and magnetic
field detection, transmitting data over LoRaWAN networks. This guide provides
step-by-step instructions to set up the sensor, register it with **The Things
Network (TTN)**, and integrate it with **Blynk IoT** for data visualization and
control.

---

# **Step 1: Unpacking and Initial Setup**

## **1.1 Required Components**
- **Miromico Miro Insight Sensor**
- **LoRaWAN Gateway** connected to TTN
- **Activation Magnet** (for waking up the device)

## **1.2 Initial Setup**
1. **Verify Package Contents:** Ensure the sensor, documentation, and accessories are included and undamaged.
2. **Wake Up the Device:**
   - Hold the magnet near the activation point of the device for a few seconds.
   - The LED should blink to confirm the device is active.
3. **Check Battery:** Pre-installed. No need for manual installation.

> **Note:** Miro Insight devices are preconfigured and activated via **magnet** instead of a physical power button.

---

# **Step 2: Registering the Sensor on The Things Network (TTN)**

## **2.1 Setting Up the LoRaWAN Gateway**

Set up your LoRaWAN Gateway as usual in the TTN Console:

- Register the **Gateway**.
- Use the correct **Frequency Plan** for your region:

| **Frequency Plan** | **Region**            |
|--------------------|------------------------|
| **EU868**          | Europe                 |
| **US915**          | North America          |
| **AU915**          | Australia / New Zealand |

Follow TTN’s Gateway setup guide to ensure your gateway is online.

---

## **2.2 Creating a New Application in TTN**

1. In the TTN Console, navigate to **Applications → Add Application**.
2. Fill in:
   - **Application ID:** (e.g., `miro-insight-environmental-monitoring`)
   - **Description:** (Optional)
   - **Handler:** Based on your region.
3. **Create Application**.

---

## **2.3 Setting Up the Payload Formatter**

You **must create a Custom JavaScript Formatter** for Miro Insight’s data.

Paste this decoder into the **Uplink Formatter** in TTN:

```javascript

function signed(value, size) {
    const mask = 1 << (size * 8 - 1);
    const buf = -(value & mask) + (value & ~mask);
    return buf;
}

function toHexString(byteArray) {
    let s = '';
    byteArray.forEach(function(byte) {
        s += ('0' + (byte & 0xFF).toString(16)).slice(-2);
    });
    return s;
}

function decodeUplink(input) {
    const bytes = input.bytes;
    const port = input.fPort;

    const decoded = {};
    const warnings = [];
    const errors = [];
    const output = { data: {} };

    if (port === 15) {
        let idx = 0;
        const total = bytes.length;

        while (idx < total) {
            const length = bytes[idx];

            switch (bytes[idx + 1]) {
                case 1: { // Temperature and Humidity
                    let start = idx + 2;
                    let tempArr = [];
                    let humArr = [];
                    while (start < idx + length) {
                        tempArr.push(
                            signed(bytes[start] + bytes[start + 1] * 256, 2) / 100
                        );
                        humArr.push(bytes[start + 2] / 2);
                        start += 3;
                    }
                    decoded.temperature = tempArr.length === 1 ? tempArr[0] : tempArr;
                    decoded.humidity = humArr.length === 1 ? humArr[0] : humArr;
                    break;
                }

                case 2: { // CO2
                    let start = idx + 2;
                    let co2Arr = [];
                    while (start < idx + length) {
                        co2Arr.push(bytes[start] + bytes[start + 1] * 256);
                        start += 2;
                    }
                    decoded.co2 = co2Arr.length === 1 ? co2Arr[0] : co2Arr;
                    break;
                }

                case 5: { // Common Settings
                    decoded.settings_cycle = bytes[idx + 2] + bytes[idx + 3] * 256;
                    decoded.settings_tx_interval = bytes[idx + 4];
                    decoded.settings_cfm = Boolean(bytes[idx + 5] & 0x80);
                    decoded.settings_led = Boolean(bytes[idx + 5] & 0x40);
                    decoded.settings_adr = Boolean(bytes[idx + 5] & 0x20);
                    decoded.settings_continous_iaq = Boolean(bytes[idx + 5] & 0x10);
                    decoded.settings_rpt_int = Boolean(bytes[idx + 5] & 0x08);
                    decoded.settings_nbretrans = bytes[idx + 6] & 0x0f;
                    break;
                }

                case 6: { // CO2 Settings
                    decoded.settings_co2_subsamples =
                        bytes[idx + 4] + bytes[idx + 5] * 256;
                    decoded.settings_co2_abc_period =
                        bytes[idx + 6] + bytes[idx + 7] * 256;
                    break;
                }

                case 9: { // Battery
                    decoded.battery_v = (bytes[idx + 2] + bytes[idx + 3] * 256) / 100;
                    break;
                }

                case 10: { // Firmware Hash
                    decoded.git_hash = toHexString(
                        bytes.slice(idx + 2, idx + 6).reverse()
                    );
                    break;
                }

                case 11: { // Door Alarm Set
                    decoded.door_alarm = true;
                    decoded.open_counter =
                        bytes[idx + 2] +
                        bytes[idx + 3] * 256 +
                        bytes[idx + 4] * 256 * 256 +
                        bytes[idx + 5] * 256 * 256 * 256;
                    decoded.alarm_counter = bytes[idx + 6] + bytes[idx + 7] * 256;
                    decoded.settings_alarm_time_s = bytes[idx + 8] + bytes[idx + 9] * 256;
                    break;
                }

                case 12: { // Door Alarm Clear
                    decoded.door_alarm = false;
                    decoded.open_counter =
                        bytes[idx + 2] +
                        bytes[idx + 3] * 256 +
                        bytes[idx + 4] * 256 * 256 +
                        bytes[idx + 5] * 256 * 256 * 256;
                    decoded.alarm_counter = bytes[idx + 6] + bytes[idx + 7] * 256;
                    break;
                }

                case 13: { // Door Status
                    decoded.open_counter =
                        bytes[idx + 2] +
                        bytes[idx + 3] * 256 +
                        bytes[idx + 4] * 256 * 256 +
                        bytes[idx + 5] * 256 * 256 * 256;
                    decoded.alarm_counter = bytes[idx + 6] + bytes[idx + 7] * 256;
                    decoded.door_alarm = Boolean(bytes[idx + 8]);
                    break;
                }

                case 14: { // Alarm Settings
                    decoded.settings_alarm_time_s = bytes[idx + 2] + bytes[idx + 3] * 256;
                    decoded.settings_hall_debounce_ms =
                        bytes[idx + 4] + bytes[idx + 5] * 256;
                    decoded.settings_door_status_time_s =
                        bytes[idx + 6] +
                        bytes[idx + 7] * 256 +
                        bytes[idx + 8] * 256 * 256 +
                        bytes[idx + 9] * 256 * 256 * 256;
                    break;
                }

                case 15: { // IAQ
                    let start = idx + 2;
                    let iaqArr = [];
                    let iaqAccArr = [];
                    while (start < idx + length) {
                        iaqArr.push(bytes[start] + (bytes[start + 1] & 0x3f) * 256);
                        iaqAccArr.push(bytes[start + 1] >> 6);
                        start += 2;
                    }
                    decoded.iaq = iaqArr.length === 1 ? iaqArr[0] : iaqArr;
                    decoded.iaq_accuracy = iaqAccArr.length === 1 ? iaqAccArr[0] : iaqAccArr;
                    break;
                }

                case 16: { // Pressure
                    let start = idx + 2;
                    let pressureArr = [];
                    while (start < idx + length) {
                        pressureArr.push(
                            bytes[start] +
                            bytes[start + 1] * 256 +
                            bytes[start + 2] * 256 * 256
                        );
                        start += 3;
                    }
                    decoded.pressure = pressureArr.length === 1 ? pressureArr[0] : pressureArr;
                    break;
                }

                case 17: { // Reported Interval
                    decoded.reported_interval = bytes[idx + 2] + bytes[idx + 3] * 256;
                    break;
                }

                case 20: { // Light
                    decoded.light = bytes[idx + 2] + bytes[idx + 3] * 256;
                    break;
                }

                case 21: { // Conditional TX thresholds
                    decoded.settings_cond_tx_co2_th = bytes[idx + 2] + bytes[idx + 3] * 256;
                    decoded.settings_cond_tx_temp_th = bytes[idx + 4] + bytes[idx + 5] * 256;
                    decoded.settings_cond_tx_hum_th = bytes[idx + 6] + bytes[idx + 7] * 256;
                    break;
                }

                case 22: {
                    decoded.settings_blind_adr_profile = bytes[idx + 2];
                    break;
                }

                case 23: {
                    decoded.settings_light_interval = bytes[idx + 2];
                    break;
                }

                default:
                    warnings.push("Unknown message type: " + bytes[idx + 1]);
                    break;
            }

            idx += length + 1;
        }
    } else {
        errors.push("Uplink is not on port 15");
    }

    if (warnings.length > 0) {
        decoded.warnings = warnings;
    }

    if (errors.length > 0) {
        decoded.errors = errors;
    }

    output.data = decoded;
    return output;
}

```

And **Downlink Formatter**:

```javascript

function encodeDownlink(input) {
  const output = {
    bytes: [],
    fPort: 3,    // fPort is 3 as you said
    warnings: [],
    errors: []
  };

  // Use input.data if available
  const commandInput = input.data ? input.data : input;

  try {
    if (commandInput.name === "reporting_iterval") {   // fixed small typo here too

      const measurementInterval = 300; // only user input

      // Hardcoded values
      const sendCycle = 0;        // Send after 1 measurement
      const flagsByte = 0x28;     // No confirmation, LED off, ADR on, VOC off, Report interval on
      const retransmissions = 0;  // 3 retransmissions

      // Build the packet
      output.bytes.push(0x06);  // Length
      output.bytes.push(0x87);  // Type

      output.bytes.push(measurementInterval & 0xFF);            // Measurement interval LSB
      output.bytes.push((measurementInterval >> 8) & 0xFF);     // Measurement interval MSB

      output.bytes.push(sendCycle);                             // Send cycle (hardcoded)
      output.bytes.push(flagsByte);                             // Flags (hardcoded)
      output.bytes.push(retransmissions & 0x0F);                // Retransmissions (hardcoded)

    } else {
      output.errors.push("Unsupported command: " + JSON.stringify(commandInput));
    }
  } catch (err) {
    output.errors.push("Encoding error: " + err.message + ". Input: " + JSON.stringify(commandInput));
  }

  return output;
}

function decodeDownlink(input) {
  return {
    data: {
      bytes: input.bytes
    },
    warnings: [],
    errors: []
  };
}

```

Save the formatter.

---

# **Step 3: Activating the Sensor**

- Use the **magnet** to wake up the sensor.
- Confirm the LoRaWAN settings:
  - **DevEUI:** Printed on device label.
  - **JoinEUI (AppEUI)**: Provided by Miromico.
  - **AppKey**: Provided by Miromico.

> 📡 **Important:** Miromico is simple — all settings are preprogrammed unless manually reconfigured via LoRaWAN MAC commands.

---

# **Step 4: Setting Up MQTT Integration in TTN**

1. In the TTN Console:
   - Open your **Application**.
   - Go to **Integrations** → **MQTT**.
2. Configure MQTT details:
   - **Hostname:** `eu1.cloud.thethings.network` (or region-specific server)
   - **Port:** 8883 (for secure MQTT)
   - **Username:** From TTN Console
   - **Password:** Generate an API Key and copy it.

---

# **Step 5: Integrating TTN with Blynk**

1. In **Blynk Console**:
   - Click **Use Blueprint**.
   - Go to **Developer Zone → Integrations → The Things Stack → Add**.
2. Choose the **Miromico Miro Insight** template.
3. Fill in:
   - **MQTT Hostname:** (e.g., `eu1.cloud.thethings.network`)
   - **Port:** 8883
   - **Username** and **Password**: From TTN
4. Click **Connect**.

---

# **Step 6: Adding the Sensor as an End Device in TTN**

- In your TTN Application:
  - **Register End Device**.
  - Select:
    - **Brand:** Miromico
    - **Model:** Miro Insight
- Enter:
  - **DevEUI**, **JoinEUI (AppEUI)**, **AppKey**.
- Choose the correct region based on your Frequency Plan.

---

# **Step 7: Onboarding the Sensor to Blynk**

**Automatic onboarding:**
- When data is received from TTN via MQTT, the device will automatically appear in the Blynk Console.

**If Manual creation needed:**
- Go to **Devices → Create Device**.
- Choose the **Miromico Miro Insight** template.

---

# **Step 8: Monitoring and Analyzing Data**

The **Blynk dashboard** will show:

| **Field**            | **Description**                  |
|----------------------|------------------------------------|
| **Temperature**       | Current temperature (°C)          |
| **Humidity**          | Current relative humidity (%)     |
| **Hall Sensor State** | 0 = No magnet, 1 = Magnet detected |
| **Battery Voltage**   | Current battery voltage (V)       |

You can view live data and historical trends.

---

# **Step 9: Alerts & Automations**

Example automations you can create:

- If **humidity > 80%**, send notification.
- If **battery voltage < 2.8V**, send low battery alert.
- If **hall sensor** detects magnet (value = 1), trigger an event.

---

# **Step 10: Adding Location in Blynk**

- In Blynk, go to **Location**.
- Drag the pin to your sensor’s physical location.
- Save the changes.
- Assign the location to the device.

---

# **Troubleshooting**

| **Issue**             | **Solution**                                           |
|-----------------------|---------------------------------------------------------|
| No Join Request       | Wake up sensor with magnet, check gateway visibility.   |
| No Uplink Received    | Confirm DevEUI/AppEUI/AppKey match, check coverage.      |
| Incorrect Payload     | Verify the TTN Payload Formatter code.                  |

---

# **Next Steps**
- Customize dashboards.
- Set notification thresholds.
- Explore historical environmental data trends.
