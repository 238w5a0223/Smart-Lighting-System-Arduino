# IoT-based control system using ESP32 and cloud platforms.
Smart Temperature Automation System with LCD, Fan & Buzzer
🔧 Components Used
ESP32 Development Board
(Main microcontroller with built-in WiFi for IoT communication)
Relay Module (4-Channel / Single Channel)
(Used to control AC/DC loads like fan, motor, etc.)
LEDs (Light Emitting Diodes)
(For indication and basic output control)
Resistors (220Ω / 330Ω)
(Current limiting for LEDs)
Jumper Wires
(For circuit connections)
Breadboard
(For prototyping and assembling the circuit)
5V Power Supply / USB Cable
(To power the ESP32 and connected components)
DC Loads
5V DC Motor
5V DC Fan
WiFi Network (Router/Hotspot)
(Required for ESP32 internet connectivity)

![Circuit](C:\Users\kusum\OneDrive\Desktop\TASK - 1.png)
Code : 
#define BLYNK_PRINT Serial

#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

// ----------- WiFi Credentials -----------
char ssid[] = "YOUR_WIFI_NAME";
char pass[] = "YOUR_WIFI_PASSWORD";

// ----------- Blynk Auth Token -----------
char auth[] = "YOUR_BLYNK_AUTH_TOKEN";

// ----------- Relay Pins -----------
#define RELAY1 26   // Motor
#define RELAY2 27   // Fan

// ----------- LED Pin -----------
#define LED 2

// ----------- Variables -----------
bool autoMode = false;

// ----------- Blynk Virtual Pins -----------
// V0 -> Motor Control
// V1 -> Fan Control
// V2 -> LED Control
// V3 -> Auto Mode Toggle

// ----------- Motor Control -----------
BLYNK_WRITE(V0) {
  int value = param.asInt();
  if (!autoMode) {
    digitalWrite(RELAY1, value);
  }
}

// ----------- Fan Control -----------
BLYNK_WRITE(V1) {
  int value = param.asInt();
  if (!autoMode) {
    digitalWrite(RELAY2, value);
  }
}

// ----------- LED Control -----------
BLYNK_WRITE(V2) {
  int value = param.asInt();
  digitalWrite(LED, value);
}

// ----------- Auto Mode Toggle -----------
BLYNK_WRITE(V3) {
  autoMode = param.asInt();
}

// ----------- Setup -----------
void setup() {
  Serial.begin(115200);

  pinMode(RELAY1, OUTPUT);
  pinMode(RELAY2, OUTPUT);
  pinMode(LED, OUTPUT);

  digitalWrite(RELAY1, LOW);
  digitalWrite(RELAY2, LOW);
  digitalWrite(LED, LOW);

  // Connect to WiFi & Blynk
  Blynk.begin(auth, ssid, pass);

  Serial.println("System Ready...");
}

// ----------- Automation Logic -----------
void automationLogic() {
  if (autoMode) {
    // Example logic: alternate ON/OFF every 5 seconds
    digitalWrite(RELAY1, HIGH);
    digitalWrite(RELAY2, LOW);
    delay(5000);

    digitalWrite(RELAY1, LOW);
    digitalWrite(RELAY2, HIGH);
    delay(5000);
  }
}

// ----------- Loop -----------
void loop() {
  Blynk.run();

  automationLogic();
}
