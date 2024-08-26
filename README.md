# Agricultural Monitoring with Microchip LoRa Technology MOTE (RN2483)

## Table of Contents
- [Introduction](#introduction)
- [Components](#components)
- [Setup](#setup)
- [Installation](#installation)
- [Usage](#usage)
- [Data Visualization](#data-visualization)
- [Applications](#applications)
- [Contributing](#contributing)
- [License](#license)

## Introduction

Monitoring environmental parameters such as humidity, temperature, pH, air quality, and greenhouse gas emissions is crucial for optimizing crop growth and ensuring sustainable farming practices. This project uses the Microchip LoRa Technology MOTE (RN2483) to collect and transmit data wirelessly over long distances using LoRaWAN.

## Components

- Microchip LoRa Technology MOTE (RN2483)
- DHT22 Sensor (Temperature and Humidity)
- pH Sensor
- MQ-135 Sensor (Air Quality)
- Breadboard and Jumper Wires
- Power Supply

## Setup

### Hardware Connections

#### DHT22 Sensor
- VCC to 3.3V or 5V
- GND to GND
- Data to digital pin D2

#### pH Sensor
- Follow the specific connection instructions for your pH sensor module.

#### MQ-135 Sensor
- VCC to 3.3V or 5V
- GND to GND
- Analog output to analog pin A0

### Software Configuration

1. Install [MPLAB X IDE](https://www.microchip.com/mplab/mplab-x-ide).
2. Install the [XC8 Compiler](https://www.microchip.com/mplab/compilers).
3. Clone this repository:
   ```sh
   git clone https://github.com/yourusername/agricultural-monitoring-lora-mote.git
   cd agricultural-monitoring-lora-mote
   ```

## Installation

1. Open MPLAB X IDE.
2. Create a new project and configure it for the specific PIC microcontroller on your Microchip LoRa Technology MOTE (RN2483).
3. Add the provided code (`main.c`) to your project.

## Usage

Upload the code to your Microchip LoRa Technology MOTE (RN2483) using MPLAB X IDE. The code initializes the sensors and sends the collected data via LoRaWAN.

### Example Code

```cpp
#include <Wire.h>
#include <DHT.h>
#include <SPI.h>
#include <LoRa.h>

// DHT Sensor
#define DHTPIN 2
#define DHTTYPE DHT22
DHT dht(DHTPIN, DHTTYPE);

// LoRa Settings
const int csPin = 10;
const int resetPin = 9;
const int irqPin = 2;

// pH Sensor
const int pHpin = A1;

// MQ-135 Sensor
const int airQualityPin = A0;

void setup() {
  Serial.begin(9600);
  dht.begin();

  // Initialize LoRa
  LoRa.setPins(csPin, resetPin, irqPin);
  if (!LoRa.begin(915E6)) { // Adjust frequency as needed
    Serial.println("Starting LoRa failed!");
    while (1);
  }
  Serial.println("LoRa Initializing OK!");
}

void loop() {
  // Read DHT22 sensor
  float humidity = dht.readHumidity();
  float temperature = dht.readTemperature();

  // Read pH sensor
  int pHValue = analogRead(pHpin);

  // Read air quality sensor (MQ-135)
  int airQuality = analogRead(airQualityPin);

  // Print sensor values
  Serial.print("Temperature: ");
  Serial.print(temperature);
  Serial.print(" °C, Humidity: ");
  Serial.print(humidity);
  Serial.print(" %, pH: ");
  Serial.print(pHValue);
  Serial.print(", Air Quality: ");
  Serial.println(airQuality);

  // Send data via LoRa
  LoRa.beginPacket();
  LoRa.print("Temp: ");
  LoRa.print(temperature);
  LoRa.print(" °C, Humidity: ");
  LoRa.print(humidity);
  LoRa.print(" %, pH: ");
  LoRa.print(pHValue);
  LoRa.print(", Air Quality: ");
  LoRa.println(airQuality);
  LoRa.endPacket();

  delay(2000); // Delay 2 seconds before next reading
}
```

## Data Visualization

To visualize and analyze the collected data, we can use platforms like [ThingSpeak](https://thingspeak.com/) or other IoT cloud services. These platforms provide tools for real-time data monitoring, historical data analysis, and alert generation based on predefined conditions.

## Applications

Deploying this setup in an agricultural field allows continuous monitoring of environmental conditions, leading to data-driven decisions to optimize crop growth. Potential applications include:

- Soil Health Monitoring: Tracking pH levels and moisture content.
- Climate Control in Greenhouses: Maintaining ideal growing conditions.
- Air Quality Management: Identifying pollutants and protecting crops.
- Greenhouse Gas Emissions: Understanding and mitigating the impact on crop growth and environmental health.

## Contributing

Contributions are welcome! Please fork this repository and submit a pull request with your improvements or bug fixes.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
```
