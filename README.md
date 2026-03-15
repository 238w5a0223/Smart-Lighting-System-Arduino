# IoT Cloud Integration and Real-Time Remote Monitoring Using ESP32:
Smart Temperature Automation System with LCD, Fan & Buzzer
Components Used
ESP32 Microcontroller
Sensor (Temperature / Light / Motion)
Breadboard
Jumper wires
USB cable
WiFi connection
Arduino IDE
ThingSpeak IoT cloud platform
Wokwi ESP32 simulator (optional)

![Circuit](C:\Users\kusum\OneDrive\Desktop\TASK - 1.png)
Code : 
#include <WiFi.h>
#include <HTTPClient.h>

// WiFi credentials
const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";

// ThingSpeak API Key
String apiKey = "YOUR_THINGSPEAK_WRITE_API_KEY";

const char* server = "http://api.thingspeak.com/update";

int sensorPin = 34;   // Analog sensor connected to GPIO 34
int sensorValue;

void setup() {
  Serial.begin(115200);

  // Connect to WiFi
  WiFi.begin(ssid, password);
  Serial.print("Connecting to WiFi");

  while (WiFi.status() != WL_CONNECTED) {
    delay(1000);
    Serial.print(".");
  }

  Serial.println("\nWiFi Connected!");
}

void loop() {

  // Read sensor value
  sensorValue = analogRead(sensorPin);

  Serial.print("Sensor Value: ");
  Serial.println(sensorValue);

  // Send data to ThingSpeak
  if (WiFi.status() == WL_CONNECTED) {

    HTTPClient http;

    String url = server;
    url += "?api_key=" + apiKey;
    url += "&field1=" + String(sensorValue);

    http.begin(url);

    int httpResponseCode = http.GET();

    if (httpResponseCode > 0) {
      Serial.print("Data sent. Response code: ");
      Serial.println(httpResponseCode);
    } 
    else {
      Serial.print("Error sending data: ");
      Serial.println(httpResponseCode);
    }

    http.end();
  }

  // ThingSpeak requires minimum 15 seconds delay
  delay(15000);
}
