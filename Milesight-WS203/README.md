# **Milesight WS302 Sound Level Sensor Blueprint Guide**

The **Milesight WS302 Sound Level Sensor** is designed to monitor and analyze
sound levels in various environments, providing metrics such as instantaneous
sound levels (**LA**), equivalent continuous sound levels (**LAeq**), and
maximum sound levels (**LAmax**). It also monitors battery levels and RSSI for
reliable operation. This guide provides step-by-step instructions to set up the
sensor, register it with **The Things Network (TTN)**, and integrate it with
**Blynk IoT** for data visualization and control.

---

## **Step 1: Unpacking and Initial Setup**

### **1.1 Required Components**
- **Milesight WS302 Sound Level Sensor**.
- **LoRaWAN Gateway** for connectivity.

### **1.2 Initial Setup**
1. **Verify Package Contents**: Ensure all components are included and undamaged.
2. **Install the Batteries**: Insert the batteries into sensor, ensuring correct polarity.
3. **Power On the Sensor**: Hold the power button for 3 seconds. The LED will confirm activation.

---

## **Step 2: Registering the Sensor on The Things Network (TTN)**

### **2.1 Set Up the LoRaWAN Gateway**
1. **Log in to TTN Console**: Visit the [TTN Console](https://console.cloud.thethings.network/).
2. **Add a Gateway**:
   - Navigate to **Gateways** > **Add Gateway**.
   - Enter:
     - **Gateway EUI** (found on the gateway label).
     - **Gateway ID** (e.g., `my-lorawan-gateway`).
     - **Frequency Plan** for your region (see table below).
   - Click **Register Gateway**.
3. **Configure the Gateway**:
   - Follow the manufacturer's instructions to connect the gateway to TTN.
   - Ensure the gateway is online and visible in the TTN Console.
   - *[Insert Screenshot of TTN Gateway Registration Page]*

#### **Frequency Plans**
| **Frequency Plan** | **Region**            | **Description**                                                                 |
|--------------------|-----------------------|---------------------------------------------------------------------------------|
| **EU868**         | Europe                | Operates at 868 MHz. Commonly used across the EU for LoRaWAN deployments.      |
| **US915**         | North America         | Operates at 915 MHz. Used in the United States, Canada, and Mexico.            |
| **AS923**         | Asia-Pacific          | Operates at 923 MHz. Covers countries like Japan, Australia, and New Zealand. |
| **IN865**         | India                 | Operates at 865 MHz. Dedicated plan for deployments in India.                  |
| **AU915**         | Australia/New Zealand | Operates at 915 MHz. Preferred for LoRaWAN in Australia and New Zealand.       |
| **KR920**         | South Korea           | Operates at 920 MHz. Allocated specifically for South Korea.                   |
| **RU864**         | Russia                | Operates at 864 MHz. Used exclusively within Russia.                           |

---

### **2.2 Create a New Application**
1. **Log in to TTN Console**.
2. **Create an Application**:
   - Navigate to **Applications** > **Add Application**.
   - Enter:
     - **Application ID**: A unique name (e.g., `sound-monitor`).
     - **Description**: (e.g., `Monitoring environmental noise levels`).
     - **Handler**: Select the handler for your region.
   - Click **Create Application**.
   - *[Insert Screenshot of TTN Application Creation Page]*

> **Note:** 
> To ensure smooth operation, it’s recommended that all devices within a TTN application are of the same sensor type (e.g., WS302). If you’d like to use different types of sensors, consider creating a separate application for each sensor type. This helps maintain better compatibility and performance.
---

### **2.3 Pair the Sensor with TTN Application**
1. **Open Milesight ToolBox App**:
   - Pair the sensor via NFC.
   - Navigate to **LoRaWAN Settings**.
   - *[Insert Screenshot of LoRaWAN Settings in ToolBox App]*
2. **Configure LoRaWAN Parameters**:
   - **DevEUI**: Use default value (found on the sensor label).
   - **AppEUI**: Use default value from manufacturer or generate a new one.
   - **AppKey**: Use default from the manufacturer or generate a new one.
   - **Frequency Plan**: Match the frequency plan configured in TTN.
   - **RX2 Data Rate**: Set to DR3 (SF9, 125 kHz).
   - **Spreading Factor**: Set to SF10-DR2.
   - **Default Application Port**: Set to 85.
   - Write changes by NFC to the sensor using button **Write**.
---

### **2.5 Configure MQTT Integration in TTN**
1. **Navigate to TTN Integration Settings**:
   - In the TTN Console, go to your application and select **Integrations**.
2. **Select MQTT**:
   - Click **MQTT** from the list of available integrations.
3. **You will need this details for setup integration using MQTT in Blynk**:
   - **Server Address** 
   - **Port**.
   - **Username**
   - **Password**: use button **Generate new API key*** - copy generated key and save it.
   - *[Insert Screenshot of TTN MQTT Configuration Screen]*
   - Navigate **API keys** at the left menu and choose key that you generated in previous step.
   - Into **Rights** choose **Grant individual rights** and select the "View device keys in application", "Create devices in application" and "Edit device keys in application" permissions. To provide this, generate a new API key or edit the existing one that you were using with Blynk.
---

## **Step 3: Integrating TTN with Blynk**

### **3.1 Configuring Blynk Integration via MQTT**
1. Push button **Use Blueprint** at the top of page with blueprint for WS302.

2. **Set Up MQTT in Blynk**:
   - Navigate to **Developer Zone** > **Integrations** > **The Things Stack** -> **Add** and choose template named **Milesight WS302**
   - Enter data from **Step 2**:
     - **Hostname**: MQTT server address with port `1883` or `8883` (e.g., `eu1.cloud.thethings.network:8883`).
     - **Username**: 
     - **Password**: API key that you are copied.
     - Click **Connect**.
     
When status will be **Connected** tap to **Edit** and turn on switch **Enable automated device onboarding**:
  - Specify the LoRaWAN version, Frequency plan, and Regional Parameter Version. Also, enable the "Support class B" and/or "Support class C" checkboxes if required.
  - Specify the addresses of The Things Stack components, which can be found by navigating to Home -> Admin panel -> Network information at the bottom of the page. If you are using The Things Stack Sandbox, all components share the same regional domain like **https://eu1.cloud.thethings.network**.
  - Save changes.

---

## **Step 4: Onboard sensor to Blynk**

1. Go to the **Developer Zone** -> **My Templates** -> choose **Milesight WS302** -> at the right panel you have button **Activate device**
2. In the windows that opened type data that you provide into app for Sensor in step 2.3:

  - Application key 
  - Join EUI 
  - DEV EUI
 
3. Tap **Next** and wait when device come online. 

You can go to the TTN and check that your device create automatically into application and you can add other sensor the same way by creating new dev by using blynk. When device went to online your redirect to the dashboard and you can testing your setup.

## **Step 5: Monitoring and Analyzing Data**

### **4.1 Dashboard Overview**
The dashboard is preconfigured and provides an intuitive interface for monitoring sensor data in real-time. You have the same functional in the web and mobile dashboards. Key features include:

- **Device Overview:**
  - Displays the **Device Name**, **Device Owner**, and **Company Name**.
  - Shows the device's **online/offline status**.
- **Metrics (Virtual Pins):**
  - **Battery (V0):** Displays the battery percentage.
  - **LA (V1):** Instantaneous sound level.
  - **LAeq (V2):** Continuous noise exposure.
  - **LAmax (V3):** Maximum recorded sound level.
  - **RSSI (V6):** Signal strength.
- **Custom Chart:**
  - Provides a time-series visualization of metrics like **LA**, **LAeq**, and **LAmax**.
  - Selectable time intervals (e.g., 1 hour, 6 hours, 1 day).
- **Location Map:**
  - Displays the sensor's geographical location.

---

### **4.2 Setting Alerts**
1. **Log in to Blynk Console**.
2. **Create Automations**:
   - Set triggers for specific thresholds (e.g., `LAeq > 80 dB`).
   - Define actions (e.g., send notifications).
3. **Save and Test**:
   - Simulate events to ensure triggers activate correctly.

---

## **Step 5: Adding Location in Blynk**

### **5.1 Configuring Location**
1. **Go to Location Section in Blynk**:
   - In the Blynk dashboard, navigate to the **Location** section.
2. **Add the Address**:
   - Enter the sensor's address or drag the pin on the map to the correct location.
   - Save the location.
   - *[Insert Screenshot of Location Configuration in Blynk]*
3. **Assign Location in Metadata**:
   - Navigate to **Info & Metadata** in Blynk.
   - Assign the saved location to the device.

---

🎥 **Watch the Video Guide**
[![Watch the video](https://img.youtube.com/vi/bOS4-W-gpJE/maxresdefault.jpg)](https://youtu.be/bOS4-W-gpJE)

## **Next Steps**

## Next Steps

- Explore the Blynk Web Console and IoT app to monitor sensor data.
- Read the Blynk Documentation to learn how to work with Virtual Pins.
- [Discover how to Share Devices with other users](https://docs.blynk.io/en/blynk.console/devices/device-sharing).
- Customize the code to meet your specific needs.
- Try onboard sensor using Static tokens.

## **Error Handling and Troubleshooting**

### **6.1 Common Issues**
1. **Sensor Issues**:
   - Check battery polarity and charge.
   - Reset the sensor if unresponsive.
2. **Connectivity Problems**:
   - Verify LoRaWAN settings and gateway connection.
   - Test MQTT integration.
3. **Data Issues**:
   - Check the payload formatter in TTN. If in dashboards you see only battery level or data gone - please go to the ttn -> your application -./ at the left menu choode payload formaters -> Uplink -> Copy paste code down here and go to the Downlink and do the same and Save changes.
   - Debug MQTT connections with tools like MQTT Explorer.

This comprehensive guide ensures seamless integration of the Milesight WS302 Sound Level Sensor with TTN and Blynk for robust environmental noise monitoring.


### FIXME: fix the video url
[![Watch the video](https://i.sstatic.net/Vp2cE.png)](https://youtu.be/cuykM1xJTrU?feature=shared)


