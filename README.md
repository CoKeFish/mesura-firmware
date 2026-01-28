# Mesura - IoT biometric sensors project

Mesura is an IoT project for the Internet of Things class that implements biometric sensors (heart rate and galvanic skin response) using different platforms: Arduino, ESP32, and Raspberry Pi.

This repository contains the **hardware/firmware** component of the Mesura system. The sensor data collected here is transmitted to the [Mesura Web](https://github.com/CoKeFish/mesura-web) application for real-time visualization and monitoring.

## Available Sensors

| Sensor | Platform | Description | Location |
|--------|----------|-------------|----------|
| **Pulse Sensor** | Arduino | Basic pulse reading with Serial Plotter visualization | `pulse-sensor/arduino/basic-pulse/` |
| **Pulse Sensor** | ESP32 | Pulse sensor using I2C protocol | `pulse-sensor/esp32/esp32-i2c-pulse/` |
| **Pulse Sensor** | Raspberry Pi | Advanced BPM reading using MCP3008 ADC | `pulse-sensor/raspberry-pi/` |
| **GSR Sensor** | Arduino | Skin conductivity reading with averaged measurements | `gsr-sensor/arduino/basic-gsr/` |
| **GSR Sensor** | ESP32 | GSR reading for ESP32 | `gsr-sensor/esp32/esp32-gsr/` |

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

### MCP3008
- 8-channel, 10-bit Analog-to-Digital Converter
- SPI Interface
- [MCP3008 Datasheet](https://www.microchip.com/wwwproducts/en/MCP3008)

## Author

**Rodion Romanovich Tabares Correa**

IoT Class Project - 2022

---

*Note: This is the hardware/firmware component. For the complete system including web interface and music recommendations, see the mesura-web repository.*
