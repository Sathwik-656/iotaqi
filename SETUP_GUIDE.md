# ESP32 IoT Air Quality Monitor - Complete Setup Guide

## 🔧 Hardware Setup

### Required Components:
1. **ESP32 Development Board** (ESP32-DevKit or similar)
2. **MQ135 Air Quality Sensor**
3. **MQ6 LPG/Butane Gas Sensor** 
4. **DHT11 Temperature & Humidity Sensor**
5. **Breadboard and Jumper Wires**
6. **5V Power Supply** (for MQ sensors)

### Pin Connections:

```
ESP32 Pin    →    Sensor Connection
-----------       ------------------
3.3V/5V      →    MQ135 VCC, MQ6 VCC, DHT11 VCC
GND          →    MQ135 GND, MQ6 GND, DHT11 GND
GPIO 34      →    MQ135 Analog Output (AO)
GPIO 39      →    MQ6 Analog Output (AO)
GPIO 4       →    DHT11 Data Pin
```

### Detailed Wiring Diagram:
```
ESP32                    MQ135 Sensor
-----                    ------------
3.3V/5V  ────────────────  VCC
GND      ────────────────  GND
GPIO 34  ────────────────  AO (Analog Output)
                          DO (Not Connected)

ESP32                    MQ6 Sensor
-----                    -----------
3.3V/5V  ────────────────  VCC
GND      ────────────────  GND
GPIO 39  ────────────────  AO (Analog Output)
                          DO (Not Connected)

ESP32                    DHT11 Sensor
-----                    ------------
3.3V     ────────────────  VCC
GND      ────────────────  GND
GPIO 4   ────────────────  DATA
```

## 💻 Software Setup

### 1. Arduino IDE Configuration

1. **Install ESP32 Board Package:**
   - Open Arduino IDE
   - Go to File → Preferences
   - Add this URL to "Additional Board Manager URLs":
     ```
     https://dl.espressif.com/dl/package_esp32_index.json
     ```
   - Go to Tools → Board → Boards Manager
   - Search for "ESP32" and install "ESP32 by Espressif Systems"

2. **Install Required Libraries:**
   ```
   - ArduinoJson (by Benoit Blanchon)
   - DHT sensor library (by Adafruit)
   - Adafruit Unified Sensor (dependency)
   ```

### 2. Code Configuration

1. **Update WiFi Credentials:**
   ```cpp
   const char* ssid = "YOUR_WIFI_SSID";
   const char* password = "YOUR_WIFI_PASSWORD";
   ```

2. **Configure Device Settings:**
   ```cpp
   const String DEVICE_ID = "Esp_353";  // Change to unique ID
   const String DEVICE_LOCATION = "Smart Home Monitor";  // Your location
   ```

3. **Firebase Configuration (Optional):**
   ```cpp
   const char* FIREBASE_HOST = "https://iot-aqi-353-default-rtdb.firebaseio.com";
   ```

4. **Dashboard API (Optional):**
   ```cpp
   const char* DASHBOARD_API = "https://your-domain.com/api/sensors";
   ```

## 🔥 Firebase Setup

### 1. Create Firebase Project:
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Create a project"
3. Enter project name: `iot-aqi-monitor`
4. Enable Google Analytics (optional)

### 2. Setup Realtime Database:
1. In Firebase Console, go to "Realtime Database"
2. Click "Create Database"
3. Choose "Start in test mode" for development
4. Select your preferred location

### 3. Configure Database Rules:
```json
{
  "rules": {
    "sensors": {
      ".read": true,
      ".write": true
    }
  }
}
```

### 4. Get Configuration:
1. Go to Project Settings (gear icon)
2. Scroll to "Your apps" section
3. Click "Web" icon to add web app
4. Copy the configuration values

## 📊 Data Structure

### Firebase Data Structure:
```
sensors/
├── Esp_353/
│   ├── Esp_353_1640995200000/
│   │   ├── deviceId: "Esp_353"
│   │   ├── aqi: 45
│   │   ├── co2: 420
│   │   ├── pm25: 12.5
│   │   ├── voc: 0.3
│   │   ├── co: 5
│   │   ├── no2: 25
│   │   ├── temperature: 24.5
│   │   ├── humidity: 55
│   │   ├── lpgLevel: 250
│   │   ├── location: "Smart Home Monitor"
│   │   ├── signalStrength: -45
│   │   └── timestamp: "2023-12-31T12:00:00.000Z"
│   └── ...
└── ...
```

## 🚀 Upload and Test

### 1. Upload Code:
1. Select Board: "ESP32 Dev Module"
2. Select Port: Your ESP32 port
3. Click Upload
4. Open Serial Monitor (115200 baud)

### 2. Expected Serial Output:
```
ESP32 IoT Air Quality Monitor Starting...
Connecting to WiFi.....
WiFi connected!
IP address: 192.168.1.100
Signal strength: -45 dBm
Setup complete!

========================
📊 SENSOR READINGS
========================
🌡️  Temperature: 24.5°C
💧 Humidity: 55.0%
🌬️  AQI: 45
🏭 CO2: 420 ppm
🌫️  PM2.5: 12.5 μg/m³
☁️  VOC: 0.30 mg/m³
⚠️  CO: 5 ppm
🔴 NO2: 25 ppb
🔥 LPG Level: 250 ppm
📶 WiFi Signal: -45 dBm
⏰ Timestamp: 2023-12-31T12:00:00.000Z
========================

✓ Data sent to Firebase successfully
✓ Data sent to Dashboard successfully
```

## 🔍 Troubleshooting

### Common Issues:

1. **WiFi Connection Failed:**
   - Check SSID and password
   - Ensure 2.4GHz network (ESP32 doesn't support 5GHz)
   - Check signal strength

2. **Sensor Reading Errors:**
   - Verify wiring connections
   - Check power supply (5V for MQ sensors)
   - Allow 2+ minutes warm-up time for MQ sensors

3. **Firebase Upload Failed:**
   - Check Firebase URL
   - Verify database rules allow write access
   - Check internet connection

4. **Invalid Sensor Readings:**
   - DHT11 returns NaN: Check wiring and power
   - MQ sensors return 0: Check analog pin connections
   - Calibrate sensors based on your environment

### Sensor Calibration:

1. **MQ135 Calibration:**
   - Place in clean air for 24 hours
   - Adjust `MQ135_CLEAN_AIR_RATIO` if needed

2. **MQ6 Calibration:**
   - Place in clean air (no LPG/gas)
   - Adjust `MQ6_CLEAN_AIR_RATIO` if needed

## 📈 Monitoring Data

### View Data in Firebase:
1. Go to Firebase Console
2. Navigate to Realtime Database
3. Browse to `sensors/[your-device-id]`

### API Endpoints:
- **GET** `/api/sensors` - Get all sensor data
- **GET** `/api/sensors?deviceId=Esp_353` - Get specific device data
- **POST** `/api/sensors` - Submit new sensor data

### Dashboard Features:
- Real-time sensor readings
- Historical data charts
- Device status monitoring
- Air quality alerts

## 🔒 Security Notes

1. **Production Firebase Rules:**
   ```json
   {
     "rules": {
       "sensors": {
         ".read": "auth != null",
         ".write": "auth != null"
       }
     }
   }
   ```

2. **API Authentication:**
   - Add API keys for production
   - Implement rate limiting
   - Use HTTPS endpoints

## 📞 Support

If you encounter issues:
1. Check serial monitor output
2. Verify all connections
3. Test individual sensors
4. Check Firebase console for data
5. Review this guide for missed steps

Happy monitoring! 🌱