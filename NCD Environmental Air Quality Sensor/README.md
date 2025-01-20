## Introduction

The sample project consists of the sensor, which measures temperature,
humidity, pressure, and air quality, and transmits these measurements wirelessly
to the NCD Enterprise IIoT Gateway.

## How To Use This Blueprint

### 1. Unpacking and Initial Setup

#### 1.1 Required Components:
- [Industrial IoT Long Range Wireless Environmental Temperature, Humidity, Pressure, and Air Quality Sensor](https://store.ncd.io/product/industrial-iot-long-range-wireless-environmental-temperature-humidity-pressure-air-quality-sensor/)
- One of the NCD Gateway options:
    - [NCD Enterprise IIoT Gateway](https://store.ncd.io/product/enterprise-iiot-gateway/)
    - [Enterprise IIoT Gateway Lite](https://store.ncd.io/product/enterprise-iiot-gateway-lite/)
- Antennas
- Batteries (if applicable)
- Mounting accessories

Verify that you have all necessary parts. For detailed instructions on unpacking and initial setup, refer to the [drawing tab](https://store.ncd.io/product/industrial-iot-long-range-wireless-environmental-temperature-humidity-pressure-air-quality-sensor/#drawing) on the product page.

### 2. Configuring the NCD Enterprise IIoT Gateway

#### 2.1 Required Components:
- NCD Enterprise IIoT Gateway
- Antennas

The NCD Enterprise IIoT Gateway facilitates seamless integration and communication between various IoT devices and networks.

If you don't setup gateway early please go to blynk cloud and use blueprint for gatway for more clear setup https://blynk.cloud/dashboard/88287/blueprints/Library/TMPL40YtWwLlq
If you already did this - continue - dont need any other action with gateway.

#### 2.5 Access Interfaces

1. **Web Configuration:** Access the configuration interface at `http://[domain_or_ip]` using `ncdio` / `ncdB3ast` for login.
   - **Domain Name:** The domain name format is `ncd-xxxx.local`, where `xxxx`
   are the last 4 characters of the Gateway's MAC address. The MAC address can
   be found on the side of the device. For example, if the last 4 characters of
   your Gateway's MAC address are `c398`, the domain name would be `http://ncd-c398.local`.
   - **Customizations:** Set up user accounts, logging, and notifications for specific events.
2. **Node-Red:** Visit `http://[domain_or_ip]:1880` to use Node-Red.
3. **SSH Access:** Connect via SSH with `ncdio` / `ncdB3ast` on port 22.

### 3. Preparing Your Device in Blynk.Cloud

1. **Use the Sensor Blueprint:** Click the **Use Blueprint** button.
2. **Generate Static Tokens**
   - Go to the **Developer Zone**
   - Select **Static Tokens**.
   - Click **Generate Static Tokens** and choose **Generate Multiple**.
   - Select the template: **NCD Environmental Air Quality**.
   - Specify the number of tokens you need for your sensors.

3. **Download and Extract the Tokens File**
   - Download the generated ZIP archive.
   - Extract it and locate the file named `*_tokens.csv` (insteaf of * must be varior number).
   - Save this file.

### 4. Transfer file with token to the gateway

   If you don't turn on ssh on gateway please follow this instruction, if already do move forward.
   - Open the Gateway's web dashboard at:
       `http://ncd-xxxx.local`
     - Go to **Service** > **SSH**.
     - Turn **ON** SSH, then click **Submit** and **Apply**.

4. **Transfer the Tokens File to the Gateway**
   Linux::

   - Open the terminal in folder contains file with token by open in Files this folder and tab right button of mouse and choose open in Terminal
   - Copy command without change and use the following command to transfer the `*_tokens.csv` file to the Gateway:
     ```bash
     scp *_tokens.csv ncdio@ncd-a9c2.local:/home/ncdio/.node-red/
     ```
     Password: `ncdB3ast`

   **Note:** Replace `xxxx` in `ncd-xxxx.local` with the last 4 characters of the Gateway's MAC address, which is printed on the device.
   - Example: If the MAC ends in `c398`, use `http://ncd-c398.local`.

   Windows:


### 5. Accessing Node-RED on the Gateway

1. Open a web browser and navigate to `http://[domain_or_ip]:1880`.
2. Log in to the Node-RED dashboard.

### 6. Configuring the Node-RED Flow

1. Copy or download the firmware flow file from second step of device activation into Node-RED.
   - In Node-RED, press **CTRL + I** or use the menu to select **Import**.
   - Paste the copied code or upload the flow file (.json).
2. **Set Up filename of token file:**
   - At the dashboard you see the node with name **Provide filename with token** tab on this and you see the new window where can locate field like msg.filename and in value instead of "** _tokens.csv**" type a name of file that you trasfre to gateway(can type only number located before _tokens.scv)

![Node-RED Flow](https://raw.githubusercontent.com/khrystynaO/blueprints/khrystynaO/feature/NCD/NCD%20Environmental%20Air%20Quality%20Sensor/Image/flow.png)

### 7. Running the Sample Using Node-RED

1. **Deploy the Flow:**
   - Click the Deploy button in Node-RED to activate the flow. When the flow
   connects to the cloud, the status indicator of the MQTT node will turn green
   and display the text "Connected."

2. **Test Your Setup:**
   - Go to Blynk.cloud to check the sensor data and verify everything is working.
     The Environmental Air Quality Dashboard provides a comprehensive,
     real-time visualization of key environmental parameters to help users
     monitor and analyze indoor air quality. This intuitive interface presents
     essential data points and insights, enabling informed decision-making to
     maintain a healthy environment.

#### **Key Features:**

1. **Device Overview:**
   - Displays the device name, status (online/offline), and ownership information.
   - Provides easy access to device settings and configurations.

2. **Air Quality Metrics:**
   - **Indoor Air Quality Index (IAQ):** A visual gauge that presents the current air quality score, helping users quickly assess the environmental conditions.
   - **Status Interpretation:** A user-friendly message indicating the air quality level and its potential impact on health (e.g., "Air is GOOD 😊").
   - **Historical Data Trends:** Graphical representation of air quality variations over different timeframes for tracking long-term changes.

3. **Environmental Parameters:**
   - **Temperature:** Displays the current temperature value, often color-coded to indicate normal or critical levels.
   - **Humidity:** Monitors moisture levels in the environment, providing insights into comfort and potential health concerns.
   - **Pressure:** Shows atmospheric pressure readings, which can affect indoor climate conditions.
   - **Gas Resistance:** Represents the concentration of airborne pollutants, aiding in the detection of potential hazards.

4. **Battery Status:**
   - A dedicated gauge displaying the current battery level to ensure continuous monitoring and timely maintenance.

5. **Data Visualization & History:**
   - Offers live and historical data views, allowing users to analyze trends over various periods, such as hours, days, or months.

## Next Steps

- Explore the Blynk Web Console and Blynk IoT app, and try monitoring your sensor data from both.
- Read the Blynk Documentation to learn how to work with Virtual Pins.
- [Learn more about Automations.](https://docs.blynk.io/en/concepts/automations)
- [Discover how to Share Devices with other users.](https://docs.blynk.io/en/blynk.console/devices/device-sharing)
- Customize the code to meet your specific needs.
- Add more sensors or devices to your setup.

## Troubleshooting

- Ensure you have the latest versions of the software you are using.
- Verify that all libraries are up to date.
- Check that all dependencies and configurations are correct.
- Inspect your code for errors.

### Common Issues:

1. **Connection Problems:**
   - Ensure the sensor is powered and within range.
   - Check network settings and firewall rules.
   - Verify the gateway's network connection.

2. **Calibration Errors:**
   - Follow the software instructions to recalibrate the sensor.
   - Ensure the environment is stable during calibration.

3. **Node-RED Issues:**
   - **Flow Not Deploying:** Check for syntax errors or missing nodes.
   - **MQTT Connection Failure:** Verify MQTT broker settings and credentials.

4. If you receive the following message, generate more tokens and upload the
   `*_tokens.csv` file to the Gateway, you don't need restart the flow for this. The file can contain only new tokens, or
    both new and old tokens:

    ```
    No unused tokens available for 00:13:a2:00:42:4f:17:12 in the tokens map.
    Please create more static tokens for One Channel Vibration Plus (sensor type = 80).
    ```
5. If you see this error, upload the `*_tokens.csv` file and restart the flows:

    ```
    Error: /home/ncdio/.node-red/17_tokens.csv does not exist.
    ```
## Related Links

- [Blynk MQTT API documentation](https://docs.blynk.io/en/hardware-guides/mqtt-api)
- [Blynk Troubleshooting guide](https://docs.blynk.io/en/getting-started/troubleshooting)
- [Blynk Documentation](https://docs.blynk.io/en/)
- [Blynk NodeRED Documentation](https://docs.blynk.io/en/hardware-guides/node-red)
- [NCD Wireless Environmental Sensor Documentation](https://ncd.io/blog/wireless-environmental-sensor-product-manual/)
- [Node-RED Documentation](https://nodered.org/docs/getting-started/local)
- [NCD Support](https://ncd.io/contact/)
- [RT090 HM EG5120 V1.0.2 Documentation](https://www.davantel.com/wp-content/uploads/2023/06/RT090_HM_EG5120_V1.0.2.pdf)
- [RobustOS Software Manual V1.0.0](https://rjconnect.co.za/wp-content/uploads/2022/12/RT_SM_RobustOS-Software-Manual_V1.0.0.pdf)

By following this blueprint, you can effectively set up and utilize your NCD Wireless Environmental Sensor for various IoT applications.

