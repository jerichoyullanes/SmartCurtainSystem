# Smart Curtain System

An ESP32-based embedded system for my capstone project titled **"Android-Based Renewable Energy Powered Smart Curtain with Automatic Light and Temperature Sensor"**. This system automates curtain movement based on ambient light and temperature readings, while also allowing full manual control through physical push buttons or an Android app over Wi-Fi.

---

## Features

- **Dual Control Modes** – Switch between Automatic and Manual modes using a physical button or the Android app
- **Automatic Mode** – Opens or closes the curtain automatically based on the ambient light level (LDR sensor)
- **Manual Mode** – Control the curtain directly using physical push buttons or the Android app
- **Android App Control** – Connect to the ESP32's Wi-Fi hotspot and send open/close/switch commands via HTTP
- **Temperature & Humidity Monitoring** – Reads and logs data from a DHT11 sensor in real time
- **Safety Limits** – Uses limit switches to stop the motor at fully open/closed positions, and an ultrasonic sensor to prevent over-closing
- **Mode Indicator LEDs** – Red LED for Manual mode, Green LED for Automatic mode

---

## Hardware Components

| Component | Description |
|---|---|
| ESP32 | Main microcontroller with built-in Wi-Fi |
| LDR (Photoresistor) | Detects ambient light level |
| DHT11 | Measures temperature and humidity |
| 12V DC Motor + L298N Driver | Drives the curtain open and closed |
| HC-SR04 Ultrasonic Sensor | Measures distance to prevent over-closing |
| Limit Switch × 2 | Detects fully open curtain position |
| Push Button (Blue) | Open curtain manually |
| Push Button (Yellow) | Close curtain manually |
| Push Button (Red) | Toggle between Manual and Automatic mode |
| Red LED | Indicates Manual mode |
| Green LED | Indicates Automatic mode |

---

## Pin Configuration

| Pin | Component | Role |
|---|---|---|
| GPIO 32 | LDR | Analog light reading |
| GPIO 33 | DHT11 | Temperature & humidity data |
| GPIO 0 | L298N IN1 | Motor direction control |
| GPIO 2 | L298N IN2 | Motor direction control |
| GPIO 4 | L298N ENA | PWM motor speed control |
| GPIO 13 | HC-SR04 Trig | Ultrasonic trigger |
| GPIO 14 | HC-SR04 Echo | Ultrasonic echo |
| GPIO 34 | Limit Switch 1 | Open-end stop |
| GPIO 35 | Limit Switch 2 | Open-end stop |
| GPIO 25 | Blue Button | Manual open |
| GPIO 26 | Yellow Button | Manual close |
| GPIO 27 | Red Button | Mode toggle |
| GPIO 16 | Red LED | Manual mode indicator |
| GPIO 17 | Green LED | Automatic mode indicator |

---

## Libraries Used

- [`WiFi.h`](https://github.com/espressif/arduino-esp32/tree/master/libraries/WiFi) – ESP32 Wi-Fi (Access Point mode)
- [`WiFiClient.h`](https://github.com/espressif/arduino-esp32/tree/master/libraries/WiFi) – HTTP client communication
- [`dht11`](https://playground.arduino.cc/Main/DHT11Lib/) – DHT11 temperature & humidity sensor
- [`OneButton`](https://github.com/mathertel/OneButton) – Multi-function push button handling (included)

---

## How It Works

### Wi-Fi / Android App Connection

The ESP32 creates its own Wi-Fi Access Point:

- **SSID:** `SmartCurtain`
- **Password:** `12345678`
- **Server Port:** `80`

Connect your Android device to this hotspot, then send HTTP GET requests to control the curtain:

| HTTP Command | Action |
|---|---|
| `GET /open` | Opens the curtain |
| `GET /close` | Closes the curtain |
| `GET /switch` | Toggles between Manual and Automatic mode |

### Automatic Mode

When Automatic mode is active, the system continuously reads the LDR value:
- If **LDR value > 3000** (bright) → **Opens** the curtain
- If **LDR value ≤ 3000** (dark) → **Closes** the curtain

### Manual Mode

When Manual mode is active:
- Press the **Blue Button** (GPIO 25) to **open** the curtain
- Press the **Yellow Button** (GPIO 26) to **close** the curtain
- Release both buttons to stop the motor

### Safety Mechanisms

- **Limit Switches** (GPIO 34 & 35): When both switches are LOW (triggered), the motor stops to prevent the curtain from over-opening
- **Ultrasonic Sensor** (HC-SR04): When the measured distance is less than 20 cm, the motor stops to prevent over-closing

---

## Circuit Diagram

The Fritzing schematic file is included in the repository:

```
ESP32 Version Final (12v DC Motor Version) .fzz
```

Open it with [Fritzing](https://fritzing.org/) to view the full wiring diagram.

---

## Setup & Upload

1. Install the [Arduino IDE](https://www.arduino.cc/en/software) and add ESP32 board support
2. Install the required libraries via the Arduino Library Manager:
   - `dht11`
   - `OneButton`
3. Open `smart_curtain_system/smart_curtain_system.ino` in the Arduino IDE
4. Select your ESP32 board and the correct COM port
5. Upload the sketch to your ESP32

---

## License

Copyright © 2023 Jericho Yu Llanes. All rights reserved.  
This project is proprietary. See the [LICENSE](LICENSE) file for full details.
