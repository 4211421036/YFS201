# YFS201
---

# YFS201 Water Flow Sensor Module

## Table of Contents
1. [Introduction](#introduction)
2. [Features](#features)
3. [Installation](#installation)
4. [Usage](#usage)
5. [Mathematical Formulation](#mathematical-formulation)
6. [Examples](#examples)
7. [Caution](#caution)
8. [Important Notes](#important-notes)
9. [Warning](#warning)
10. [License](#license)

---

## Introduction
The **YFS201 Water Flow Sensor Module** is an Arduino-compatible library designed to interface with the YF-S201 water flow sensor. This sensor measures the flow rate of water using the Hall Effect principle, generating pulses proportional to the flow rate. This library provides an easy-to-use interface to read the flow rate, calculate the total volume of water, and integrate with other components like LCDs, relays, or SD cards.

---

## Features
- **Flow Rate Measurement**: Accurately measure the flow rate of water in liters per minute (L/min).
- **Total Volume Calculation**: Calculate the total volume of water that has passed through the sensor.
- **Pulse Counting**: Count the number of pulses generated by the sensor to determine flow rate.
- **Compatibility**: Works seamlessly with Arduino boards and other compatible microcontrollers.
- **Extensible**: Easily integrate with other libraries for LCDs, SD cards, or IoT platforms.

---

## Installation
To use the YFS201 library, follow these steps:

1. Download the library as a ZIP file or clone the repository.
2. Open the Arduino IDE.
3. Go to `Sketch` > `Include Library` > `Add .ZIP Library` and select the downloaded file.
4. Include the library in your sketch:
   ```cpp
   #include "yfs201.h"
   ```

---

## Usage
### Basic Setup
1. Connect the YF-S201 sensor to your Arduino:
   - **VCC** to `5V`
   - **GND** to `GND`
   - **Signal** to a digital pin (e.g., `D2`).
2. Initialize the sensor in your sketch:
   ```cpp
   const int flowSensorPin = 2; // Pin connected to the sensor
   yfs201 flowSensor(flowSensorPin);

   void setup() {
       Serial.begin(9600);
       flowSensor.begin();
   }
   ```

3. Read the flow rate and total volume in the `loop()` function:
   ```cpp
   void loop() {
       float flowRate = flowSensor.getFlowRate();
       unsigned long totalPulses = flowSensor.getTotalPulses();

       Serial.print("Flow Rate: ");
       Serial.print(flowRate);
       Serial.println(" L/min");

       Serial.print("Total Pulses: ");
       Serial.println(totalPulses);

       delay(1000); // Wait 1 second before reading again
   }
   ```

---

## Mathematical Formulation
The flow rate $$Q$$ (in liters per minute) is calculated using the following formula:

$$
Q = \frac{f \times 60}{K}
$$

Where:
- $$Q$$: Flow rate in liters per minute (L/min).
- $$f$$: Frequency of pulses in Hertz (Hz).
- $$K$$: Sensor calibration factor (typically 7.5 for YF-S201).

The total volume $$V$$ (in liters) is calculated by integrating the flow rate over time:

$$
V = \int_{0}^{t} Q \, dt
$$

In the code, this is approximated as:

$$
V = \sum_{i=1}^{n} \left( \frac{Q_i \times \Delta t_i}{60} \right)
$$

Where:
- $$Q_i$$: Flow rate at time interval \( i \).
- $$\Delta t_i$$: Time elapsed since the last measurement (in milliseconds).

---

## Examples
### Example 1: Basic Flow Rate Measurement
This example reads the flow rate and displays it on the Serial Monitor.

```cpp
#include "yfs201.h"

const int flowSensorPin = 2;
yfs201 flowSensor(flowSensorPin);

void setup() {
    Serial.begin(9600);
    flowSensor.begin();
}

void loop() {
    float flowRate = flowSensor.getFlowRate();
    Serial.print("Flow Rate: ");
    Serial.print(flowRate);
    Serial.println(" L/min");
    delay(1000);
}
```

### Example 2: Total Volume Calculation
This example calculates and displays the total volume of water that has passed through the sensor.

```cpp
#include "yfs201.h"

const int flowSensorPin = 2;
yfs201 flowSensor(flowSensorPin);

float totalVolume = 0;
unsigned long lastTime = 0;

void setup() {
    Serial.begin(9600);
    flowSensor.begin();
    lastTime = millis();
}

void loop() {
    unsigned long currentTime = millis();
    unsigned long elapsedTime = currentTime - lastTime;
    lastTime = currentTime;

    float flowRate = flowSensor.getFlowRate();
    float volumeIncrement = (flowRate * elapsedTime) / 60000.0;

    totalVolume += volumeIncrement;

    Serial.print("Total Volume: ");
    Serial.print(totalVolume);
    Serial.println(" L");

    delay(1000);
}
```

---

> [!Caution]
> - Ensure the sensor is properly connected to avoid damage to the Arduino or sensor.
> - Do not exceed the maximum flow rate specified in the sensor's datasheet (typically 30 L/min for YF-S201).

---

> [!Important]
> - The calibration factor $$K$$ may vary depending on the specific sensor. Always calibrate the sensor for accurate measurements.
> - The sensor is not suitable for measuring non-conductive fluids.

---

## Warning
- **Electrical Safety**: Always disconnect power before making changes to the circuit.
- **Water Safety**: Avoid exposing the sensor to water temperatures above its rated limit (typically 60°C).

---

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---
