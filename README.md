# 🤖 Chiku – The Waving Robot

**Chiku** is a small Arduino-based robot that waves its hand when you come close or greet it!  
Built using an Ultrasonic Sensor (HC-SR04) and a Servo Motor, this project uses simple sensors to simulate friendly interaction — inspired by WALL·E 🌿.

---

## 🧩 Components Used
- Arduino UNO
- Ultrasonic Sensor (HC-SR04)
- SG90 Micro Servo Motor
- Jumper Wires
- Breadboard
- (Optional) LED for indication

---

## ⚙️ Circuit Connections
| Component | Arduino Pin |
|------------|-------------|
| Ultrasonic Sensor TRIG | D9 |
| Ultrasonic Sensor ECHO | D10 |
| Servo Signal | D6 |
| LED | D7 |
| Power (5V, GND) | Common rails on breadboard |

![Circuit Diagram](circuit_diagram.png)

---

## 💻 Arduino Code
File: `chiku_wave_robot.ino`

```cpp
#include <Servo.h>

Servo myservo;
int trigPin = 9;
int echoPin = 10;
int ledPin = 7;

long duration;
int distance;

void setup() {
  myservo.attach(6);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  Serial.print("Distance: ");
  Serial.println(distance);

  if (distance < 20) {
    digitalWrite(ledPin, HIGH);
    waveServo();
  } else {
    digitalWrite(ledPin, LOW);
    myservo.write(90);
  }

  delay(200);
}

void waveServo() {
  for (int i = 60; i <= 120; i++) {
    myservo.write(i);
    delay(15);
  }
  for (int i = 120; i >= 60; i--) {
    myservo.write(i);
    delay(15);
  }
}
