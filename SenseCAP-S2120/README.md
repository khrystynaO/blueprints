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

![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/gateway.png)

### **2.2 Creating an Application**
1. Navigate to **Applications** > **Add Application**.
2. Fill out:
   - **Application ID**: `sensecap-8-in-1-weather-station-1`
   - **Description**: e.g., `Outdoor environmental monitoring`
   - **Handler**: Choose the regional handler
3. Click **Create Application**.

![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/create-form.png)

> **Note:**
> It is recommended to keep all devices in one application the same model. For mixed models, use separate applications to ensure optimal decoding and compatibility.

![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/ttn-dash.png)

### **2.3 Adding the Device to TTN**
1. Use the **SenseCAP Mate App** or **SenseCAP Console**.
2. Connect to the sensor via **Bluetooth**.
3. Navigate to **LoRaWAN Settings**, and input:
   - **DevEUI**, **AppEUI**, **AppKey** (from sensor label or generated)
   - **Frequency Plan**: Match the gateway region
   - **LoRaWAN Version**: 1.0.3
   - **Class Type**: Typically **Class A**
   - **RX2 Data Rate**: DR3
   - **Spreading Factor**: SF10-DR2
   - **Default Application Port**: 85
4. Save and apply settings.

![App](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/mobile-2.png)

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/mobile.png)

---

## **Step 3: Configure MQTT in TTN for Blynk**
1. In the TTN Console, go to your Application > **Integrations** > **MQTT**.
2. Click **Generate new API Key**.
3. Save the following:
   - **Server**: `eu1.cloud.thethings.network`
   - **Port**: `8883`
   - **Username**: Application ID
   - **Password**: Generated API key
4. In **API Keys**, click the generated key and ensure the following permissions:
   - View device keys
   - Create/edit devices
   - View uplink messages

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/integarion-mqtt-key.png)

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/edit-permition.png)

---

## **Step 4: Integrating TTN with Blynk**

### **4.1 Add Integration in Blynk**
1. Log in to Blynk Console.
2. Go to **Developer Zone** > **Integrations** > **The Things Stack** > **Add**.
3. Choose the template **SenseCAP S2120**.
4. Enter:
   - **Hostname**: `eu1.cloud.thethings.network`
   - **Port**: `8883`
   - **Username**: TTN Application ID
   - **Password**: MQTT API Key
5. Click **Connect**.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/connect.png)

6. Enable **Automated Device Onboarding** and specify:
   - LoRaWAN version
   - Frequency plan
   - Regional parameters

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/advanced-complete.png)

### **4.2 Activate the Device in Blynk**
1. Go to **My Templates** > **SenseCAP S2120** > **Activate Device**.
2. Enter:
   - **Join EUI**, **DevEUI**, **App Key**

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/sample-data.png)

3. Click **Next**, wait until the device shows as online.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/wait-for-dev.png)

---

## **Step 5: Monitoring Data in Blynk**

### **5.1 Dashboard Overview**

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/img_1.png)

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

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/img_2.png)

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

## **Step 7: Location Assignment**

1. In Blynk Dashboard, navigate to **Location** widget.
2. Add your device's physical location:
   - Enter address or drag the pin on the map

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/location.png)

3. Save changes.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/location-2.png)

4. Go to **Device Metadata**, assign saved location.

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/location-3.png)

![Key](https://raw.githubusercontent.com/khrystynaO/blueprints/refs/heads/khrystynaO/feature/SenseCAPS2120/SenseCAPS2120/Images/location-4.png)

🎥 **Watch the Video Guide for setup LoraWAN device in Blynk**
[![Watch the video](https://img.youtube.com/vi/YbFF-Iec38w/maxresdefault.jpg)](https://youtu.be/YbFF-Iec38w)

---

## **Step 8: Troubleshooting**

### **Common Issues**
- **No Data**:
   - Check TTN application status and device join info
   - Ensure payload formatter is applied
- **Battery Drain**:
   - Reduce reporting interval
- **Incorrect Readings**:
   - Check sensor calibration

### **Payload Formatter (Example)**
```js
function decodeUplink(input) {
  var bytes = input.bytes;
  return {
    data: {
      temperature: bytes[0] / 10,
      humidity: bytes[1] / 2,
      pressure: ((bytes[2] << 8) + bytes[3]) / 10,
      windSpeed: bytes[4] / 10,
      windDirection: (bytes[5] << 8) + bytes[6],
      rainfall: bytes[7] / 10,
      solarRadiation: (bytes[8] << 8) + bytes[9],
      uvIndex: bytes[10] / 10,
      battery: bytes[11],
    }
  };
}
```
> Use SenseCAP official payload docs for full decoding structure.

---

## **Next Steps**
- Customize dashboards per project needs
- Invite users to view or control devices
- Explore [Blynk Documentation](https://docs.blynk.io)
- Try static token provisioning for large-scale deployments


