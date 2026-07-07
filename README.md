# 🌱 ESP32 NPK Soil Sensor Dashboard

**IoT-Based Agriculture Monitoring System**  
*Developed by Agricultural Engineering Students | NIMS University Rajasthan, Jaipur*

![Status](https://img.shields.io/badge/Status-Active-green)
![Platform](https://img.shields.io/badge/Microcontroller-ESP32-blue)
![Protocol](https://img.shields.io/badge/Communication-Modbus%20RTU-yellow)

## 📖 About This Project

This project implements a real-time **NPK (Nitrogen, Phosphorous, Potassium)** soil nutrient monitoring system using the ESP32 microcontroller. 

The system communicates with an external NPK analog/digital sensor via RS485 (Modbus protocol), processes the data, displays the values on a local **OLED Screen**, and hosts a responsive **Web Server**. Users can access live sensor readings through any smartphone or laptop connected to the device's Wi-Fi network.

### ✨ Features
- 🖥️ **Local OLED Display**: Real-time visualization of N, P, and K values.
- 🌐 **Wi-Fi Web Server**: Create an Access Point (AP) to view data on a custom dashboard without internet.
- ⚡ **Fast Refresh Rate**: Polls sensor data every few seconds automatically.
- 💻 **Easy Configuration**: Customize SSID and Password directly in code.
- 🔧 **Modbus Support**: Uses SoftwareSerial to communicate with industrial-grade sensors.

---

## 🛠️ Hardware Requirements

| Component | Quantity | Description |
| :--- | :---: | :--- |
| **Microcontroller** | 1 | ESP32 DevKit V1 / ESP32-WROOM-32 |
| **Sensor** | 1 | NPK Soil Sensor (RS485 / Modbus Compatible) |
| **Display** | 1 | 0.96 inch OLED (SSD1306 Driver, SPI Mode) |
| **Power** | 1 | 5V USB Power Supply / Li-Po Battery |
| **Connections** | Multi | Jumper Wires, Breadboard |

---

## 🔌 Wiring Connections

Configure your ESP32 according to the following pin mapping used in the firmware:

### OLED Display (SPI Mode)
| OLED Pin | ESP32 GPIO |
| :--- | :--- |
| **DC (Data/Command)** | GPIO 16 |
| **CS (Chip Select)** | GPIO 5 |
| **MOSI (Master Out)** | GPIO 23 |
| **SCK (Clock)** | GPIO 18 |
| **RES (Reset)** | GPIO 17 |

### NPK Sensor (RS485 / Modbus)
| Function | ESP32 GPIO | Notes |
| :--- | :--- | :--- |
| **TXD (Sensor)** | GPIO 27 | Connected to `SoftwareSerial_RX` |
| **RXD (Sensor)** | GPIO 26 | Connected to `SoftwareSerial_TX` |
| **DE (Driver Enable)** | GPIO 33 | Set HIGH to Send, LOW to Receive |
| **RE (Receiver Enable)** | GPIO 32 | Set HIGH to Send, LOW to Receive |
| *(Note)* | *Ensure A/B lines match RS485 standard* | *Verify sensor polarity* |

---

## 📦 Software Dependencies

To run this code successfully, you must install the following libraries via the **Arduino Library Manager**:

1.  **Adafruit GFX Library**
2.  **Adafruit SSD1306**
3.  **ESP32** Core (Board Manager URL: `https://dl.espressif.com/dl/package_esp32_index.json`)
4.  **SPI** (Built-in)
5.  **Wire** (Built-in)
6.  **WiFi** (Built-in)
7.  **WebServer** (Built-in)

---

## 🚀 Getting Started

### 1. Flash the Firmware
1.  Open the project in the **Arduino IDE**.
2.  Connect the ESP32 via USB.
3.  Select **Tools > Board > ESP32 Arduino**.
4.  Update the Wi-Fi Credentials in the code (see *Configuration*).
5.  Click **Upload**.

### 2. Configure Network Settings
Open the file and modify these lines to set up your Wi-Fi Access Point:

```cpp
// Replace with your desired network name and password
const char* ssid = "ESP_npk_sensor";
const char* password = "123456789";
```

### 3. Run the Project
Power on the ESP32.
Connect to the Wi-Fi network ESP_npk_sensor with password 123456789.
Open a browser and enter the displayed IP address (printed in Serial Monitor).
Default IP: 192.168.4.1 (Check Serial Monitor upon boot).
View the dashboard showing Nitrogen, Phosphorous, and Potassium values.

### 💻 Key Functions Explanation
nitrogen() / phosphorous() / potassium():
Send specific Modbus request codes to the sensor and retrieve 1-byte values representing mg/kg concentration.
setup():
Initializes the Wi-Fi Access Point, starts the Web Server, calibrates the OLED screen, and shows a startup splash screen.
loop():
Continuously polls the sensor three times (one per nutrient), updates the serial logs, refreshes the OLED display, and handles incoming web requests.
handleRoot():
Serves the custom HTML/CSS page containing the dashboard UI when the root URL is accessed.

### 🧠 Limitations & Troubleshooting
Sensor Communication: If values read as 0 or strange hex numbers, ensure the A/B (485) wires are not swapped and power supply (5V) is stable.
OLED Not Showing: Verify SPI pins (23, 18, 5, 16, 17) are correct for your specific board. Some ESP32 modules require GND toggling for reset.
IP Address: If the IP does not appear in Serial, try resetting the board. It prints AP IP address: at startup.

### 🏆 Credits
Developed for Academic Purposes.

Developer: Priyanshu Kumar
Institution: NIMS University Rajasthan, Jaipur
Department: Agricultural Engineering
Year: 2024

### 📜 License
This project is open-source software. See the LICENSE file in the root directory for details.

If you find this helpful for your agricultural engineering projects, please give a ⭐ Star to support the team!</strong> </p> ```

### 💡 Pro-Tips for You:
Library Manager: When uploading, make sure to tell your IDE to auto-install the missing libraries if they aren't there yet.
Voltage: The NPK sensors often require 5V to drive the RS485 chip properly. Ensure your ESP32 5V pin (Vin) is being used for the sensor power if possible.
HTML Images: In the handleRoot() function, there is a reference to .sensor img { width: 100px... }, but no actual images are included in the HTML. I added comments in the README suggesting where to add icons if you want to upload assets later.
Security: Since this runs in SoftAP mode (Access Point), anyone nearby can potentially connect to it. For production use, you might switch to Station Client mode (connecting to a home router) later.
