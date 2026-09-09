# Automatic Parking Indicator

An Arduino-based smart parking distance indicator designed and simulated in Tinkercad. The system uses an **HC-SR04 Ultrasonic Sensor** to detect the proximity of an approaching vehicle and provides real-time visual feedback using colored LEDs (Green, Yellow, Red) to guide safe parking.


## 🚗 Features
* **Real-time Proximity Sensing:** Accurately measures the distance of an object using ultrasonic sound waves.
* **Visual Distance Feedback:**
  * 🟢 **Green LED:** Safe distance — vehicle is clear to move forward.
  * 🟡 **Yellow LED:** Caution distance — vehicle is getting close.
  * 🔴 **Red LED:** Stop distance — vehicle is at the minimum safe margin.
* **Tinkercad Simulation:** Fully designed and tested virtually before hardware assembly.

---

## 🛠️ Components Used
* **Microcontroller:** Arduino Uno R3
* **Sensor:** HC-SR04 Ultrasonic Sensor
* **Indicators:** 
  * 1x Green LED
  * 1x Yellow LED
  * 1x Red LED
* **Resistors:** 3x 220Ω Resistors (for LEDs)
* **Breadboard & Jumper Wires**

---

## 📐 Circuit Diagram & Simulation

You can view and run the live simulation directly on Tinkercad:
👉 **[View Tinkercad Project](https://www.tinkercad.com/things/9rpAWR6VOQB-automatic-parking-indicator)**

![Circuit Diagram](circuit_diagram.png) *(Note: Ensure you upload your screenshot named `circuit_diagram.png` to the repository root directory)*

---

## 🔌 Pin Connections

| Component | Pin / Terminal | Arduino Pin |
| :--- | :--- | :--- |
| **HC-SR04 Sensor** | VCC | 5V |
| | GND | GND |
| | Trig | Pin 9 |
| | Echo | Pin 10 |
| **Green LED** | Anode (+) | Pin 2 (via 220Ω resistor) |
| **Yellow LED** | Anode (+) | Pin 3 (via 220Ω resistor) |
| **Red LED** | Anode (+) | Pin 4 (via 220Ω resistor) |

---

## 💻 Arduino Source Code

```cpp
// Pin Definitions
const int trigPin = 9;
const int echoPin = 10;
const int greenLed = 2;
const int yellowLed = 3;
const int redLed = 4;

// Variables
long duration;
int distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  
  pinMode(greenLed, OUTPUT);
  pinMode(yellowLed, OUTPUT);
  pinMode(redLed, OUTPUT);
  
  Serial.begin(9600);
}

void loop() {
  // Clear trigPin
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  
  // Send 10µs pulse
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  // Read echoPin
  duration = pulseIn(echoPin, HIGH);
  
  // Calculate distance in centimeters
  distance = duration * 0.034 / 2;
  
  // Print distance to Serial Monitor
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");
  
  // LED Logic based on distance threshold
  if (distance > 30) {
    // Safe Range
    digitalWrite(greenLed, HIGH);
    digitalWrite(yellowLed, LOW);
    digitalWrite(redLed, LOW);
  } 
  else if (distance <= 30 && distance > 10) {
    // Caution Range
    digitalWrite(greenLed, LOW);
    digitalWrite(yellowLed, HIGH);
    digitalWrite(redLed, LOW);
  } 
  else {
    // Stop Range
    digitalWrite(greenLed, LOW);
    digitalWrite(yellowLed, LOW);
    digitalWrite(redLed, HIGH);
  }
  
  delay(100);
}


