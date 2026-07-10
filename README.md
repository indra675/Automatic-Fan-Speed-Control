# Automatic Fan Speed Control Using Temperature Sensor

An embedded systems project built with Arduino UNO and a TMP36 analog temperature sensor, implementing proportional (PWM-based) fan speed control based on real-time temperature readings.

## 📋 Overview

Traditional cooling systems rely on simple ON/OFF thermostatic switches, causing abrupt starts/stops and inefficient power use. This project uses a microcontroller-based closed-loop approach: the TMP36 sensor feeds analog temperature data to the Arduino, which computes a PWM duty cycle and drives a DC fan through a transistor switching stage — giving three distinct speed levels (OFF / Medium / Full).

## 🔧 Hardware Used

- Arduino UNO R3
- TMP36 Analog Temperature Sensor
- NPN Transistor (2N2222 or equivalent)
- 5V DC Fan/Motor
- 1N4001 Flyback Diode
- 1kΩ Resistor
- Breadboard + Jumper Wires

## ⚙️ How It Works

1. **Sense** – TMP36 outputs a voltage proportional to temperature (10mV/°C)
2. **Convert** – Arduino's ADC reads the analog signal (0–1023)
3. **Decide** – Temperature compared against thresholds (23°C / 33°C)
4. **Actuate** – PWM signal drives fan speed via transistor switching

| Temperature | Fan State | PWM Value | Duty Cycle |
|---|---|---|---|
| Below 23°C | OFF | 0 | 0% |
| 23°C – 33°C | Medium | 128 | ~50% |
| ≥ 33°C | Full Speed | 255 | 100% |

## 💻 Code

```cpp
const int tempPin = A0;
const int fanPin = 9;
int tempC;
int pwmValue;

void setup() {
  pinMode(fanPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  int sensorValue = analogRead(tempPin);
  tempC = sensorValue * 0.488;

  if (tempC < 23) {
    pwmValue = 0;
  }
  else if (tempC >= 23 && tempC < 33) {
    pwmValue = 128;
  }
  else {
    pwmValue = 255;
  }

  analogWrite(fanPin, pwmValue);
  Serial.print("Temperature: ");
  Serial.print(tempC);
  Serial.print(" C | PWM: ");
  Serial.println(pwmValue);
  delay(500);
}
```

## 📊 Simulation Results (Tinkercad)

| Temperature | Expected PWM | Observed PWM | Fan Speed (RPM) | Result |
|---|---|---|---|---|
| 9°C | 0 | 0 | 0 rpm | ✅ Fan OFF |
| 25°C | 128 | 128 | 724 rpm | ✅ Medium speed |
| 44°C | 255 | 255 | 1432 rpm | ✅ Full speed |

## 🧪 Simulation Tool

Built and validated using **Autodesk Tinkercad Circuits**.

## 🚀 Future Scope

- Continuous PWM mapping using `map()` for smoother transitions
- Hysteresis to prevent speed "chattering" near thresholds
- LCD/OLED display for standalone readout
- IoT integration (ESP32 + MQTT/cloud dashboard)
- RPM feedback sensor for closed-loop speed control
- TMP36 offset calibration for improved accuracy


# 👤 Author

**Galla Indranag**
B.Tech ECE, NRI Institute of Technology, Guntur
[LinkedIn](https://linkedin.com/in/gallaindranag) | [GitHub](https://github.com/indra675)
