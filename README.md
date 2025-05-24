# 🌞 Solar Panel Tracking System | Single Axis (Arduino + Servo + LDRs)

An automated **Single-Axis Solar Panel Tracking System** built using **Arduino Uno**, **Servo Motor**, and **LDR sensors** to optimize solar energy collection by continuously adjusting the panel’s angle based on sunlight direction throughout the day.

---

## 📌 Project Description

This project improves the efficiency of a solar panel by automatically adjusting its position in the **horizontal (left-right)** direction, following the sun’s movement from east to west. The system uses **four LDR sensors** to detect light intensity and a **single servo motor** to rotate the panel toward the direction of maximum sunlight.

---

## ✨ Features

- 📡 Real-time solar tracking using LDR sensors.
- 🛠️ Single-axis horizontal movement via servo motor.
- ⚡ Up to **30% improvement in energy output** compared to a fixed panel.
- 💻 Open-source Arduino code, easy to modify.
- 🔋 Compact, lightweight, and affordable for DIY/academic projects.

---

## 🛠️ Hardware Components

- 1 × Arduino Uno  
- 1 × Servo Motor (MG996R recommended)  
- 4 × LDR Sensors  
- 4 × 10kΩ Resistors  
- 1 × Mini Solar Panel  
- Jumper Wires, Breadboard  
- MDF/Plastic frame for mounting  

---

## 📖 Working Principle

- **4 LDR sensors** are placed around the solar panel (2 on each side).
- The **Arduino reads light intensity** values from the sensors.
- It compares the average values of the left and right sensor pairs.
- The panel rotates **left or right using a servo motor** to face the brighter side.
- The process runs continuously, optimizing energy absorption throughout the day.

---

## 📊 Testing and Observations

| Time       | Fixed Panel Voltage (V) | Tracking Panel Voltage (V) | Efficiency Gain (%) |
|------------|:------------------------|:---------------------------|:--------------------|
| 10:00 AM   | 3.10                     | 4.20                        | 26                   |
| 12:00 PM   | 3.90                     | 5.10                        | 24                   |
| 2:00 PM    | 3.20                     | 4.50                        | 29                   |
| 4:00 PM    | 2.80                     | 3.80                        | 26                   |

**Average efficiency improvement:** **26-30%**

---

## 👨‍💻 Arduino Code

```cpp

#include <Servo.h>
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

#define LDR1 A0
#define LDR2 A1
#define error 30

int Spoint = 90;

Servo servo;

void setup() {
  lcd.init();
  lcd.backlight();
  servo.attach(11);
  servo.write(Spoint);
  Serial.begin(9600);

  //welcome message
  lcd.setCursor(0, 0);
  lcd.print("Single - Axis");
  lcd.setCursor(0, 1);
  lcd.print("Solar Tracker");
  delay(4000);  // display for 3 seconds
  lcd.clear();
}

void loop() {
  int ldr1 = readAverage(LDR1);
  int ldr2 = readAverage(LDR2);

  int diff = abs(ldr1 - ldr2);

  Serial.print("LDR1: ");
  Serial.print(ldr1);
  Serial.print(" | LDR2: ");
  Serial.print(ldr2);
  Serial.print(" | Diff: ");
  Serial.println(diff);

  if (diff > error) {
    if (ldr1 > ldr2 && Spoint > 0) {
      Spoint = Spoint - 3;
    }
    if (ldr1 < ldr2 && Spoint < 180) {
      Spoint = Spoint + 3;
    }
    servo.write(Spoint);
    delay(30);

    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Angle of Panel:");
    lcd.setCursor(0, 1);
    lcd.print(Spoint);
  } 
  else {
    lcd.clear();
    lcd.setCursor(0, 0);
    lcd.print("Attained");
    lcd.setCursor(0, 1);
    lcd.print("Equilibrium.");
  }

  delay(300);  // safe refresh rate
}

int readAverage(int pin) {
  int total = 0;
  for (int i = 0; i < 10; i++) {
    total += analogRead(pin);
    delay(2);
  }
  return total / 10;
}


```
## Future Improvements

Implement dual-axis tracking for vertical (up-down) movement.
Integrate IoT for remote monitoring and data logging.
Add automatic night reset to reposition panel horizontally after sunset.
Include rain and wind sensors for automatic protection.
Use a solar charge controller and battery backup system.
