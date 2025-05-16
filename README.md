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

Servo myservo;
int ldrLeft1 = A0;
int ldrLeft2 = A1;
int ldrRight1 = A2;
int ldrRight2 = A3;

int pos = 90;

void setup() {
  myservo.attach(9);
  myservo.write(pos);
  pinMode(ldrLeft1, INPUT);
  pinMode(ldrLeft2, INPUT);
  pinMode(ldrRight1, INPUT);
  pinMode(ldrRight2, INPUT);
  Serial.begin(9600);
}

void loop() {
  int leftValue = (analogRead(ldrLeft1) + analogRead(ldrLeft2)) / 2;
  int rightValue = (analogRead(ldrRight1) + analogRead(ldrRight2)) / 2;

  int diff = leftValue - rightValue;

  if (diff > 50) {
    if (pos < 170) pos += 1;
    myservo.write(pos);
    delay(50);
  } else if (diff < -50) {
    if (pos > 10) pos -= 1;
    myservo.write(pos);
    delay(50);
  }

  Serial.print("Left: "); Serial.print(leftValue);
  Serial.print(" | Right: "); Serial.println(rightValue);
  delay(500);
}

```
## Future Improvements

Implement dual-axis tracking for vertical (up-down) movement.
Integrate IoT for remote monitoring and data logging.
Add automatic night reset to reposition panel horizontally after sunset.
Include rain and wind sensors for automatic protection.
Use a solar charge controller and battery backup system.
