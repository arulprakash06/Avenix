<div align="center">

# 🏆 RUSHHOUR 2026
## 24-Hour National Engineering Challenge

<img src="https://readme-typing-svg.herokuapp.com?font=Poppins&weight=700&size=28&duration=3500&pause=1200&color=8A2BE2&center=true&vCenter=true&width=900&lines=National+Engineering+Hackathon;Innovate+%E2%80%A2+Build+%E2%80%A2+Solve+%E2%80%A2+Impact;Team+Avenix;IGNIS-BOT" />

<br>

<img src="https://img.shields.io/badge/Hackathon-RushHour%202026-8A2BE2?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Duration-24%20Hours-blueviolet?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Team-Avenix-purple?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Category-AI%20%7C%20IoT%20%7C%20Robotics-success?style=for-the-badge"/>

</div>

---

# 🏆 About RushHour

**RushHour 2026** is a **24-Hour National Engineering Challenge** that brings together passionate innovators, developers, and engineers from across the country to solve real-world problems through technology.

The hackathon provides a platform for students to transform innovative ideas into impactful solutions within a limited time. Participants collaborate as teams to design, develop, test, and present working prototypes that address practical challenges using emerging technologies.

Our team participated with the goal of building an intelligent robotic system capable of assisting firefighters and improving emergency response using **Artificial Intelligence**, **Embedded Systems**, and **Computer Vision**.

---

# 👥 Team Name

# **Avenix**

> *Engineering intelligent solutions for a safer tomorrow.*

---

# 🤖 Project Name

# **IGNIS BOT**

---

# 📖 Project Description

The **IGNIS BOT** is an intelligent robotic system developed to detect, monitor, and extinguish fire with minimal human intervention.

The robot combines **Artificial Intelligence**, **Computer Vision**, **Embedded Systems**, and **IoT** technologies to improve firefighting efficiency while reducing the risks faced by emergency responders.

Using an **ESP32-CAM** integrated with an **Edge Impulse Object Detection Model**, the robot continuously monitors its surroundings for fire. It intelligently navigates through its environment by avoiding obstacles using ultrasonic sensing and confirms fire using flame sensors.

Once a fire is detected, the robot automatically aligns a **servo-controlled water nozzle** toward the source and activates a **water pump** to extinguish the fire. The system also provides **live video streaming** and supports both **manual** and **autonomous** operation through a web-based interface, enabling users to monitor and control the robot remotely.

Our solution demonstrates how AI and robotics can work together to create safer, faster, and smarter emergency response systems.

---

# 👨‍💻 Team Details

| Team Information | Details |
|-----------------|---------|
| 🏷 Team Name | **Avenix** |
| 🏆 Hackathon | RushHour 2026 |
| 🏫 Institution | Meenakshi Sundararajan Engineering College |
| 👥 Team Size | 6 Members |

## Team Members

| No | Name | Role |
|----|------|------|
| 01 | Dharaneesh SA | Team Leader |
| 02 | Gokula Kannan S | Team Member |
| 03 | Arul Prakash S | Team Member |
| 04 | Kumaravelu S | Team Member |
| 05 | Akhilesh L | Team Member |
| 06 | Muhammad Thoufik Ansari N | Team Member |

---

# ❗ Problem Statement

Fire accidents can spread rapidly, causing severe damage to property and posing a serious risk to human lives.

Existing low-cost firefighting systems have limitations in autonomous fire detection, remote monitoring, intelligent navigation, and efficient fire suppression. These limitations often require firefighters to enter dangerous environments, increasing the risk to human life.

Our project addresses this challenge by developing an **AI-Based Autonomous Fire Suppression Robot** capable of detecting fire using computer vision, avoiding obstacles, providing live video streaming, supporting both autonomous and manual operation, and automatically extinguishing fire using a water spray mechanism.

---

# 💡 Proposed Solution

The proposed solution is an **AI-Based Autonomous Fire Suppression Robot** that integrates Artificial Intelligence, Computer Vision, Embedded Systems, and IoT technologies into a single intelligent platform.

The robot uses an **ESP32-CAM** equipped with an **Edge Impulse Object Detection Model** to detect fire in real time through computer vision. While navigating autonomously, it continuously monitors its surroundings and avoids obstacles using an ultrasonic sensor. Flame sensors provide an additional layer of verification to accurately confirm the presence of fire.

Once a fire is confirmed, a **servo-controlled water nozzle** automatically rotates toward the fire source, and a **water pump** is activated to extinguish the flames.

To enhance usability, the robot provides **live video streaming** and a **web-based control interface**, allowing users to remotely monitor and operate the system in either **manual** or **autonomous** mode.

By combining intelligent fire detection, autonomous navigation, obstacle avoidance, remote monitoring, and automated fire suppression, the proposed solution improves firefighter safety, reduces response time, and minimizes damage to both human life and property.

---

<div align="center">

### 🚀 Built with Passion by Team Avenix

**RushHour 2026 | AI • Robotics • IoT • Embedded Systems**

⭐ *Thank you for visiting our project repository!*

</div>
<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/44c9b45b-a7de-42ed-873d-7736d79d36e0" />

<<<OUR CODE>>>

#include <WiFi.h>
#include <WebServer.h>

// --- Wi-Fi Configuration ---
const char *ssid = "SIST-Hackathon-2026";
const char *password = "sbu123!@#";

// --- Pin Mapping (ESP32 DevKit V1) ---
// L298N Motor Driver
#define ENA 13
#define IN1 27
#define IN2 26
#define ENB 14
#define IN3 25
#define IN4 33

// HC-SR04 Ultrasonic Sensor
#define TRIG_PIN 5
#define ECHO_PIN 18

// Flame Sensors (Digital Outputs)
#define FLAME_LEFT   34
#define FLAME_CENTER 35
#define FLAME_RIGHT  32

// --- Robot State Variables ---
bool autoMode = false;
String botStatus = "Stopped";

WebServer server(80);

// --- Motor Speed Control ---
void setMotorSpeed(int speed) {
  analogWrite(ENA, speed);
  analogWrite(ENB, speed);
}

// --- Direction Commands ---
void stopMotors() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
  botStatus = "Stopped";
}

void moveForward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  botStatus = "Moving Forward";
}

void turnLeft() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
  botStatus = "Turning Left";
}

void turnRight() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
  botStatus = "Turning Right";
}

// --- Sensor Functions ---
long getDistanceCM() {
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  
  long duration = pulseIn(ECHO_PIN, HIGH, 30000); // 30ms timeout
  if (duration == 0) return 999;
  return duration * 0.034 / 2;
}

bool isFireDetected() {
  // Flame sensors output LOW when flame is detected
  return (digitalRead(FLAME_LEFT) == LOW || 
          digitalRead(FLAME_CENTER) == LOW || 
          digitalRead(FLAME_RIGHT) == LOW);
}

// --- Auto Mode Handler ---
void handleAutoMode() {
  if (isFireDetected()) {
    stopMotors();
    botStatus = "🔥 FIRE DETECTED!";
    return;
  }

  long distance = getDistanceCM();
  if (distance < 20) { // Object closer than 20cm
    stopMotors();
    delay(200);
    turnRight(); // Obstacle dodge maneuver
    delay(400);
  } else {
    moveForward();
    botStatus = "Auto Navigating...";
  }
}

// --- Built-in Web Page HTML/CSS/JS ---
const char HTTP_WEBPAGE[] PROGMEM = R"rawliteral(
<!DOCTYPE html>
<html>
<head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ESP32 DevKit V1 Bot</title>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; background: #121212; color: #ffffff; margin: 0; padding: 20px; }
        h2 { color: #00e676; margin-bottom: 5px; }
        .status { font-size: 18px; margin: 15px 0; font-weight: bold; color: #ffeb3b; }
        .mode-box { margin-bottom: 20px; }
        .mode-btn { padding: 12px 24px; font-size: 16px; font-weight: bold; border: none; border-radius: 8px; cursor: pointer; margin: 5px; }
        .btn-manual { background: #2196f3; color: white; }
        .btn-auto { background: #ff9800; color: white; }
        .grid { display: grid; grid-template-columns: repeat(3, 80px); grid-gap: 12px; justify-content: center; }
        .btn { width: 80px; height: 80px; background: #333; color: white; font-size: 24px; font-weight: bold; border: 2px solid #555; border-radius: 12px; touch-action: manipulation; }
        .btn:active { background: #555; }
        .stop { background: #f44336; border: none; }
        .empty { visibility: hidden; }
    </style>
</head>
<body>
    <h2>ESP32 Robot Control</h2>
    <div class="status" id="bot-status">Status: Initializing...</div>

    <div class="mode-box">
        <button class="mode-btn btn-manual" onclick="sendCommand('manual')">Manual Mode</button>
        <button class="mode-btn btn-auto" onclick="sendCommand('auto')">Auto Mode</button>
    </div>

    <div class="grid">
        <div class="empty"></div>
        <button class="btn" onclick="sendCommand('forward')">▲</button>
        <div class="empty"></div>
        
        <button class="btn" onclick="sendCommand('left')">◀</button>
        <button class="btn stop" onclick="sendCommand('stop')">■</button>
        <button class="btn" onclick="sendCommand('right')">▶</button>
    </div>

    <script>
        function sendCommand(cmd) {
            fetch('/' + cmd);
        }
        setInterval(() => {
            fetch('/status').then(res => res.text()).then(txt => {
                document.getElementById('bot-status').innerText = "Status: " + txt;
            });
        }, 1000);
    </script>
</body>
</html>
)rawliteral";

// --- Server Routes Setup ---
void setupServerRoutes() {
  server.on("/", HTTP_GET, []() {
    server.send(200, "text/html", HTTP_WEBPAGE);
  });

  server.on("/manual", HTTP_GET, []() {
    autoMode = false;
    stopMotors();
    server.send(200, "text/plain", "OK");
  });

  server.on("/auto", HTTP_GET, []() {
    autoMode = true;
    server.send(200, "text/plain", "OK");
  });

  server.on("/forward", HTTP_GET, []() { if(!autoMode) moveForward(); server.send(200, "text/plain", "OK"); });
  server.on("/left", HTTP_GET, []() { if(!autoMode) turnLeft(); server.send(200, "text/plain", "OK"); });
  server.on("/right", HTTP_GET, []() { if(!autoMode) turnRight(); server.send(200, "text/plain", "OK"); });
  server.on("/stop", HTTP_GET, []() { if(!autoMode) stopMotors(); server.send(200, "text/plain", "OK"); });

  server.on("/status", HTTP_GET, []() {
    server.send(200, "text/plain", autoMode ? "AUTO MODE - " + botStatus : "MANUAL MODE - " + botStatus);
  });
}

void setup() {
  Serial.begin(115200);

  // Configure Motor Control Pins
  pinMode(ENA, OUTPUT);
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(ENB, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  setMotorSpeed(200); // Set default PWM motor speed (0-255)
  stopMotors();

  // Configure Sensor Pins
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);

  pinMode(FLAME_LEFT, INPUT);
  pinMode(FLAME_CENTER, INPUT);
  pinMode(FLAME_RIGHT, INPUT);

  // Start Wi-Fi Access Point
  WiFi.softAP(ssid, password);
  Serial.println("\n--- WiFi Access Point Started ---");
  Serial.print("IP Address: ");
  Serial.println(WiFi.softAPIP());

  // Start Web Server
  setupServerRoutes();
  server.begin();
  Serial.println("Web server running.");
}

void loop() {
  server.handleClient();

  if (autoMode) {
    handleAutoMode();
  }
}

<img width="1062" height="727" alt="WhatsApp Image 2026-07-24 at 4 38 28 PM" src="https://github.com/user-attachments/assets/a52eca10-6866-46c3-9127-d6d3ebb3a7d1" />


<div align="center">

# 🚨 ZEROTH HOUR 🚨

### 🔥 *Adaptive Fire Source Localization & Intelligent Path Planning Engine*

<img src="https://readme-typing-svg.demolab.com?font=Poppins&weight=700&size=28&duration=2500&pause=1000&color=FF3B3B&center=true&vCenter=true&width=900&lines=🚒+Detect+.+Locate+.+Navigate.;🧠+AI-Powered+Fire+Localization.;📡+Sensor+Fusion+Without+Extra+Hardware.;⚡+Real-Time+Dynamic+Path+Planning.;🎯+Autonomous+Nozzle+Targeting.;🔥+ZEROTH+HOUR" alt="Typing SVG" />

<br>

<img src="https://img.shields.io/badge/Hackathon-Innovation-red?style=for-the-badge"/>
<img src="https://img.shields.io/badge/AI-Enabled-orange?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Real--Time-Navigation-blue?style=for-the-badge"/>
<img src="https://img.shields.io/badge/Status-Prototype-success?style=for-the-badge"/>

<br><br>

<img src="https://capsule-render.vercel.app/api?type=waving&color=ff4d4d,ff9900&height=220&section=header&text=ZEROTH%20HOUR&fontSize=60&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=Adaptive%20Fire%20Source%20Localization%20%7C%20AI%20Path%20Planning&descAlignY=58"/>

</div>

---

## 🎯 Motto

> **"Every Second Counts. Every Decision Saves Lives."**

---


