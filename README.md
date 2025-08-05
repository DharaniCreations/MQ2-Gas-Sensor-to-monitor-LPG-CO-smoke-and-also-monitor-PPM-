# 🔥 Arduino Gas Leak Detector using MQ-2 Sensor & I2C LCD

A simple and effective gas leakage detection system using an **MQ-2 gas sensor**, **Arduino**, and an **I2C LCD display**. The project monitors gas concentration (LPG, CO, smoke, etc.) and provides real-time display of values with alert capability.

---

## 📌 Features

- Detects gases like **LPG**, **carbon monoxide**, **butane**, and **smoke**
- Displays real-time gas levels on an **I2C LCD (16x2)**
- Optional buzzer or LED alert system when unsafe levels are detected
- Expandable and beginner-friendly

---

## 🧰 Bill of Materials

| Component            | Quantity |
|----------------------|----------|
| Arduino Uno/Nano     | 1        |
| MQ-2 Gas Sensor      | 1        |
| I2C 16x2 LCD Display | 1        |
| Potentiometer (10k)  | 1 (optional for contrast) |
| Breadboard + Jumper Wires | As needed |
| Buzzer / LED (Optional) | 1       |

---

## 🔌 Circuit Connections

### MQ-2 Sensor:
- VCC → 5V  
- GND → GND  
- AOUT → A0  

### I2C LCD:
- VCC → 5V  
- GND → GND  
- SDA → A4 (for Uno)  
- SCL → A5 (for Uno)  

> Note: I2C address is typically `0x27` or `0x3F`. Use an I2C scanner if unsure.

---

## 💻 Arduino Code

```cpp
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2); // Change address if needed
const int gasPin = A0;
const int threshold = 300;

void setup() {
  Serial.begin(9600);
  lcd.init();
  lcd.backlight();
  lcd.setCursor(0, 0);
  lcd.print("Gas Monitor Init");
  delay(2000);
}

void loop() {
  int gasLevel = analogRead(gasPin);
  Serial.println(gasLevel);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Gas Level:");
  lcd.setCursor(0, 1);
  lcd.print(gasLevel);

  if (gasLevel > threshold) {
    lcd.setCursor(10, 0);
    lcd.print("ALERT!");
    // digitalWrite(buzzerPin, HIGH); // Optional
  }

  delay(500);
}
