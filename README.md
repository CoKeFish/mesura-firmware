# Mesura - IoT Biometric Sensors Project

Mesura is an IoT project for the Internet of Things class that implements biometric sensors (heart rate and galvanic skin response) using different platforms: Arduino, ESP32, and Raspberry Pi.

This repository contains the **hardware/firmware** component of the Mesura system. The sensor data collected here is transmitted to the [Mesura Web](https://github.com/CoKeFish/mesura-web) application for real-time visualization and monitoring.

## Repository Structure

```
mesura-firmware/
├── pulse-sensor/              # Heart rate sensor
│   ├── arduino/              # Arduino implementation
│   │   └── basic-pulse/
│   │       └── basic-pulse.ino
│   ├── esp32/                # ESP32 implementation
│   │   └── esp32-i2c-pulse/
│   │       └── esp32-i2c-pulse.ino
│   └── raspberry-pi/         # Raspberry Pi implementation
│       └── pulsesensor.py
│
├── gsr-sensor/               # GSR (Galvanic Skin Response) sensor
│   ├── arduino/              # Arduino implementation
│   │   └── basic-gsr/
│   │       └── basic-gsr.ino
│   └── esp32/                # ESP32 implementation
│       └── esp32-gsr/
│           └── esp32-gsr.ino
│
├── shared/                   # Shared code between sensors
│   └── raspberry-pi/
│       └── MCP3008.py       # Class to read MCP3008 ADC via SPI
│
└── README.md                 # This file
```

## Available Sensors

| Sensor | Platform | Description | Location |
|--------|----------|-------------|----------|
| **Pulse Sensor** | Arduino | Basic pulse reading with Serial Plotter visualization | `pulse-sensor/arduino/basic-pulse/` |
| **Pulse Sensor** | ESP32 | Pulse sensor using I2C protocol | `pulse-sensor/esp32/esp32-i2c-pulse/` |
| **Pulse Sensor** | Raspberry Pi | Advanced BPM reading using MCP3008 ADC | `pulse-sensor/raspberry-pi/` |
| **GSR Sensor** | Arduino | Skin conductivity reading with averaged measurements | `gsr-sensor/arduino/basic-gsr/` |
| **GSR Sensor** | ESP32 | GSR reading for ESP32 | `gsr-sensor/esp32/esp32-gsr/` |

## Platform Requirements

### Arduino
- **IDE**: Arduino IDE 1.8.x or higher
- **Libraries**: No additional libraries required
- **Hardware**: 
  - Arduino UNO or compatible
  - PulseSensor.com pulse sensor
  - GSR Sensor (Grove GSR Sensor or similar)

### ESP32
- **IDE**: Arduino IDE with ESP32 support or PlatformIO
- **Libraries**: Wire.h (included)
- **Hardware**:
  - ESP32 DevKit or compatible
  - Pulse sensor with I2C interface
  - ESP32-compatible GSR sensor

### Raspberry Pi
- **Operating System**: Raspbian/Raspberry Pi OS
- **Python**: Python 3.x
- **Dependencies**:
  ```bash
  # Enable SPI (sudo raspi-config)
  sudo apt-get update 
  sudo apt-get upgrade
  sudo apt-get install python3-dev python3-pip
  
  # Install spidev
  wget https://github.com/doceme/py-spidev/archive/master.zip 
  unzip master.zip
  cd py-spidev-master
  sudo python3 setup.py install
  ```
- **Hardware**:
  - Raspberry Pi (any model with GPIO)
  - MCP3008 ADC (Analog-to-Digital Converter)
  - Analog pulse sensor

## Quick Start Guide

### Pulse Sensor - Arduino

1. Connect the pulse sensor to analog pin A0
2. Open `pulse-sensor/arduino/basic-pulse/basic-pulse.ino` in Arduino IDE
3. Select your board and port
4. Upload the sketch
5. Open the Serial Plotter (Tools > Serial Plotter) to visualize the signal
6. The built-in LED will blink with each heartbeat

### Pulse Sensor - ESP32

1. Connect the pulse sensor to the ESP32 I2C bus (address 0xA0)
2. Open `pulse-sensor/esp32/esp32-i2c-pulse/esp32-i2c-pulse.ino`
3. Configure ESP32 in Arduino IDE
4. Upload the sketch
5. Open the Serial Monitor at 9600 baud to see BPM values

### Pulse Sensor - Raspberry Pi

1. Connect the MCP3008 to Raspberry Pi via SPI:
   - VDD → 3.3V
   - VREF → 3.3V
   - AGND → GND
   - DGND → GND
   - CLK → GPIO 11 (SCLK)
   - DOUT → GPIO 9 (MISO)
   - DIN → GPIO 10 (MOSI)
   - CS → GPIO 8 (CE0)
2. Connect the pulse sensor to an MCP3008 channel (CH0 by default)
3. Copy `MCP3008.py` and `pulsesensor.py` to the Raspberry Pi
4. Run:
   ```python
   from pulsesensor import Pulsesensor
   
   ps = Pulsesensor(channel=0)
   ps.startAsyncBPM()
   
   try:
       while True:
           print(f"BPM: {ps.BPM}")
           time.sleep(1)
   except KeyboardInterrupt:
       ps.stopAsyncBPM()
   ```

### GSR Sensor - Arduino

1. Connect the GSR sensor to analog pin A0
2. Open `gsr-sensor/arduino/basic-gsr/basic-gsr.ino`
3. Upload the sketch to your Arduino
4. Open the Serial Monitor at 9600 baud
5. Place your fingers on the GSR sensor electrodes
6. Observe the conductivity values (higher values = greater conductivity/sweating)

### GSR Sensor - ESP32

1. Connect the GSR sensor to an ESP32 analog pin (pin 36 by default)
2. Open `gsr-sensor/esp32/esp32-gsr/esp32-gsr.ino`
3. Adjust the pin in the code if necessary
4. Upload the sketch
5. Open the Serial Monitor at 9600 baud to see the readings

## Hardware Connections

### Pulse Sensor (Arduino/Analog)
- **PURPLE Wire** → Analog Pin A0
- **RED Wire** → 5V (or 3.3V)
- **BLACK Wire** → GND

### GSR Sensor (Arduino/ESP32)
- **Signal Pin** → Analog Pin A0 (Arduino) or GPIO 36 (ESP32)
- **VCC** → 5V (Arduino) or 3.3V (ESP32)
- **GND** → GND

### MCP3008 (Raspberry Pi)
| MCP3008 Pin | Raspberry Pi Pin |
|-------------|------------------|
| VDD         | 3.3V (Pin 1)     |
| VREF        | 3.3V (Pin 1)     |
| AGND        | GND (Pin 6)      |
| DGND        | GND (Pin 6)      |
| CLK         | GPIO 11 (SCLK)   |
| DOUT        | GPIO 9 (MISO)    |
| DIN         | GPIO 10 (MOSI)   |
| CS/SHDN     | GPIO 8 (CE0)     |

## References and Resources

### Pulse Sensor
- [Official PulseSensor Tutorial](https://pulsesensor.com/pages/code-and-guide)
- [Video Tutorial](https://www.youtube.com/watch?v=RbB8NSRa5X4)
- [Original Arduino Repository](https://github.com/WorldFamousElectronics/PulseSensor_Amped_Arduino)

### GSR Sensor
- Based on the Grove GSR Sensor
- Measures the electrical resistance of the skin
- Useful for detecting stress, emotional arousal, and physiological responses

### MCP3008
- 8-channel, 10-bit Analog-to-Digital Converter
- SPI Interface
- [MCP3008 Datasheet](https://www.microchip.com/wwwproducts/en/MCP3008)

## Project Architecture

Mesura consists of two main components:

1. **[Mesura Firmware](https://github.com/CoKeFish/mesura-firmware)** (this repository) - Hardware and sensor implementations for Arduino, ESP32, and Raspberry Pi
2. **[Mesura Web](https://github.com/CoKeFish/mesura-web)** - Web application for real-time data visualization and monitoring

The firmware collects biometric data from the sensors and transmits it to the web application, where users can view and analyze the readings in real-time.

## License

This project was developed as part of an IoT course.

## Author

**Rodion Romanovich Tabares Correa**

IoT Class Project - 2024
