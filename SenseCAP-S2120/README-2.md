## **SenseCAP S2120 8-in-1 Weather Sensor Blueprint Guide**

The **SenseCAP S2120 8-in-1 LoRaWAN Weather Sensor** is a powerful device designed to measure and transmit key environmental data including **air temperature**, **humidity**, **barometric pressure**, **wind speed**, **wind direction**, **precipitation**, **solar radiation**, and **UV index**. This guide walks you through setup, registration on **The Things Network (TTN)**, and integration with **Blynk IoT** for real-time monitoring and control.

---

## **Step 1: Unpacking and Initial Setup**

### **1.1 Required Components**
- **SenseCAP S2120 Weather Sensor**
- **LoRaWAN Gateway** (e.g., SenseCAP Gateway)

### **1.2 Initial Setup**
1. **Check Package Contents**: Ensure all parts are present and undamaged.
2. **Install Batteries or Power Supply**: Follow the manual to correctly power up the sensor.
3. **Activate the Sensor**: The LED should blink or show status indicating it's ready to join the network.

---

## **Step 2: Registering the Sensor on The Things Network (TTN)**

### **2.1 Setting Up the LoRaWAN Gateway**
1. Log in to the [TTN Console](https://console.cloud.thethings.network/).
2. Navigate to **Gateways** > **Add Gateway** and fill in:
   - **Gateway EUI** (from the device label)
   - **Gateway ID** (e.g., `my-sensecap-gateway`)
   - **Frequency Plan** matching your region (EU868, US915, etc.)
   - Click **Register Gateway**
3. Follow manufacturer instructions to bring the gateway online.

!![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/gateway.png)


#### **Frequency Plans**
| **Frequency Plan** | **Region**            | **Description**                                                                 |
|--------------------|-----------------------|---------------------------------------------------------------------------------|
| **EU868**          | Europe                | Operates at 868 MHz. Commonly used across the EU for LoRaWAN deployments.       |
| **US915**          | North America         | Operates at 915 MHz. Used in the United States, Canada, and Mexico.             |
| **AS923**          | Asia-Pacific          | Operates at 923 MHz. Covers countries like Japan, Australia, and New Zealand.   |
| **IN865**          | India                 | Operates at 865 MHz. Dedicated plan for deployments in India.                   |
| **AU915**          | Australia/New Zealand | Operates at 915 MHz. Preferred for LoRaWAN in Australia and New Zealand.        |
| **KR920**          | South Korea           | Operates at 920 MHz. Allocated specifically for South Korea.                    |
| **RU864**          | Russia                | Operates at 864 MHz. Used exclusively within Russia.                            |

---

### **2.2 Creating a New Application**
1. **Log in to TTN Console**.
2. **Create an Application:**
   - Navigate to **Applications** > **Add Application**.
   - Enter:
     - **Application ID**: A unique name (e.g., `sound-monitor`).
     - **Description**: (e.g., `Monitoring environmental noise levels`).
     - **Handler**: Select the handler for your region.
   - Click **Create Application**.

![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/create-form.png)

> **Note:**
> To ensure smooth operation, it’s recommended that all devices within a TTN
> application are of the same sensor type (e.g., SenseCAP-S2120). If you’d like to use
> different types of sensors, consider creating a separate application for each
> sensor type. This helps maintain better compatibility and performance.

The dashboard for the created application looks like this:

![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/dash-app.png)

---

### **2.3 Pairing the Sensor with the TTN Application**
1. **[Download and install SenseCAP Mate App](https://www.pgyer.com/sensecapmate)**

2. **Connect to Sensor to App**:
   - After the battery is installed, the red LED will be steady on. Also activate Bluetooth. Press button and hold for 3 seconds, the LED will flash at 1s frequency.
   - Please click the “Setup” button to turn on Bluetooth and click “ Scan ” to start scanning the sensor's Bluetooth.
   - Enter the pairing password. The default password is 000000.
   - Select the Sensor by BT ID (BT ID is on the bottom label of the sensor). Then, the basic information of the sensor will be displayed after entering.

![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/mobile-2.png)

3. **Configure parameters**:
   S2120 weather stations are manufactured to support universal frequency plan from 863MHz ~928MHz in one SKU. That is to say, every single device can support 7 frequency plans.
   - Set the corresponding frequency band based on the frequency band of the gateway.
   - Click the “Setting” and select the platform is “The Things Network”.
   - Select the Frequency Plan, if your gateway is US915, set the sensor to US915.
   - The device uses OTAA to join the LoRaWAN network by default. So, we can set the device EUI, App EUI and APP Key here.
       - **DevEUI**: Use the default value (found on the sensor label).
       - **AppEUI**: Use the manufacturer’s default value or generate a new one.
       - **AppKey**: Use the manufacturer’s default or generate a new one.
  - Click the “Send” button, send the setting to the sensor for it to take effect.
  - Click the “Home” button, the App will disconnect the Bluetooth connection. Then, the sensor will reboot.
  - When the device is disconnected from Bluetooth, the LED lights up for 5 seconds and then flashes as a breathing ligh


![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/mobile.png)

---

### **2.4 Configuring MQTT Integration in TTN**
1. **Navigate to TTN Integration Settings**:
   - In the TTN Console, open your application and select **Integrations**.
2. **Select MQTT**:
   - Click **MQTT** from the available integration options.
3. **You’ll need these details to set up the MQTT integration in Blynk**:
   - **Server Address**
   - **Port**
   - **Username**
   - **Password**: Click **Generate new API key** and copy the generated key. Keep it secure.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/integarion-mqtt-key.png)

   - In the **API Keys** section of the left menu, select the newly generated key.
   - Under **Rights**, select **Grant individual rights** and ensure the following permissions are enabled:
     - **View device keys in application**
     - **Create devices in application**
     - **Edit device keys in application**

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/edit-permition.png)

---

## **Step 3: Integrating TTN with Blynk**

### **3.1 Configuring Blynk Integration via MQTT**
1. **Click "Use Blueprint"** at the top of the blueprint page.
2. **Set Up MQTT in Blynk:**
   - Navigate to **Developer Zone** -> **Integrations** -> **The Things Stack** -> **Add**, and choose the template named **SenseCAP-S2120**.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/add.png)

   - Enter the data from **Step 2:**
     - **Hostname:** MQTT server address with port `1883` or `8883` (e.g., `eu1.cloud.thethings.network:8883`)
     - **Username**
     - **Password:** The copied API key
   - Click **Connect**.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/connect.png)

3. **Enable Automated Device Onboarding:**
   - Once connected, click **Edit** and toggle **Enable automated device onboarding**.
   - Specify the LoRaWAN version, frequency plan, and regional parameter version.
   - Enable "Support class B" and/or "Support class C" if required.
   - Specify The Things Stack component addresses (found under **Network Information** in the **Admin Panel**).
   - Save changes.

	![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/advanced-complete.png)
---

## **Step 4: Onboarding the Sensor to Blynk**

1. Go to **Developer Zone** > **My Templates** > select **SenseCAP-S2120 Weather Station**, then click **Activate Device**.

2. In the new window, enter the data provided in **Step 2.3:**
   - **Application Key**
   - **Join EUI**
   - **DevEUI**

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/sample-data.png)

3. Click **Next** and wait for the device to come online.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/wait-for-dev.png)

You can verify the device’s status in TTN. To add more sensors, create new devices in Blynk following the same process. Once the device is online, you'll be redirected to the dashboard to test your setup.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/created-dev.png)

---

## **Step 5: Monitoring and Analyzing Data**

### **5.1 Dashboard Overview**
The dashboard is preconfigured, providing an intuitive interface for real-time monitoring. Both web and mobile dashboards offer the same functionality. Key features include:

### **5.1 Dashboard Overview**

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/img_1.png)

| Widget                 | Virtual Pin | Unit      |
|------------------------|-------------|-----------|
| Air Temperature        | V1          | °C        |
| Air Humidity           | V0          | %         |
| Barometric Pressure    | V2          | Pa        |
| Wind Speed             | V13         | m/s       |
| Wind Direction         | V12         | °         |
| Precipitation          | V9          | mm/h      |
| UV Index               | V11         | Index     |
| Light Intensity        | V7          | lux       |
| Battery Level          | V3          | %         |

---

### **5.2 Device Details Tab**

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAP-S2120/Image/img_2.png)

| Widget                 | Virtual Pin | Description                        |
|------------------------|-------------|------------------------------------|
| Battery Level          | V3          | Shows remaining battery percent    |
| GPS Interval           | V5          | Reporting interval (if applicable) |
| Measurement Interval   | V8          | Current interval in seconds        |
| Set Interval           | V8          | Dropdown to update interval        |
| Firmware Version       | V4          | Version string                     |
| Hardware Version       | V6          | Version string                     |
| Reboot Device          | V10         | Remote reboot toggle               |

---

## **Step 6: Alerts & Automations**

1. Navigate to **Automations** in Blynk Console.
2. Choose **Device State** or **Sensor Value** triggers.
3. Example triggers:
   - **UV Index > 6** → send mobile notification
   - **Wind Speed > 10 m/s** → update dashboard label
4. Save and test automations.

---

### **Step 6 Setting Alerts**
1. **Log in to Blynk Console**.
2. **Create Automations**:
   - Choose **Device State**
   - Choose your device and triggers for specific thresholds (e.g., LAeq > 80 dB).
   - Define actions (e.g., send notifications).
3. **Save and Test**:
   - Simulate events to ensure triggers activate correctly.

---

## **Step 7: Adding Location in Blynk**

### **5.1 Configuring Location**
1. **Go to Location Section in Blynk**:
   - In the Blynk dashboard, navigate to the **Location** section.
2. **Add the Address**:

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAP-S2120/SenseCAP-S2120/Image/location.png)

   - Enter the sensor's address or drag the pin on the map to the correct location.
   - Save the location.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAP-S2120/SenseCAP-S2120/Image/location-2.png)

3. **Assign Location in Metadata**:
   - Navigate to device **Info & Metadata** in Blynk.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAP-S2120/SenseCAP-S2120/Image/location-3.png)

   - Assign the saved location to the device.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAP-S2120/SenseCAP-S2120/Image/location-4.png)

🎥 **Watch the Video Guide**
[![Watch the video](https://img.youtube.com/vi/YbFF-Iec38w/maxresdefault.jpg)](https://youtu.be/YbFF-Iec38w)

## **Step 6: Error Handling and Troubleshooting**

### **6.1 Common Issues**
1. **Sensor Issues:**
   - Check battery polarity and charge
   - Reset the sensor if unresponsive
2. **Connectivity Problems:**
   - Verify LoRaWAN settings and gateway connection
   - Test MQTT integration
3. **Data Issues:**
   - Check the payload formatter in TTN
   - If only the battery level is displayed or data is missing, go to **TTN** > **Your Application** > **Payload Formatters** > **Uplink/Downlink**, update the code, and save changes

Uplink:
````
---

## **Next Steps**
- Explore the Blynk Web Console and IoT app to monitor sensor data
- Read the Blynk Documentation to learn about Virtual Pins
- [Learn how to Share Devices with other users](https://docs.blynk.io/en/blynk.console/devices/device-sharing)
- Customize the code to meet your specific needs
- Try onboarding the sensor using static tokens

---


