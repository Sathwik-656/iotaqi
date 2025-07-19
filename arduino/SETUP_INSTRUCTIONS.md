# 🚀 ESP32 Setup Instructions - Fixed Version

## ✅ **Problem Fixed!**
The compilation error was caused by having duplicate files in the same folder. I've cleaned up the code and removed the duplicate file.

## 📝 **Before Uploading - Update These Settings:**

### 1. **WiFi Configuration (Lines 18-19):**
```cpp
const char* ssid = "YOUR_WIFI_SSID";        // Replace with your WiFi network name
const char* password = "YOUR_WIFI_PASSWORD"; // Replace with your WiFi password
```

**Change to your actual WiFi:**
```cpp
const char* ssid = "353";                    // Your WiFi name
const char* password = "Dipenn353";          // Your WiFi password
```

### 2. **Device Configuration (Lines 38-39):**
```cpp
const String DEVICE_ID = "Esp_353";              // Unique device identifier
const String DEVICE_LOCATION = "Smart Home Monitor"; // Device location description
```

**Customize as needed:**
```cpp
const String DEVICE_ID = "esp32_kitchen";        // Make it unique
const String DEVICE_LOCATION = "Kitchen Table";  // Your actual location
```

## 🔧 **Hardware Connections:**
```
ESP32 Pin    →    Sensor
-----------       --------
GPIO 34      →    MQ135 AO (Air Quality)
GPIO 39      →    MQ6 AO (LPG Gas)
GPIO 4       →    DHT11 Data
3.3V/5V      →    All VCC pins
GND          →    All GND pins
```

## 📤 **Upload Steps:**

1. **Close Arduino IDE completely**
2. **Open only the main file:** `esp32_sensor_code.ino`
3. **Update WiFi credentials** (lines 18-19)
4. **Select Board:** ESP32 Dev Module
5. **Select Port:** Your ESP32 port
6. **Click Upload**

## 📊 **Expected Serial Output:**
```
ESP32 IoT Air Quality Monitor Starting...
Connecting to WiFi.....
WiFi connected!
IP address: 192.168.1.100
Warming up sensors for 2 minutes...
ESP32 IoT Air Quality Monitor Ready!

========================
📊 SENSOR READINGS
========================
🌡️  Temperature: 24.5°C
💧 Humidity: 55%
🌬️  AQI: 45
🏭 CO2: 420 ppm
🌫️  PM2.5: 12.5 μg/m³
☁️  VOC: 0.30 mg/m³
⚠️  CO: 5 ppm
🔴 NO2: 25 ppb
🔥 LPG Level: 250 ppm
📶 WiFi Signal: -45 dBm
========================

✓ Data sent to Firebase successfully
✓ Data sent to Dashboard successfully
```

## 🔍 **Troubleshooting:**

### **If you get compilation errors:**
1. Make sure only ONE .ino file is in the folder
2. Close and reopen Arduino IDE
3. Check that all libraries are installed:
   - ArduinoJson
   - DHT sensor library
   - Adafruit Unified Sensor

### **If WiFi doesn't connect:**
1. Double-check SSID and password
2. Ensure you're using 2.4GHz network (not 5GHz)
3. Check signal strength

### **If sensors read 0 or NaN:**
1. Verify wiring connections
2. Check power supply (5V for MQ sensors)
3. Wait for 2+ minute warm-up period

## 🎯 **What the Code Does:**

1. **Connects to WiFi** and syncs time
2. **Warms up sensors** for 2 minutes (important!)
3. **Reads sensors every 30 seconds:**
   - DHT11: Temperature & Humidity
   - MQ135: Air quality, CO2, PM2.5, VOC, CO, NO2
   - MQ6: LPG/Butane gas levels
4. **Uploads data to Firebase** for storage
5. **Sends to dashboard API** for visualization
6. **Displays readings** on Serial Monitor

## ✅ **Ready to Upload!**
The code is now clean and ready to upload. Just update your WiFi credentials and you're good to go!

---
**Need help?** Check the serial monitor output for debugging information.