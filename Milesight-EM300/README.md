## **Milesight EM300 - Water Leak Detector Blueprint Guide**

The **Milesight EM300 Water Leak Detector** monitors and detects water leaks in
various environments, providing real-time alerts to prevent damage. It also
monitors battery levels and RSSI for reliable operation. This guide provides
step-by-step instructions to set up the sensor, register it with **The Things
Network (TTN)**, and integrate it with **Blynk IoT** for data visualization and
control.

---

## **Step 1: Unpacking and Initial Setup**

### **1.1 Required Components**
- **Milesight EM300 Water Leak Detector**
- **LoRaWAN Gateway** for connectivity

### **1.2 Initial Setup**
1. **Verify Package Contents:** Make sure all components are included and undamaged.
2. **Install the Batteries:** Insert the batteries into the sensor, ensuring the correct polarity.
3. **Power On the Sensor:** Hold the power button for 3 seconds. The LED will confirm activation.

---

## **Step 2: Registering the Sensor on The Things Network (TTN)**

### **2.1 Setting Up the LoRaWAN Gateway if you don't do this before**
1. **Log in to the TTN Console:** Visit the [TTN Console](https://console.cloud.thethings.network/).
2. **Add a Gateway:**
   - Navigate to **Gateways** > **Add Gateway**.
   - Enter:
     - **Gateway EUI** (found on the gateway label)
     - **Gateway ID** (e.g., `my-lorawan-gateway`)
     - **Frequency Plan** for your region (see the table below)
   - Click **Register Gateway**.
3. **Configure the Gateway:**
   - Follow the manufacturer's instructions to connect the gateway to TTN.
   - Ensure the gateway is online and visible in the TTN Console.

![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/gateway.png)

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
     - **Application ID**: A unique name (e.g., `water-leak-monitor`).
     - **Description**: (e.g., `Monitoring water leaks in critical areas`).
     - **Handler**: Select the handler for your region.
   - Click **Create Application**.

![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/create-form.png)

> **Note:**
> To ensure smooth operation, it’s recommended that all devices within a TTN
> application are of the same sensor type (e.g., EM300). If you’d like to use
> different types of sensors, consider creating a separate application for each
> sensor type. This helps maintain better compatibility and performance.

The dashboard for the created application looks like this:

![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/dash-app.png)

---

### **2.3 Pairing the Sensor with the TTN Application**
1. **[Download and install Milesight ToolBox App](https://www.milesight.com/iot/resources/download-center/#milesight-toolbox)**

2. **Open the Milesight ToolBox App**:
   - Pair the sensor via NFC.
   - Navigate to **Settings** -> **LoRaWAN Settings**.

![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/mobile-2.png)

3. **Configure LoRaWAN Parameters**:
   - **DevEUI**: Use the default value (found on the sensor label).
   - **AppEUI**: Use the manufacturer’s default value or generate a new one.
   - **AppKey**: Use the manufacturer’s default or generate a new one.
   - **Frequency Plan**: Match the frequency plan configured in TTN.
   - **RX2 Data Rate**: Set to DR3 (SF9, 125 kHz).
   - **Spreading Factor**: Set to SF10-DR2.
   - **Default Application Port**: Set to 85.
   - Use the **Write** button to save these settings via NFC.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/mobile.png)

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

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/integarion-mqtt-key.png)

   - In the **API Keys** section of the left menu, select the newly generated key.
   - Under **Rights**, select **Grant individual rights** and ensure the following permissions are enabled:
     - **View device keys in application**
     - **Create devices in application**
     - **Edit device keys in application**

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/edit-permition.png)

---

## **Step 3: Integrating TTN with Blynk**

### **3.1 Configuring Blynk Integration via MQTT**
1. **Click "Use Blueprint"** at the top of the EM300 blueprint page.
2. **Set Up MQTT in Blynk:**
   - Navigate to **Developer Zone** -> **Integrations** -> **The Things Stack** -> **Add**, and choose the template named **Milesight EM300**.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/add.png)

  - Enter the data from **Step 2:**
     - **Hostname:** MQTT server address with port `1883` or `8883` (e.g., `eu1.cloud.thethings.network:8883`)
     - **Username**
     - **Password:** The copied API key
   - Click **Connect**.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/connect.png)

3. **Enable Automated Device Onboarding:**
   - Once connected, click **Edit** and toggle **Enable automated device onboarding**.
   - Specify the LoRaWAN version, frequency plan, and regional parameter version.
   - Enable "Support class B" and/or "Support class C" if required.
   - Specify The Things Stack component addresses (found under **Network Information** in the **Admin Panel**).
   - Save changes.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/advanced-complete.png)

----

## **Step 4: Onboarding the Sensor to Blynk**

1. Go to **Developer Zone** > **My Templates** > select **Milesight EM300**, then click **Activate Device**.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/bp-dashboard.png)

2. In the new window, enter the data provided in **Step 2.3:**
   - **Application Key**
   - **Join EUI**
   - **DevEUI**

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/sample-data.png)

3. Click **Next** and wait for the device to come online.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/wait-for-dev.png)

You can verify the device’s status in TTN. To add more sensors, create new
devices in Blynk following the same process. Once the device is online, you'll
be redirected to the dashboard to test your setup.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/created-dev.png)

When data from the sensor is received in TTN, you will see something like this in the dashboard:

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/ttn-dash-with-data.png)

---

## **Step 5: Monitoring and Analyzing Data**

### **5.1 Dashboard Overview**
The dashboard is preconfigured, providing an intuitive interface for real-time monitoring. Both web and mobile dashboards offer the same functionality. Key features include:

- **Device Overview:**

   - Water Status (V12): The device is currently off, as indicated by the text "Device is Off" and the blue indicator.

   - Humidity (V1): Shows a value of 42%, represented by a circular progress bar.

   - Temperature (V2): Displays a temperature of 54°C, with a circular progress bar indicating the temperature within a range of -30 to 70°C.

   - Power (V5): A toggle switch that is currently set to "Off," suggesting the device is not powered on.

   - RSSI (V4): Indicates a signal strength of -74 dBm, which is typical for wireless network devices, showing the signal reception quality.

   - Battery level (V0): Shows a battery level of 66%, indicating the remaining charge of the device.- **Location Map:** Displays the sensor's geographical location

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/blynk-dash-with-data.png)

### **5.2 Settings Overview**

- **Frequency Weighting (V7):** Choose between A and C frequency weightings
- **Fast Time Weighting (V8):** Toggle fast time weighting On/Off
- **Reporting Interval (V9):** Set the data reporting interval (Live, 15 sec, 30 sec, 1 min)
- **Built-In LED (V10):** Toggle the built-in LED On/Off

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/blynk-dash-2-page.png)

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

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/location.png)

   - Enter the sensor's address or drag the pin on the map to the correct location.
   - Save the location.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/location-2.png)

3. **Assign Location in Metadata**:
   - Navigate to device **Info & Metadata** in Blynk.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/location-3.png)

   - Assign the saved location to the device.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/Milesight/Milesight-EM300/Images/location-4.png)


🎥 **Watch the Video Guide**
[![Watch the video](https://img.youtube.com/vi/bOS4-W-gpJE/maxresdefault.jpg)](https://youtu.be/cuykM1xJTrU?feature=shared)

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
   - If only the battery level is displayed or data is missing, go to **TTN** >
   **Your Application** > **Payload Formatters** > **Uplink/Downlink**, update
   the code, and save changes

Uplink:
````

````

---

## **Next Steps**
- Explore the Blynk Web Console and IoT app to monitor sensor data
- Read the Blynk Documentation to learn about Virtual Pins
- [Learn how to Share Devices with other users](https://docs.blynk.io/en/blynk.console/devices/device-sharing)
- Customize the code to meet your specific needs
- Try onboarding the sensor using static tokens

---

**FIXME:** Replace the video URL with the correct link once available.

